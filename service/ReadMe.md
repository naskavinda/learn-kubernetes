### Service Commands

# Create a service from the service-definition.yml file
kubectl create -f service-definition.yml

# Get the list of services
kubectl get service

# Get detailed information about a specific service
kubectl describe service my-service

# Delete the service
kubectl delete service my-service

# Expose a deployment as a service (example)
kubectl expose deployment myapp-deployment --type=NodePort --port=80

# Get endpoints associated with the service
kubectl get endpoints my-service

# List all resources (including services)
kubectl get all

