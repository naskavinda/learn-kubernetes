# Kubernetes Labels and Selectors

## Overview
Labels are key-value pairs attached to Kubernetes objects (like pods, services, deployments). Selectors are used to identify and group objects based on their labels. They are fundamental to how Kubernetes organizes and manages resources.

## Key Concepts
- **Labels**: Metadata attached to objects for identification and organization
- **Selectors**: Queries used to filter objects based on their labels
- **Annotations**: Similar to labels but for non-identifying metadata (not used for selection)

---

## Label Management Commands

### Add Labels to Objects

```bash
# Add a label to a pod
kubectl label pod nginx-pod app=frontend

# Add multiple labels to a pod
kubectl label pod nginx-pod app=frontend tier=web environment=production

# Add label to a deployment
kubectl label deployment myapp-deployment version=v1.2

# Add label to a service
kubectl label service myapp-service component=backend

# Add label to a namespace
kubectl label namespace development team=devops
```

### View Labels

```bash
# Show labels for all pods
kubectl get pods --show-labels

# Show labels for specific pod
kubectl describe pod nginx-pod

# Show labels for all objects of a type
kubectl get deployments --show-labels
kubectl get services --show-labels

# Show labels in custom columns
kubectl get pods -L app,tier,environment
kubectl get pods --label-columns=app,tier
```

### Modify Labels

```bash
# Update/modify existing label
kubectl label pod nginx-pod app=backend --overwrite

# Remove a label (add minus sign after label name)
kubectl label pod nginx-pod app-

# Remove multiple labels
kubectl label pod nginx-pod app- tier- environment-
```

---

## Selector Commands

### Equality-based Selectors

```bash
# Select pods with specific label
kubectl get pods -l app=frontend
kubectl get pods --selector app=frontend

# Select pods with multiple labels (AND operation)
kubectl get pods -l app=frontend,tier=web

# Select pods where label does not equal value
kubectl get pods -l app!=backend

# Select objects that have a specific label (regardless of value)
kubectl get pods -l app

# Select objects that do not have a specific label
kubectl get pods -l '!app'
```

### Set-based Selectors

```bash
# Select pods where app is either frontend OR backend
kubectl get pods -l 'app in (frontend,backend)'

# Select pods where app is NOT frontend or backend
kubectl get pods -l 'app notin (frontend,backend)'

# Select pods that have the 'environment' label
kubectl get pods -l 'environment'

# Select pods that do NOT have the 'environment' label
kubectl get pods -l '!environment'

# Complex selector combining multiple conditions
kubectl get pods -l 'app in (frontend,backend),tier=web'
```

---

## Practical Examples

### Example 1: Creating Resources with Labels

```bash
# Create a pod with labels using imperative command
kubectl run nginx-frontend --image=nginx --labels="app=frontend,tier=web,env=prod"

# Create a deployment with labels
kubectl create deployment web-app --image=nginx --dry-run=client -o yaml > web-app-deployment.yaml
# Then edit the YAML to add labels in metadata section

# Expose deployment as service with selector
kubectl expose deployment web-app --port=80 --target-port=80 --selector="app=web-app"
```

### Example 2: Managing Multiple Environments

```bash
# Label pods for different environments
kubectl label pods --all environment=development
kubectl label pod prod-pod-1 environment=production --overwrite
kubectl label pod test-pod-1 environment=testing --overwrite

# Get pods by environment
kubectl get pods -l environment=production
kubectl get pods -l environment=development
kubectl get pods -l environment=testing

# Get all non-production pods
kubectl get pods -l 'environment in (development,testing)'
```

### Example 3: Deployment and Service Selectors

```bash
# Create deployment with specific labels
kubectl create deployment api-server --image=nginx --dry-run=client -o yaml | \
kubectl label --local -f - app=api tier=backend --dry-run=client -o yaml | \
kubectl apply -f -

# Create service that selects specific pods
kubectl expose deployment api-server --port=80 --name=api-service --selector="app=api,tier=backend"

# Verify service endpoints match labeled pods
kubectl get endpoints api-service
kubectl get pods -l app=api,tier=backend
```

