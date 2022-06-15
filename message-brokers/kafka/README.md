helm repo add bitnami https://charts.bitnami.com/bitnami
kubectl create namespace kafka
# Install with Credentials
helm install kafka \
    --set auth.clientProtocol=sasl \
    --set auth.sasl.jaas.clientUsers[0]=teste \
    --set auth.sasl.jaas.clientPasswords[0]=teste \
    bitnami/kafka -n kafka

## copy credentials to the pod
kubectl cp --namespace kafka ./client.properties kafka-client:/tmp/client.properties
kubectl cp --namespace kafka ./kafka_jaas.conf kafka-client:/tmp/kafka_jaas.conf

# Install without Credentials
helm install kafka \
    bitnami/kafka -n kafka

# Run Producer

## connect pod
kubectl exec --tty -i kafka-client --namespace kafka -- bash

## run producer + credentials
export KAFKA_OPTS="-Djava.security.auth.login.config=/tmp/kafka_jaas.conf"
cd /opt/bitnami/kafka/bin && ./kafka-console-producer.sh \
    --producer.config /tmp/client.properties \
    --broker-list kafka.kafka.svc.cluster.local:9092 \
    --topic teste


## run producer
cd /opt/bitnami/kafka/bin && ./kafka-console-producer.sh \
    --broker-list kafka.kafka.svc.cluster.local:9092 \
    --topic teste

# Run Consumer

## connect pod
kubectl exec --tty -i kafka-client --namespace kafka -- bash

## run consumer + credentials
export KAFKA_OPTS="-Djava.security.auth.login.config=/tmp/kafka_jaas.conf"
cd /opt/bitnami/kafka/bin && ./kafka-console-consumer.sh \
    --consumer.config /tmp/client.properties \
    --group consumer-group \
    --bootstrap-server kafka.kafka.svc.cluster.local:9092 \
    --topic teste

## run consumer
cd /opt/bitnami/kafka/bin && ./kafka-console-consumer.sh \
    --group consumer-group \
    --bootstrap-server kafka.kafka.svc.cluster.local:9092 \
    --topic teste

# Check Partition Quantity

## connect pod
kubectl exec --tty -i kafka-client --namespace kafka -- bash

## check partitions + credentials
export KAFKA_OPTS="-Djava.security.auth.login.config=/tmp/kafka_jaas.conf"
cd /opt/bitnami/kafka/bin && ./kafka-topics.sh \
    --command-config /tmp/client.properties \
    --bootstrap-server kafka.kafka.svc.cluster.local:9092 \
    --describe \
    --topic teste

## check partitions
cd /opt/bitnami/kafka/bin && ./kafka-topics.sh \
    --bootstrap-server kafka.kafka.svc.cluster.local:9092 \
    --describe \
    --topic teste


# Uninstall
helm uninstall kafka -n kafka
