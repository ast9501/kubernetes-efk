# kubernetes-efk
EFK Kubernetes manifests

## Introduction
The repo provide manifests for:
* Elasticsearch (v7.5.0) (in YAML)
* Kibana (v7.5.0) (in YAML)
* Fluent bit (2.0.8) (in Helm Charts version 0.23.0) 

### Environment
>Cluster build from kubespray project
* Kubernetes v1.25.6
* Containerd 1.6.15
* Ubuntu 20.04
* Calico CNI

## Elasticsearch
* Configure `nfs-provisioner` to provide storageClass (skip)
* Edit `elasticsearch/es-tst.yaml` `storageClassName` field
* Deploy
```
kubectl apply -f elasticsearch/
```
## Kibana
```
kubectl apply -f kibana/
```
## Fluent bit
```
helm install fluentbit .
```

### Values
The `INPUT`, `OUTPUT`, `PARSER` will be set by values.yaml
The following is the modification from original charts:
modify on [OUTPUT]:
```
...
        Host ${FLUENT_ELASTICSEARCH_HOST}
        Port ${FLUENT_ELASTICSEARCH_PORT}
        Index my_index
        Replace_Dots On
        Type _doc
...
```

use environment variable to parse elasticsearch server info:
```
...
env:
    - name: FLUENT_ELASTICSEARCH_HOST
      value: "elasticsearch.default"
    - name: FLUENT_ELASTICSEARCH_PORT
      value: "9200"
...
```
