# Creating YAML Files with kubectl Commands### Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) and save it to a file

```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

Make necessary changes to the file (for example, adding more replicas) and then create the deployment:

```bash
kubectl create -f nginx-deployment.yaml
```As you might have seen already, it is a bit difficult to create and edit YAML files, especially in the CLI. During the exam, you might find it difficult to copy and paste YAML files from browser to terminal. 

Using the `kubectl run` command can help in generating a YAML template. And sometimes, you can even get away with just the `kubectl run` command without having to create a YAML file at all. For example, if you were asked to create a pod or deployment with specific name and image you can simply run the `kubectl run` command.

## Instructions

Use the below set of commands and try the previous practice tests again, but this time try to use the below commands instead of YAML files. Try to use these as much as you can going forward in all exercises.

## Reference

**Bookmark this page for exam. It will be very handy:**

[kubectl Conventions](https://kubernetes.io/docs/reference/kubectl/conventions/)

## Pod Operations

### Create an NGINX Pod

```bash
kubectl run nginx --image=nginx
```

### Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml
```

## Deployment Operations

### Create a deployment

```bash
kubectl create deployment --image=nginx nginx
```

### Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```

Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run) and save it to a file.

```bash
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

Make necessary changes to the file (for example, adding more replicas) and then create the deployment.

```bash
kubectl create -f nginx-deployment.yaml
```


## Alternative Method (Kubernetes 1.19+)

In k8s version 1.19+, we can specify the `--replicas` option to create a deployment with 4 replicas:

```bash
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```