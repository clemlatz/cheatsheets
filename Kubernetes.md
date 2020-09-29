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
kubectl describe [pod]                           # get info about a pod
kubectl exec [pod-name] -it sh                   # access a shell inside a pod
```

## Pods

A pod is the basic execution unit of a Kubernetes application — the smallest
and simplet unit in the Kubernetes object model that we create or deploy. It's
the basic building block.

### Managing a pod

#### Create a pod

Run a container with the nginx:alpine image in a pod in a imperative way:

```console
kubectl run [deployment-name] --image=nginx:alpine
```

#### List pods

```console
kubectl get pods
kubectl get pods --watch
```

#### Expose pod ports

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

### Declarative configuration

Instead of creating pods in a imperative way using kubectl commands, we can use
a YAML file to describe a pod in a declarative way.

#### YAML file specs

```yaml
# nginx.pod.yaml
apiVersion: v1            # Kuberentes API version
kind: Pod                 # Type of Kubernetess resource
metadata:                 # Pod metadata
  name: my-nginx
  labels:                 # Labels are used to link ressources together
    app: nginx
    rel: stable
spec:                     # Spec or blueprint for the pod
  containers:             # Info about the containers that will run in our pod
  - name: my-nginx
    image: nginx:alpine
    ports:
    - containerPort: 80
```

#### Create a pod using a YAML file

```console
kubectl create --filename nginx.pod.yaml
kubectl create -f nginx.pod.yaml --dry-run  # dry run
kubectl apply -f nginx.pod.yaml             # create or update if already exists
```

#### Edit a pod created with a YAML file's config

Will open a file editor to live edit pod's config.

```console
kubectl edit -f nginx.pod.yaml
```

#### Delete a pod created with a YAML file

Will effectiverly delete the pod because no deployment is associated.

```console
kubectl delete -f nginx.pod.yaml
```

### Pod health

Kubernetes pod health relies on probes that periodically performes diagnostic
on a container.

They are two types of probes:
- The liveness probe determines is a pod is healty and running as expected
- The readiness probe determines if a pod is ready to receive requests

A probe can perfoms thee actions:
- ExecAction: runs a command in the container
- TCPSocketAction: checks tcp against the container's ip on a specified port
- HTTPGetAction: runs an HTTP GET request against a container

A probe can have the following results:
- Success
- Failure
- Unknown

Probes is defined in the YAML config file:

```yaml
apiVersion: v1
kind: Pod
# …
spec:
  containers:
  - name: my-nginx
    image: nginx: alpine
    livenessProbe:                # Define a liveness probe
      httpGet:
        path: /index.html         # Check GET /index.html on port 80
        port: 80
      initialDelaySeconds: 15     # Wait 15 seconds after container started (default none)
      timeoutSeconds: 2           # Timeout after 2 seconds (default 1)
      periodSeconds: 5            # Check every 5 seconds (default 10)
      failureThreshold: 1         # Number of failure before restart (default 3)
    readinessProbe:
      httpGet:
        path: /index.html
        port: 80
      initialDelaySeconds: 2
      periodSeconds: 5
```

## Deployments

Deployments and ReplicaSets ensure Pods stay running.

A ReplicaSet's
- act as a self-healing mechanism
- ensures the requested number of pods are available
- provides fault-tolerance
- can be used to scale pods
- relies on a Pod template
- used by deployments

A Deployment
- manages Pods using ReplicaSet
- scales ReplicaSets, which in turn scale Pods
- supports zero-downtime updates by creating and destroying ReplicaSets
- provides rollback functionnality
- creates a unique label assigned to the ReplicaSet and generated Pods
- uses a YAML config file very similar to a ReplicaSet

### Creating a Deployment

```yaml
apiVersion: v1
kind: Deployment
metadata:                         # Deployment metadata
  name: frontend
  labels:
    app: my-nginx
    tier: frontend
spec:
  selector:                       # Select a template to use by matching labels
    matchLabels:
      tier: frontend
  template:                       # Template to used to create pod/containers,
    metadata:                     # using matching labels as it could be placed
      labels:                     # in another file
        tier: front-end
    spec:                         # Template spec
      containers:
      - name: my-nginx
        image: nginx-alpine
# …
```
