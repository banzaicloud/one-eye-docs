---
title: Detach peer cluster
weight: 200
---

To detach the peer cluster from the observer, delete the related custom resources. Complete the following steps.

1. Switch to the Kubernetes context of the observer cluster.
1. List the ThanosPeers custom resources on the cluster.

    ```bash
    kubectl list ThanosPeers
    ```

1. Delete the custom resource of the peer cluster you want to detach.
1. Switch to the Kubernetes context of the peer cluster.
1. List the ThanosEndpoint custom resources on the cluster.

    ```bash
    kubectl list ThanosEndpoints
    ```

1. Delete the custom resource from the peer cluster.