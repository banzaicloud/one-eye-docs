---
title: Customize Dex configuration
weight: 300
---

To customize the Dex configuration of your One Eye installation, complete the following steps.

## Prerequisites

You must have a working One Eye installation. [Dex supports several different authentication backends](https://dexidp.io/docs/connectors/), the following example uses [GitHub authentication]({{< relref "/docs/one-eye/authentication/_index.md" >}}).

## Steps

1. Prepare a customized Dex configuration snippet.

    > Note: One Eye installs Dex using the [official Dex Helm chart](https://github.com/dexidp/helm-charts/tree/dex-0.0.7/charts/dex). For details on the Dex parameters you can configure, see the [list of Dex configuration values](https://github.com/dexidp/helm-charts/tree/dex-0.0.7/charts/dex#values), and the [official Dex documentation](https://dexidp.io/docs/connectors/).

    ```bash
    configyaml="
    config:
      connectors:
      - config:
          clientID: <github-client-id>
          clientSecret: <github-client-secret>
          redirectURI: https://dex.<pomerium-root-domain>/dex/callback
          loadAllGroups: true
          useLoginAsID: true
        type: github
        id:   github
        name: Github
    "
    ```

1. Create a secret using the configuration snippet from the previous step to use with Dex. Adjust the namespace if needed to match the namespace Dex is installed in:

    ```bash
    kubectl apply -f -<<EOF
    apiVersion: v1
    kind: Secret
    metadata:
      name: <name-of-dex-secret>
      namespace: default
    data:
      config: $(echo "$configyaml" | base64)
    EOF
    ```

1. Check that the secret is successfully created with the proper configuration.

    ```bash
    kubectl get secret <name-of-dex-secret> -o json | jq -r '.data | map_values(@base64d)["config"]'
    ```

1. Edit the One Eye Observer custom resource (called one-eye in the example) to use the newly created secret for Dex.

    ```bash
    kubectl patch observer one-eye --type merge -p "
    spec:
      dex:
        dexchart:
          valuesSecret:
            key: config
            name: <name-of-dex-secret>"
    ```

1. Edit your One Eye Observer custom resource (called one-eye in the example) and set a groupRoleMapping to give editor privileges to a GitHub group.

    ```bash
    kubectl patch observer one-eye --type merge -p "
    spec:
      authorization:
        groupRoleMapping:
          <one-eye-editors-github-group>:  editor"
    ```
