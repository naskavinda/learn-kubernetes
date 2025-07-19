### Commands

# Create a deployment from the deployment.yml file
kubectl create -f deployment.yml

# Get the status of the specific deployment
kubectl get deployment/myapp-deployment

# (Optional) Get all resources in the current namespace
(kubectl get all)

# Apply changes in deployment.yml to the existing deployment
kubectl apply -f deployment.yml

# Record the command for the rolling update (for history tracking)
kubectl apply -f deployment.yml --record

# Show the status of the rolling update for the deployment
kubectl rollout status deployment/myapp-deployment

# Check the history of rolling updates for the deployment
kubectl rollout history deployment/myapp-deployment

# Undo the last rolling update for the deployment
kubectl rollout undo deployment/myapp-deployment

# Show detailed information about the deployment
kubectl describe deployment/myapp-deployment

# Delete the deployment
kubectl delete deployment/myapp-deployment

