# Elastic Cloud on Kubernetes Helm chart

Helm chart to deploy [ECK operator](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-overview.html).

Based on the official Elastic [resource file](https://download.elastic.co/downloads/eck/1.0.1/all-in-one.yaml).

## Usage

Once installed you can create `Elasticsearch` clusters using custom resources:

```YAML
apiVersion: elasticsearch.k8s.elastic.co/v1
kind: Elasticsearch
metadata:
  name: my-es-custer
spec:
  version: 6.8.4
  nodeSets:
  - name: my-group
    count: 1
    config:
      node.master: true
      node.data: true
      node.ingest: true
      node.store.allow_mmap: false
```

More info can be found on the offical [quickstart](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-quickstart.html).