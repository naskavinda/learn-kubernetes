# Kubernetes Namespaces

Namespaces provide a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces.

## Commands

### Basic Namespace Operations

```bash
# List all namespaces
kubectl get namespaces
# or short form
kubectl get ns

# Get detailed information about a specific namespace
kubectl describe namespace <namespace-name>

# Create a new namespace
kubectl create namespace <namespace-name>

# Create a namespace using imperative command
kubectl create ns <namespace-name>

# Delete a namespace (this will delete all resources in the namespace)
kubectl delete namespace <namespace-name>
```

### Working with Resources in Specific Namespaces

```bash
# List pods in a specific namespace
kubectl get pods -n <namespace-name>
kubectl get pods --namespace=<namespace-name>

# List all pods in all namespaces
kubectl get pods --all-namespaces
# or short form
kubectl get pods -A

# Create a pod in a specific namespace
kubectl run nginx --image=nginx -n <namespace-name>

# Apply a YAML file to a specific namespace
kubectl apply -f pod.yaml -n <namespace-name>

# Get services in a specific namespace
kubectl get svc -n <namespace-name>

# Get deployments in a specific namespace
kubectl get deployments -n <namespace-name>
```

### Setting Default Namespace Context

```bash
# Set the default namespace for current context
kubectl config set-context --current --namespace=<namespace-name>

# View current context and default namespace
kubectl config view --minify | grep namespace

# Get current context
kubectl config current-context
```

### Namespace Resource Quotas and Limits

```bash
# Get resource quotas in a namespace
kubectl get resourcequota -n <namespace-name>

# Describe resource quota details
kubectl describe resourcequota -n <namespace-name>

# Get limit ranges in a namespace
kubectl get limitrange -n <namespace-name>
```

### Generate Namespace YAML

```bash
# Generate namespace YAML without creating it
kubectl create namespace <namespace-name> --dry-run=client -o yaml

# Generate and save namespace YAML to file
kubectl create namespace <namespace-name> --dry-run=client -o yaml > namespace.yaml
```

### Advanced Namespace Operations

```bash
# Filter resources by label in a namespace
kubectl get pods -n <namespace-name> -l app=nginx

# Watch pods in a specific namespace
kubectl get pods -n <namespace-name> -w

# Get events in a specific namespace
kubectl get events -n <namespace-name>

# Sort events by timestamp in a namespace
kubectl get events -n <namespace-name> --sort-by='.metadata.creationTimestamp'
```

## Default Namespaces in Kubernetes

- **default**: The default namespace for objects with no other namespace
- **kube-system**: The namespace for objects created by the Kubernetes system
- **kube-public**: This namespace is readable by all users (including those not authenticated)
- **kube-node-lease**: This namespace holds lease objects associated with each node

## Examples

```bash
# Create a development namespace
kubectl create namespace development

# Create a pod in the development namespace
kubectl run test-pod --image=nginx -n development

# List all resources in development namespace
kubectl get all -n development

# Switch default namespace to development
kubectl config set-context --current --namespace=development

# Create a namespace from YAML file
kubectl apply -f namespace.yaml
```