### Example 4: Node Selectors (Scheduling)

```bash
# Label nodes for pod scheduling
kubectl label nodes worker-node-1 disktype=ssd
kubectl label nodes worker-node-2 disktype=hdd

# View node labels
kubectl get nodes --show-labels
kubectl get nodes -L disktype

# Create pod with nodeSelector (add to YAML):
# spec:
#   nodeSelector:
#     disktype: ssd
```

---

## Advanced Selector Operations

### Delete Resources by Labels

```bash
# Delete all pods with specific label
kubectl delete pods -l app=frontend

# Delete all resources with specific label
kubectl delete all -l app=myapp

# Delete pods with multiple label conditions
kubectl delete pods -l 'app in (frontend,backend),environment=development'
```

### Scale Deployments by Labels

```bash
# Scale all deployments with specific label
kubectl get deployments -l app=web -o name | xargs -I {} kubectl scale {} --replicas=3

# Scale specific deployment found by label
kubectl scale deployment -l app=frontend --replicas=5
```

### Resource Monitoring by Labels

```bash
# Watch pods with specific labels
kubectl get pods -l app=frontend -w

# Get resource usage for labeled pods
kubectl top pods -l app=frontend

# Describe all services with specific label
kubectl describe services -l component=backend
```

---

## YAML Examples

### Pod with Labels
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: labeled-pod
  labels:
    app: frontend
    tier: web
    environment: production
    version: v1.0
spec:
  containers:
  - name: nginx
    image: nginx
```

### Service with Selector
```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend-service
  labels:
    app: frontend
spec:
  selector:
    app: frontend    # This service will target pods with app=frontend label
    tier: web
  ports:
  - port: 80
    targetPort: 80
```

### Deployment with Labels and Selector
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
  labels:
    app: web
spec:
  replicas: 3
  selector:
    matchLabels:      # Deployment manages pods with these labels
      app: web
      tier: frontend
  template:
    metadata:
      labels:         # Pods created will have these labels
        app: web
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
```

---

## Best Practices

### Label Naming Conventions
- Use meaningful, descriptive names
- Follow format: `key=value` where key can include prefixes
- Common patterns:
  - `app=frontend` (application name)
  - `tier=web` (architectural tier)
  - `environment=production` (deployment environment)
  - `version=v1.2` (version information)
  - `component=database` (component type)
  - `managed-by=helm` (management tool)

### Recommended Labels
```bash
# Standard recommended labels
kubectl label pod myapp-pod \
  app.kubernetes.io/name=myapp \
  app.kubernetes.io/instance=myapp-prod \
  app.kubernetes.io/version=1.0.0 \
  app.kubernetes.io/component=frontend \
  app.kubernetes.io/part-of=myplatform \
  app.kubernetes.io/managed-by=kubectl
```

### Selector Best Practices
- Use specific selectors to avoid unintended matches
- Combine multiple labels for precise selection
- Be consistent with labeling across similar resources
- Use set-based selectors for complex queries
- Document your labeling strategy for team consistency

---

## Troubleshooting

### Common Issues

```bash
# Check if selector matches any pods
kubectl get pods -l app=myapp --show-labels

# Verify service selector matches pod labels
kubectl describe service myservice
kubectl get pods -l <service-selector> --show-labels

# List all unique label keys in use
kubectl get pods -o json | jq -r '.items[].metadata.labels | keys[]' | sort -u

# List all unique label values for a specific key
kubectl get pods -o json | jq -r '.items[].metadata.labels.app' | sort -u
```

### Debug Service Selection

```bash
# Check service endpoints
kubectl get endpoints myservice

# Verify pod labels match service selector
kubectl describe service myservice | grep Selector
kubectl get pods --show-labels | grep <matching-labels>
```
