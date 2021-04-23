---
title: Multicluster metrics
weight: 700
---

One Eye allows you to attach multiple **peer clusters** to a central **observer cluster** to make your metrics available centrally, and if needed, store them in object storage in a cost and storage-efficient way. One Eye builds upon [Thanos](https://thanos.io) and our open source [Thanos operator](https://github.com/banzaicloud/thanos-operator/) to collect and manage multicluster metrics.

The observer cluster runs an entire Thanos deployment. The peer clusters require only the Thanos operator that takes care of configuring and managing the components needed for the observer cluster to collect the metrics from the peer. The observer accesses the peer cluster through an ingress controller. The connection between the clusters is encrypted using mTLS, so trust must be established between the observer and peer clusters (for example, using a shared certificate).

<!-- FIXME figure -->

If you need to recreate the observer cluster, you can simply deploy the related resources to the new cluster, it will automatically start collecting the metrics from the peers.

{{< include-headless "multicluster-metrics-limitations.md" "one-eye" >}}

To configure One Eye to collect metrics from a peer cluster, see {{% xref "/docs/one-eye/quickstart/multicluster.md" %}}.
