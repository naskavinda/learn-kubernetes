### Commands

# Create a ReplicaSet from the replicaset.yml file
kubectl create -f replicaset.yml

# Get the list of ReplicaSets (rs is short for replicaset)
kubectl get replicaset / kubectl get rs

# Get the list of pods managed by the ReplicaSet
kubectl get pod

# Edit the ReplicaSet configuration directly
kubectl edit replicaset myapp-replicaset

# Scale the ReplicaSet to 2 replicas
kubectl scale replicaset myapp-replicaset --replicas=2

# Delete the ReplicaSet
kubectl delete rs myapp-replicaset

# get the replicaset live yaml config
kubectl  get rs myapp-replicaset -o yaml


