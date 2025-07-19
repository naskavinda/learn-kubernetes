### Commands

# Create a deployment from the deployment.yml file
kubectl create -f deployment.yml

# Get the status of the specific deployment
kubectl get deployment/myapp-deployment

# (Optional) Get all resources in the current namespace
(kubectl get all)

# Apply changes in deployment.yml to the existing deployment
kubectl apply -f deployment.yml

# Show detailed information about the deployment
kubectl describe deployment/myapp-deployment

# Delete the deployment
kubectl delete deployment/myapp-deployment

