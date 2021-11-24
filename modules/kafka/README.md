$$ install helm in vm
see https://helm.sh/docs/intro/install/

```sh
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter> curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/ram@LAPTOP-QSM1JPGC/scripts/get-helm-3
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter> chmod 700 get_helm.sh
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter> ./get_helm.sh0
Downloading https://get.helm.sh/helm-v3.6.3-linux-amd64.tar.gz
Verifying checksum... Done.
Preparing to install helm into /usr/local/bin
helm installed into /usr/local/bin/helm
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter> helm version
version.BuildInfo{Version:"v3.6.3", GitCommit:"d506314abfb5d21419df8c7e7e68012379db2354", GitTreeState:"clean", GoVersion:"go1.16.5"}
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter> 
```

$$ install kafka in kubernetes via helm
see https://bitnami.com/stack/kafka/helm
https://www.ignitesol.com/how-to-deploy-kafka-connect-on-kubernetes-using-helm-charts/
```
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter $ helm repo list
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/ram/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /home/ram/.kube/config
Error: no repositories to show
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter $ helm repo add bitnami https://charts.bitnami.com/bitnami
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/ram/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /home/ram/.kube/config
"bitnami" has been added to your repositories
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter $ helm repo list
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/ram/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /home/ram/.kube/config
NAME   	URL                               
bitnami	https://charts.bitnami.com/bitnami

ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter $ helm search repo kafka

WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/ram/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /home/ram/.kube/config


NAME                            CHART VERSION   APP VERSION     DESCRIPTION                                       
bitnami/kafka                   14.4.1          2.8.1           Apache Kafka is a distributed streaming platform. 
bitnami/dataplatform-bp1        9.0.0           1.0.1           OCTO Data platform Kafka-Spark-Solr Helm Chart    
bitnami/dataplatform-bp2        10.0.0          1.0.1           OCTO Data platform Kafka-Spark-Elasticsearch He...

ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter $ export KUBECONFIG=${HOME}/.kube/config
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter$ helm install my-release bitnami/kafka
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/ram/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /home/ram/.kube/config
NAME: my-release
LAST DEPLOYED: Tue Nov 23 19:35:24 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: kafka
CHART VERSION: 14.4.1
APP VERSION: 2.8.1

** Please be patient while the chart is being deployed **

Kafka can be accessed by consumers via port 9092 on the following DNS name from within your cluster:

    my-release-kafka.default.svc.cluster.local

Each Kafka broker can be accessed by producers via port 9092 on the following DNS name(s) from within your cluster:

    my-release-kafka-0.my-release-kafka-headless.default.svc.cluster.local:9092

To create a pod that you can use as a Kafka client run the following commands:

    kubectl run my-release-kafka-client --restart='Never' --image docker.io/bitnami/kafka:2.8.1-debian-10-r31 --namespace default --command -- sleep infinity
    kubectl exec --tty -i my-release-kafka-client --namespace default -- bash

    PRODUCER:
        kafka-console-producer.sh \
            
            --broker-list my-release-kafka-0.my-release-kafka-headless.default.svc.cluster.local:9092 \
            --topic test

    CONSUMER:
        kafka-console-consumer.sh \
            
            --bootstrap-server my-release-kafka.default.svc.cluster.local:9092 \
            --topic test \
            --from-beginning

```

verify

```
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter $ kubectl get pods
NAME                             READY   STATUS        RESTARTS   AGE
my-release-kafka-0               1/1     Running       0          14m
my-release-zookeeper-0           1/1     Running       0          14m
udaconnect-api-89dbffbf9-mcvr5   1/1     Terminating   1          3h36m
ram@LAPTOP-QSM1JPGC:~/udacity/nd064-c2-message-passing-projects-starter $ 
```





