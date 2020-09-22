# Kubernetes

## What is Kubernetes

Kubernetes (K8s) is an open-source system for automating deployment, scaling, 
and management of containerized applications.

## kubectl

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