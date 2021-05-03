---
title: Multicluster setup
weight: 200
---

The following procedure shows you how to collect metrics from a peer cluster to an existing One Eye deployment. The cluster running the One Eye deployment is called the **observer cluster**, the new cluster you are collecting metrics from is called the **peer cluster**. For details on how One Eye collects metrics from multiple clusters, see {{% xref "/docs/one-eye/multicluster/_index.md" %}}.

## Prerequisites

- You have completed the prerequisites described in {{% xref "/docs/one-eye/quickstart/_index.md#prerequisites" %}}.
- You have another cluster that will be the peer cluster.

{{< include-headless "warning-prometheus-label.md" "one-eye" >}}

{{< include-headless "multicluster-metrics-limitations.md" "one-eye" >}}

## Steps

> Note: The following procedure requires you to switch between the Kubernetes context of the observer and the peer cluster. A convenient way to switch between contexts is to use the [kubectx tool](https://github.com/ahmetb/kubectx).

<!-- FIXME: Include a kubectx-less command? -->
1. Deploy One Eye on the observer cluster. Run the **one-eye install** command, then follow the on-screen instructions.

    > Note: The interactive installer helps you configure a simple logging system, this is detailed in {{% xref "/docs/one-eye/quickstart/_index.md#deploy-with-logging" %}}. To collect only metrics, you don't have to configure logging on the cluster. In this case, just set the name of the cluster, then reply **No** to the subsequent questions.

1. Obtain the name of the peer cluster, for example, by getting it from the current context of the peer's kubeconfig.

    On the peer cluster, run the following command.

    ```bash
    kubectx $PEER_CONTEXT
    export PEER_ENDPOINT=$(kubectl config current-context | cut -d '@' -f 2)
    ```

1. Switch to the observer cluster.

    ```bash
    kubectx $OBSERVER_CONTEXT
    ```

1. If not already installed, install cert-manager and thanos-operator components on the observer cluster.

    ```bash
    one-eye cert-manager install --update
    one-eye thanos install --operator-only --update
    ```

1. Create a secret that will be used to establish trust between the observer and the peer clusters. The following steps show you how to use the same certificate on both clusters.
    <!-- FIXME: Mention other possibilities
    > pepov mentioned that secrets from an external PKI like vault can be used - how?
    > what if the clusters are in a service mesh? -->

    1. Create a self-signed CA and a certificate.

        ```bash
        cat <<EOF | kubectl apply -f-
        apiVersion: cert-manager.io/v1
        kind: Issuer
        metadata:
          name: selfsigned
        spec:
          selfSigned: {}
        EOF
        ```

    1. Create a Kubernetes secret for the certificate with proper labels (**monitoring.banzaicloud.io/thanospeer** and **monitoring.banzaicloud.io/thanospeer-ca**). The labels are needed so the Thanos operator can find the secret automatically and attach it to the ThanosPeer custom resource.

        ```bash
        cat <<EOF | kubectl apply -f-
        apiVersion: v1
        kind: Secret
        metadata:
          name: ${PEER_ENDPOINT}-tls
          labels:
            monitoring.banzaicloud.io/thanospeer: ${PEER_ENDPOINT}
            monitoring.banzaicloud.io/thanospeer-ca: ${PEER_ENDPOINT}
        type: kubernetes.io/tls
        data:
          tls.crt: ""
          tls.key: ""
          ca.crt: ""
        EOF
        ```

    1. Create the certificate that will be used on both clusters.

        ```bash
        cat <<EOF | kubectl apply -f-
        apiVersion: cert-manager.io/v1
        kind: Certificate
        metadata:
          name: ${PEER_ENDPOINT}-tls
        spec:
          secretName: ${PEER_ENDPOINT}-tls
          commonName: peer-endpoint.cluster.notld
          dnsNames:
          - $PEER_ENDPOINT
          issuerRef:
            name: selfsigned
          usages:
          - server auth
          - client auth
        EOF
        ```

    1. Save the secret so you can load it into the peer later (for example, into the file called ${PEER_ENDPOINT}-tls.yaml).

        ```bash
        kubectl get secret ${PEER_ENDPOINT}-tls -o yaml > ${PEER_ENDPOINT}-tls.yaml
        ```

    1. Switch to the peer cluster.

        ```bash
        kubectx $PEER_CONTEXT
        ```

    1. If not already installed, install Prometheus and the Thanos operator on the peer cluster.

        {{< include-headless "warning-prometheus-label.md" "one-eye" >}}

        ```bash
        one-eye prometheus install --update
        one-eye thanos install --operator-only --update
        ```

    1. Install the ingress controller.

        ```bash
        one-eye ingress install --update
        ```

    1. Apply the saved secret.

        ```bash
        kubectl apply -f ${PEER_ENDPOINT}-tls.yaml
        ```

1. Connect the peer cluster to the observer cluster.

    1. On the peer cluster, create the ThanosEndpoint custom resource.

        ```bash
        one-eye thanos endpoint generate $PEER_ENDPOINT --cert-secret-name ${PEER_ENDPOINT}-tls --ca-bundle-secret-name ${PEER_ENDPOINT}-tls | kubectl apply -f-
        ```

    1. Generate the ThanosPeer custom resource for the cluster and save it (for example, into the thanos-peer.yaml file).

        ```bash
        one-eye thanos peer generate --name $PEER_ENDPOINT --wait-poll-interval 5  > thanos-peer.yaml
        ```

    1. Switch to the observer cluster.

        ```bash
        kubectx $OBSERVER_CONTEXT
        ```

    1. Apply the generated ThanosPeer custom resource to the observer cluster.

        ```bash
        kubectl apply -f thanos-peer.yaml
        ```

1. Verify that the metrics of the peer cluster are stored to the observer cluster. Run the following commands:

    ```bash
    kubectl port-forward svc/${PEER_ENDPOINT}-peer-query 10902:10902

    open localhost:10902/stores
    ```

    Select **Stores** on the Thanos web interface:

    ![Thanos stores UI](../thanos-stores-ui.png)