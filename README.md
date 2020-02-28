# Elastic Cloud on Kubernetes Helm chart

Helm chart to deploy [ECK operator](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-overview.html).

This chart is based on the official Elastic [resource file](https://download.elastic.co/downloads/eck/1.0.1/all-in-one.yaml).

__DISCLAIMER__: This Helm chart is provided as-is and is an alpha version.

## TL;DR;

```console
$ helm install <repo>/elastic-operator
```

## Prerequisites

- Kubernetes 1.12+ or OpenShift 3.11+ 

See [ECK overview](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-overview.html).

## Installing the Chart

To install the chart with the release name `my-release` and default configuration:

```console
$ helm install --name my-release <repo>/elastic-operator
```

## Uninstalling the Chart

To delete the chart:

```console
$ helm delete my-release
```

## Configuration

The following table lists the configurable parameters of the elastic-operator chart and their default values.

Parameter | Description | Default
--- | --- | ---
`replicaCount` | Number of replicas | `1`
`image.repository` | Image repository | `docker.elastic.co/eck/eck-operator`
`image.tag` | Image tag | `1.0.1`
`image.pullPolicy` | Image pull policy | `IfNotPresent`
`imagePullSecrets` | Image pull secrets | `[]`
`nameOverride` | Override the name of the chart | `""`
`fullnameOverride` | Override the fullname of the chart | `""`
`labels` | Extra labels to add to statefulset | `{}`
`annotations` | Extra annotations to add to statefulset | `{}`
`podLabels` | Extra labels to add to pod | `{}`
`podAnnotations` | Extra annotations to add to pod | `{}`
`operator.logVerbosity` | Verbosity level of logs. -2=Error, -1=Warn, 0=Info, 0 and above=Debug. See [Documentation](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-operator-config.html) | `0`
`operator.metricsPort` | If configured, will expose Prometheus metrics. See [Documentation](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-operator-config.html) | `nil`
`operator.roles` | Roles this operator should assume. Valid values are namespace, global, webhook or all. Accepts multiple space separated values. See [Documentation](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-operator-config.html) | `all`
`webhook.enabled` | If `true`, creates the validating webhook. Make sure to also configure properly the `operator.roles` also. See [Documentation](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-webhook.html) | `true`
`serviceAccount.create` | If `true`, a new service account is created | `true`
`serviceAccount.name` | The service account to be created/used | `nil`
`rbac.create` | If `true`, will create rbac for the `serviceAccount.name` | `true`
`podSecurityContext` | Security context for the entire pod | `{}`
`securityContext` | Security context for containers running in the pod | `{}`
`resources` | Pod resource requests and limits | `{}`
`nodeSelector` | Node labels for pod assignment | `{}`
`tolerations` | Node taints to tolerate | `[]`
`affinity` | Pod affinity | `{}`

Specify each parameter you'd like to override using a YAML file as described above in the [installation](#installing-the-chart) section or by using the `--set key=value[,key=value]` argument to `helm install`.

```console
$ helm install <repo>/elastic-operator --name my-release --values values.yaml
```

## Creating resources

Once installed you can create Elastic resources. For example, creating an `Elasticsearch` cluster:

```YAML
apiVersion: elasticsearch.k8s.elastic.co/v1
kind: Elasticsearch
metadata:
  name: my-es-custer
spec:
  version: 7.6.0
  nodeSets:
  - name: my-group
    count: 1
    config:
      node.master: true
      node.data: true
      node.ingest: true
      node.store.allow_mmap: false
```

A more complete example can be found [here](./samples/elasticsearch.yaml).
More info can be found on the offical [ECK quickstart](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-quickstart.html).