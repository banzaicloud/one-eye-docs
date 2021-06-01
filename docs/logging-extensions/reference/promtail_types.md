---
title: Promtail
weight: 200
generated_file: true
---

## PromtailSpec

PromtailSpec defines the desired state of Promtail

### namespace (string, required) {#promtailspec-namespace}

The resources of Promtail will be placed into this namespace<br>

Default: -

### enableRecreateWorkloadOnImmutableFieldChange (bool, optional) {#promtailspec-enablerecreateworkloadonimmutablefieldchange}

EnableRecreateWorkloadOnImmutableFieldChange enables the operator to recreate the<br>daemonset (and possibly other resource in the future) in case there is a change in an immutable field<br>that otherwise couldn't be managed with a simple update.<br>

Default: -

### workloadMetaOverrides (*types.MetaBase, optional) {#promtailspec-workloadmetaoverrides}

Override metadata of the created resources<br>

Default: -

### workloadOverrides (*types.PodSpecBase, optional) {#promtailspec-workloadoverrides}

Override podSpec fields for the given daemonset<br>

Default: -

### containerOverrides (*types.ContainerBase, optional) {#promtailspec-containeroverrides}

Override container fields for the given statefulset<br>

Default: -

### containerRuntime (string, optional) {#promtailspec-containerruntime}

Container Runtime  (docker, containerd)<br>

Default: -

### pipelineStages ([]string, optional) {#promtailspec-pipelinestages}

PipelineStages  (docker, cri)<br>

Default: -

### lokiUrl (string, optional) {#promtailspec-lokiurl}

Loki URL http://loki:3100/loki/api/v1/push<br>

Default: -

### security (*Security, optional) {#promtailspec-security}

Security defines promtail deployment security properties<br>

Default: -

### tolerations ([]corev1.Toleration, optional) {#promtailspec-tolerations}

Default: -

### nodeSelector (map[string]string, optional) {#promtailspec-nodeselector}

Default: -

### affinity (*corev1.Affinity, optional) {#promtailspec-affinity}

Default: -

### podPriorityClassName (string, optional) {#promtailspec-podpriorityclassname}

Default: -

### resources (corev1.ResourceRequirements, optional) {#promtailspec-resources}

Default: -

### image (ImageSpec, optional) {#promtailspec-image}

Default: -


## PromtailStatus

PromtailStatus defines the observed state of Promtail


## Promtail

Promtail is the Schema for the promtails API

###  (metav1.TypeMeta, required) {#promtail-}

Default: -

### metadata (metav1.ObjectMeta, optional) {#promtail-metadata}

Default: -

### spec (PromtailSpec, optional) {#promtail-spec}

Default: -

### status (PromtailStatus, optional) {#promtail-status}

Default: -


## PromtailList

PromtailList contains a list of Promtail

###  (metav1.TypeMeta, required) {#promtaillist-}

Default: -

### metadata (metav1.ListMeta, optional) {#promtaillist-metadata}

Default: -

### items ([]Promtail, required) {#promtaillist-items}

Default: -


## Security

Security defines promtail deployment security properties

### serviceAccount (string, optional) {#security-serviceaccount}

Default: -

### roleBasedAccessControlCreate (*bool, optional) {#security-rolebasedaccesscontrolcreate}

Default: -

### podSecurityPolicyCreate (bool, optional) {#security-podsecuritypolicycreate}

Default: -

### securityContext (*corev1.SecurityContext, optional) {#security-securitycontext}

Default: -

### podSecurityContext (*corev1.PodSecurityContext, optional) {#security-podsecuritycontext}

Default: -


## ImageSpec

ImageSpec struct hold information about image specification

### repository (string, optional) {#imagespec-repository}

Default: -

### tag (string, optional) {#imagespec-tag}

Default: -

### pullPolicy (string, optional) {#imagespec-pullpolicy}

Default: -


