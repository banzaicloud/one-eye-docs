---
title: Prometheus Sharding
shorttitle: Prometheus Sharding
weight: 500
aliases:
    - /docs/one-eye/prometheus-sharding/
---

To enable Prometheus Sharding via One Eye just modify the Observer.

To search and review the collected metrics, complete the following steps.

1. Edit the Observer


    ```bash
    kubectl edit observer one-eye
    ```


1. Set the parameters

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
      values: '{"prometheus":{"prometheusSpec":{"shards":"3","replicas":"2"}}}}'

}
```

1. Apply the configchange

```bash
go run cmd/one-eye/main.go observer reconcile
```

> EXPERIMENTAL: Number of shards to distribute targets onto. Number of replicas multiplied by shards is the total number of Pods created. Note that scaling down shards will not reshard data onto remaining instances, it must be manually moved. Increasing shards will not reshard data either but it will continue to be available from the same instances. To query globally use Thanos sidecar and Thanos querier or remote write data to a central location. Sharding is done on the content of the `__address__` target meta-label.