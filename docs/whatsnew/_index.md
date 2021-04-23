---
title: What's new in One Eye
shorttitle: What's new
weight: 15
---

## Release 0.5 (2021-04-19) {#oe05}

One Eye allows you to attach multiple peer clusters to a central observer cluster to make your metrics available centrally, and store in object storage in a cost and storage-efficient way. One Eye automates several steps of connecting and maintaining the peer clusters, making it possible to operate your monitoring infrastructure with a low overhead. For details on how One Eye collects metrics from multiple clusters, see {{% xref "/docs/one-eye/multicluster/_index.md" %}}.

In version 0.5, you can secure One Eye access using an external ingress, and authenticate and authorize user connections. The solution is based on [Pomerium](https://github.com/pomerium/pomerium) and [Dex](https://github.com/dexidp/dex), allowing you to use the identity provider of your choice (for example, GitHub or LDAP). You can also apply role based authorization, and assign read-only or read and write access to One Eye.

## Release 0.4 (2020-12-03) {#oe04}

One Eye 0.4 brings the following significant new features:

- You can now easily restore archived logs into an Elasticsearch instance. For details, see {{% xref "/docs/one-eye/logs-events/log-restoration/index.md" %}}.
- Search and display the collected metrics using the Prometheus PromQL query language directly on the One Eye interface. For details, see ({{< xref "/docs/one-eye/logs-events/metrics/index.md" >}}).
- Promtail support.
- Fluentd (1.11.5), Fluent Bit (1.6.4), and plugin updates.
- Easier troubleshooting of [One Eye]({{< relref "/docs/one-eye/troubleshooting/index.md#explain" >}}) and the [Logging operator]({{< relref "/docs/one-eye/logging-operator/operation/troubleshooting/_index.md" >}}).

For a more detailed overview of the new features, see the [One Eye 0.4 announcement blog post]({{< blogref "one-eye-release-0.4/index.md" >}}).

## Release 0.3 (2020-06-25) {#oe03}

For an overview of the new features, see the [One Eye 0.3 announcement blog post]({{< blogref "one-eye-updates/index.md" >}}).
