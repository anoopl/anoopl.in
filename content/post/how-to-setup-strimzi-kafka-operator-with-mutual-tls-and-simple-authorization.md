
# How to Setup Strimzi Kafka Operator with Mutual TLS and Simple Authorization

How to Setup Strimzi Kafka Operator with Mutual TLS and Simple Authorization

This assumes that you have setup a Strimzi Kafka Operator. You can follow the links to install it on Openshift/Kubernetes:

[https://strimzi.io/quickstarts/](https://strimzi.io/quickstarts/)

Detailed setup: [https://strimzi.io/docs/master/full.html#getting-started-str](https://strimzi.io/docs/master/full.html#getting-started-str)

1. Once you have the Kafka Operator up and running. Create a Kafka Cluster using following yaml:

[https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-persistent.yaml](https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-persistent.yaml)

kubectl apply -f [https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-persistent.yaml](https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-persistent.yaml)

Wait for a few minutes so that Kafka and Zookeeper pods are in ready state

![Kafka and Zookeeper pods Running](https://cdn-images-1.medium.com/max/2880/1*AaBHRgl65b1SEBcxkgD1nA.png)*Kafka and Zookeeper pods Running*

This creates a Kafka Cluster named my-cluster with TLS authentication and Simple Authorization enabled. This creates and Kubernetes Service with LoadBalancer which can be used to access the Kafka cluster outside Kubernetes cluster.

You can get the Load Balancer IP by listing the Services in the namespace:

![](https://cdn-images-1.medium.com/max/2866/1*LwrsC4QyMJAkesQEKpDrnw.png)

Use the “my-cluster-kafka-external-bootstrap” Service’s External IP to access your Kafka Cluster from outside.

2. Create a Kafka Topic using the operator with following yaml:

[https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-topic.yaml](https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-topic.yaml)

kubectl apply -f [https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-topic.yaml](https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-topic.yaml)

3. Create a Kafka User with Authentication TLS & Simple Authorization

[https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-user.yaml](https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-user.yaml)

kubectl apply -f [https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-user.yaml](https://raw.githubusercontent.com/anoopl/strimzi-kafka-operator-authn-authz/master/kafka-user.yaml)

4. Download the Cluster CA Cert and PKCS12 ( .p12) keys of User to use with Kafka Client:

kubectl get secret my-cluster-cluster-ca-cert -o jsonpath=’{.data.ca\.crt}’ | base64 -d > ca.crt

kubectl get secret my-user -o jsonpath=’{.data.user\.password}’ | base64 -d > user.password

kubectl get secret my-user -o jsonpath=’{.data.user\.p12}’ | base64 -d > user.p12

5. Convert the ca.cert to truststore jks and user.p12 to Keystore.jks

Please note that use the password for keystore same as the password you got from the above step:

keytool -keystore user-truststore.jks -alias CARoot -import -file ca.crt

keytool -importkeystore -srckeystore user.p12 -srcstoretype pkcs12 -destkeystore user-keystore.jks -deststoretype jks

6. Use Kafkatool ( [http://www.kafkatool.com/](http://www.kafkatool.com/)) to connect to the Kafka cluster. Use the LoadBalance IP you got from describing service: my-cluster-kafka-external-bootstrap to connect to cluster.

![](https://cdn-images-1.medium.com/max/2288/1*x_ZByl0KnS9kYKBwMplNAg.png)

Use the JKS key path and password to connect:

![](https://cdn-images-1.medium.com/max/2296/1*GkMv2-Eb-FNWDgsfuKbGMg.png)

7. Now you can see your topics here:

![](https://cdn-images-1.medium.com/max/2288/1*FRadcIToQm4bEgUADtcLOA.png)

Yay!!!
