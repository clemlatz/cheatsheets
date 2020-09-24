# Kubernetes

## What is Kubernetes

Kubernetes (K8s) is an open-source system for automating deployment, scaling, 
and management of containerized applications.

## kubectl commands

```console
kubectl version                                  # shows version
kubectl clust-info                               # view cluster information
kubectl get all                                  # retrieve information about 
                                                   pods, deploys, services, etc.
kubectl run [container] --image=[image-name]     # simple way to create a   
                                                   deployment for a pod
kubectl port-forward [pod] [ports]               # forward a port to allow 
                                                   external access
kubectl expose                                   # expose a port for a 
                                                   deployment/pod
kubectl create [resource]                        # create a resource
kubectl apply [resource]                         # create or modify a resource
```

## Pods

A pod is the basic execution unit of a Kubernetes application — the smallest
and simplet unit in the Kubernetes object model that we create or deploy. It's
the basic building block.

### Create a pod

Run a container with the nginx:alpine image in a pod in a imperative way:

```console
kubectl run [deployment-name] --image=nginx:alpine
```

### List pods

```console
kubectl get pods
kubectl get pods --watch
```

### Expose pod ports

Pods and containers are only accessible within the Kubernetes cluster by 
default. One way to expose a container port is:

```console
kubectl port-forward [pod-id] 8080:80
```

#### Delete a pod

If we simply delete a pod, k8s will recreate it immediatly, because its 
deployment stil exists, and it's role is to make sure the pod is running.

```console
kubectl delete pod [pod-id]
```

If we need to delete a pod for good, we need to find and delete its deployment.

```console
kubectl delete deployment [deployment-name]
```
