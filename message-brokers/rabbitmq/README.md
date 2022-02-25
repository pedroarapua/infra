# Install 
helm repo add bitnami https://charts.bitnami.com/bitnami
kubectl create namespace rabbitmq
helm install rabbitmq \
  --set auth.username=admin,auth.password=admin,auth.erlangCookie=secretcookie \
    bitnami/rabbitmq --namespace rabbitmq

# To Access the RabbitMQ AMQP port:

    echo "URL : amqp://127.0.0.1:5672/"
    kubectl port-forward --namespace rabbitmq svc/rabbitmq 5672:5672

# To Access the RabbitMQ Management interface:

    echo "URL : http://127.0.0.1:15672/"
    kubectl port-forward --namespace rabbitmq svc/rabbitmq 15672:15672

# Uninstall
helm uninstall rabbitmq -n rabbitmq
