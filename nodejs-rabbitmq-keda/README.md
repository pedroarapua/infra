# Install
kubectl create namespace nodejs-rabbitmq-keda
kubectl apply --filename ./nodejs-producer-worker/deployment.yaml --namespace nodejs-rabbitmq-keda
kubectl apply --filename ./nodejs-consumer-worker/deployment.yaml --namespace nodejs-rabbitmq-keda