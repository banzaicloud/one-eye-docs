---
---
{{< warning >}}If Prometheus is already installed on the peer cluster, make sure that properly sets the **cluster label** for the collected metrics: the label must be present, and unique, otherwise the metrics of the different clusters become mixed up.
If One Eye has been installed on the peer cluster, make sure that the `spec.clusterName` field of the observer custom resource is different on the observer and the peer clusters. Latest version of `One Eye` (>=0.5.0) tries to detect it automatically from the context.
{{< /warning >}}
