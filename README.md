# EDA-using-Kafka
A demo architecture for Event Driven Communication Using Apache Kafka on Kubernetes

# install KubeDB

Get a license from [appscode license server](https://appscode.com/issue-license/). They provide 30 day trial license for testing. Make sure you provide `global.featureGates.Kafka=true` flag during installation with helm chart.

```bash
helm upgrade -i kubedb oci://ghcr.io/appscode-charts/kubedb \
            --version v2024.4.27 \
            --namespace kubedb --create-namespace \
            --set-file global.license=/home/raka/Downloads/kubedb-license-f70806bb-fe64-4121-9cd8-e7a772c17e08.txt \
            --set global.featureGates.Kafka=true
```

Deploy Kafka sample cluster with `Kafka` sample resource using [this](/kafka-cluster.yaml) yaml. This will create kafka broker pods, controller pods (kafka's inbuilt metadata and leader election mechanism), secrets containing cluser configurations and authentication secret (if not provided by user), service to communicate with clients and between brokers.

```bash
$ kubectl get pods,secrets,svc,kafka -n demo

NAME                     READY   STATUS    RESTARTS   AGE
pod/kafka-broker-0       1/1     Running   0          6m22s
pod/kafka-broker-1       1/1     Running   0          3m55s
pod/kafka-broker-2       1/1     Running   0          3m50s
pod/kafka-controller-0   1/1     Running   0          6m21s
pod/kafka-controller-1   1/1     Running   0          3m54s

NAME                             TYPE                       DATA   AGE
secret/kafka-admin-cred          kubernetes.io/basic-auth   2      6m24s
secret/kafka-broker-config       Opaque                     2      6m24s
secret/kafka-controller-config   Opaque                     2      6m22s

NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                       AGE
service/kafka-pods   ClusterIP   None         <none>        9092/TCP,9093/TCP,29092/TCP   6m24s

NAME                     TYPE                  VERSION   STATUS   AGE
kafka.kubedb.com/kafka   kubedb.com/v1alpha2   3.6.1     Ready    6m24s
```

