---
title: Use Let's Encrypt
weight: 400
---

To use certificates issued by Let's Encrypt on the One Eye login page, complete the following steps.

1. Create a ClusterIssuer yaml file for cert-manager and edit it to suit your environment. One Eye will use this to issue certificates to Pomerium and Dex.
    1. Download the following sample ClusterIssuer custom resource and replace the name of the issuer and the administrator email address with valid values.

        {{< include-code "clusterissuer.yaml" "yaml" >}}

    1. Apply the ClusterIssuer:

        ```bash
        kubectl apply -f <clusterissuer.yaml>
        ```

    1. Check that it was created successfully.

        ```bash
        kubectl get clusterissuers.cert-manager.io
        ```

1. Edit the Observer custom resource of One Eye, set the **observer.spec.certmanager.issuer** field to the name of the ClusterIssuer, then apply your changes. On the running One Eye cluster, you can do that by running ```kubectl edit observer```.

    ```yaml
    spec:
      authorization: {}
      certmanager:
        certManagerOperatorChart: {}
        enabled: true
        namepace: cert-manager
        issuer: <name-of-the-clusterissuer>
    ```

1. Delete the old Pomerium secret.

    ```bash
    kubectl delete secret one-eye-pomerium-secret
    ```

1. Wait until cert-manager creates a new secret for Pomerium that uses a Let's Encrypt certificate.

    ```bash
    kubectl get certificate
    ```

    In the output, the READY field of ```one-eye-pomerium-ingress``` should be true.

1. Run the following command:

    ```bash
    one-eye pomerium install --update
    ```

    Wait until the reconciling is finished.

1. Open the external URL of the One Eye Dashboard in your browser, and verify that the certificate shown on the login screen is valid.