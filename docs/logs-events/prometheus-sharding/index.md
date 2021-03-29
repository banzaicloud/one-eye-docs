---
title: Prometheus Sharding
shorttitle: Prometheus Sharding
weight: 500
aliases:
    - /docs/one-eye/prometheus-sharding/
---

To enable Prometheus Sharding via One Eye, modify the Observer Custom Resource and set the number of shards to distribute targets onto.

{{< warning >}}
This is an **experimental** feature.
{{< /warning >}}

The total number of Pods created is the number of replicas multiplied by shards. Note that:

- Scaling down shards will not reshard data onto remaining instances, it must be manually moved.
- Increasing shards will not reshard data, the data will be available from the same instances.
- To query globally, use the Thanos sidecar and Thanos querier, or remote write data to a central location.
- Sharding is done on the content of the `__address__` target meta-label.

To search and review the collected metrics, complete the following steps.

1. Edit the Observer Custom Resource.

    ```bash
    kubectl edit observer one-eye
    ```

1. Set the parameters.

    ```json
    {
    apiVersion: one-eye.banzaicloud.io/v1alpha1
    kind: Observer
      name: one-eye
    spec:
      prometheus:
        enabled: true
        namespace: default
        prometheusOperatorChart:
          values: '{"prometheus":{"prometheusSpec":{"shards":"3","replicas":"2"}}}'

    }
    ```

1. Apply the configuration change.

    ```bash
    go run cmd/one-eye/main.go observer reconcile
    ```
