# Create namespace
kubectl create namespace kafka-keda

# Apply manifests credential none
kubectl apply --filename ./deployment-credential-none.yaml --namespace kafka-keda

# Apply manifests credential plaintext
kubectl apply --filename ./deployment-credential-plaintext.yaml --namespace kafka-keda