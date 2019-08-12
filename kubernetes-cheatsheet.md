[back to overwiev](/../..)

# Kubernetes Cheatsheet

## Table of Contents

- [Overview](#overview)
  - …
- [Minikube](#minikube)
  - …
- [Kubeadm](#kubeadm)
  - …
- [Kubectl](#kubectl)
  - …
- [PODs](#pod)
  - …
- [Replication Controller](#replication-controller)
  - …
- [Services](#services-1)
  - …
- [Deployments](#deployments)
  - …
- [Secrets](#secrets)
  - …
- [Useful](#useful)
  - [image from private registry](#image-from-private-registry)
- [Troubleshooting](#troubleshooting)


## Overview

- Kubernetes is an orchestrator for microservice apps.

### Pods

- Pod run Containers (that share the pod environment)
- Pods usually have only one container, they can have sidecars
- Sidecars are for example if you have a main container that writes logs + a log scraper that collects the logs and then expose them somewhere that log scraper would be called sidecar container.
- A pod is mortal, if it dies, a new one is created. They are never brought back to life.
- Every time a new pod is spin up it gets a new ip, that is why pod ips are not reliable
- That is why services are useful.

### Replication Controller / Replica Sets

- Are constructs designed to make sure the required number of pods is always running
- Kind of replaced by deployments
- Replica Sets is how they are called inside deployments (with subtle no needed to know diffs)

### Services

- Is a simple object defined by a manifest
- Provides a stable IP and DNS for pods sitting behind it
- Loadbalances requests
- Pods belong to services using labels (for example Prod+BE+1.3, then to update just change label to 1.4, so a rollback and forward is just a matter of changing labels)

### Deployment

- Is defined in yaml as desired state
- Add features to replication controllers/sets and takes care of it
- Simple rolling updates and rollbacks (blue-green / canary)

## Minikube

- Play around with kubernetes locally (single host kubernetes cluster)

### Install

#### OSX

https://minikube.sigs.k8s.io/docs/getting-started/macos/

Enable Kubernetes on your Docker for Mac Settings

```
brew install kubectl
brew cask install minikube

# Optional

brew install docker-machine-driver-xhyve
# follow instruction commands
```

#### Windows

https://minikube.sigs.k8s.io/docs/getting-started/windows/

#### Linux

https://minikube.sigs.k8s.io/docs/getting-started/linux/

### Basics

```bash
minikube start --vm-driver=<driver> --kubernetes-version=<version>
# example: minikube start --vm-driver=xhyve --kubernetes-version="v1.6.0"
```

- `--vm-driver`: which driver to use (default is a virtual machine)
- `--kubernetes-version`: default is latest

```bash
minikube stop
minikube delete
minikube dashboard
# Opens a dashboard GUI for minikube
```


## Kubeadm

### Startup – Multinodes

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

```
apt-get install docker.io kubeadm kubectl kubelet kubernetes-cni -y
```

```bash
kubeadm init
# follow instructions
```

You’ll need to have a pod network running. You can use the weave networking setup:

```bash
kubectl apply --filename https://git.io/weave-kube-1.6
# You can also run it with other versions using https://git.io/weave-kube without the 1.6
```

Now join other nodes (servers) with the join token you received from `kubeadm init`.

### Startup – Singlenode


## Kubectl

### Basics

```bash
kubectl config current-context
# Displays the current working context. For example `minikube`
kubectl cluster-info
# Information about your cluster. For example the IP
```

### Nodes

```
kubectl get nodes
```

Displays current running nodes

### Pods

```bash
kubectl get pods
# normal
kubectl get pods --all-namespaces
# Display the pods from all namespaces (= also system ones)
kubectl get pods/<pod-name>
# Example: kubectl get pods/hello-pod
# retrieves a single pod
kubectl describe pods
# Displays more information on the pods
```

### Replication Controllers

```bash
kubectl get rc
# normal
kubectl get rc/<rc-name>
# Example: kubectl get rc/hello-rc
# retrieves a single rc
kubectl describe rc
# Displays more information on the rcs
```

## POD

- Smallest unit in Kubernetes
- Contains 1 or more containers
- 1 IP to 1 POD relationship
- POD is either running or down, never half running
- Declared in a manifest file

### Example

*Note: usually you never work directly on a pod*

```yml
apiVersion: v1
kind: Pod
metadata:
  name: hello-pod
  labels:
    zone: prod
    version: v1
spec:
  containers:
    - name: hello-ctr
      image: nigelpoulton/pluralsight-docker-ci:latest
      ports:
        containerPort: 8080
```

- `apiVersion`: version to use
- `kind`: type of object we are declaring
- `metadata`: extra information
- `labels`: is a key-value pair object
- `containers`: list of containers to run in a pod

```bash
kubectl create -f <path>
# example: kubectl create -f pod.yml
```

remove it:
```bash
kubectl delete pods <name>
# example: kubectl delete pods hello-pod
```

## Replication Controller

### Example

```yml
apiVersion: v1
kind: ReplicationController
metadata:
  name: hello-rc
spec:
  replicas: 10
  selector:
    app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
        - name: hello-pod
          image: nigelpoulton/pluralsight-docker-ci:latest
          ports:
            containerPort: 8080
```


```bash
kubectl create -f <path>
# example: kubectl create -f rc.yml
```

### Update

- Just edit the yml file

```bash
kubectl apply -f <path>
```

## Services

- Sits in front of pods
- Exposes an IP that can be called to reach the PODs
- It will loadbalance between PODs automatically
- Services matches PODs via labels

### Example

#### Iterative

```bash
kubectl expose rc <rc-name> --name=<service-name> --target-port=<port> --type=<type>
# Example: kubectl expose rc hello-rc --name=hello-svc --target-port=8080 --type=NodePort
kubectl describe svc <service-name>
kubectl delete svc <service-name>
```

#### Declatative

```yml
apiVersion: v1
kind: Service
metadata:
  name: hello-svc
  labels:
    app: hello-world
spec:
  type: NodePort
  ports:
    - port: 8080
      nodePort: 30001
      protocol: TCP
  selector:
    app: hello-world
```

- `type`: the service type.
  - `CluserIP`: Stable internal cluster IP. Default. Makes the service only available to other nodes in the same cluster.
  - `NodePort`: Exposes the app outside of the cluster by adding a cluster wide port on top of the IP
  - `LoadBalancer`: Integrates the NodePort with an existing LoadBalancer
- `port`: is the port exposed within the container. It gets mapped through the NodePort (convention something above 30000) on the whole cluster (here 30001).
- `selector`: has to match the label of the PODs/RC

```bash
kubectl create -f <path>
# example: kubectl create -f svc.yml
# kubectl delete svc hello-svc
```

### Updates / Deployments

- Usually you give the pods a version label. E.g. (app=foo;zone=prod;ver=1.0.0)
- The service matches everything but the version label. E.g. (app=foo;zone=prod)
- New version comes: spin up new pods with the new version tag. E.g. (app=foo;zone=prod;ver=2.0.0)
- Now it’s loadbalanced across the new and the old
- If you’re happy, let the service match only the new version by adding the version label to the service. E.g. (app=foo;zone=prod;ver=2.0.0)
- The old pods are still there. To rollback, you just revert the label of the service to the old version  E.g. (app=foo;zone=prod;ver=1.0.0) and it will only target the old app. No downtime.
This is basically called blue/green deployment

## Deployments

- All about rolling updates and simple rollbacks
- Deployments wrap around replication controllers (in the world of deployment called replica set)
- Deployments manage Replica Sets, Replica Sets manage Pods

### Example

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-deploy
spec:
  selector:
    matchLabels:
      app: hello-world
  replicas: 10
  minReadySeconds: 10
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
        - name: hello-pod
          imagePullPolicy: IfNotPresent
          image: nigelpoulton/pluralsight-docker-ci:latest
          ports:
            - containerPort: 8080
```

- `minReadySeconds`: let the pod run for 10 second before marking it as ready
- `strategy`: select what kind of update strategy to use (here we use rolling updates)
  - `maxUnavailable` & `maxSurge`: by setting it to 1 we tell the deployment to do them 1 by 1

```bash
kubectl create -f <path>
# example: kubectl create -f deploy.yml
kubectl describe deploy hello-deploy
```

### Updates

- Edit the yml file.
- `kubectl apply -f <file> --record` will apply the changes
- `kubectl rollout status deployment <name-of-deployment>` to watch it
- `kubectl get deploy <name-of-deployment>` check if all are available
- `kubectl rollout history deployment <name-of-deployment>` to see the versions and why it happened
- `kubectl get rs` you’ll see that you have 2 replica sets, 1 with 0 pods and another with the new pods

### Rollbacks

- `kubectl rollout undo deployment <name-of-deployment> --to-revision=<number>` (you can see revisions via `kubectl rollout history deployment <name-of-deployment>`)
- does the same as for the update but in reverse to match the desired state of the specified revision number.

## Secrets

https://kubernetes.io/docs/concepts/configuration/secret/

### Example

```yml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

- `data`: is a key-value store with the value set as a BASE64 encoded sring.

```
kubectl apply -f ./secret.yaml
```

*Note: secrets can also be created from files:*

```bash
kubectl create secret generic <name> --from-file=<path>
```

- `name`: the name of the secret

And then retrieved with:

```bash
kubectl get secret <name> -o yaml
```

## Useful

### Image from private registry

https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/

```
docker login <registry uri>
```
```
kubectl create secret generic regcred \
    --from-file=.dockerconfigjson=<path/to/.docker/config.json> \
    --type=kubernetes.io/dockerconfigjson
```

- `<path/to/.docker/config.json>`: is usually in the users folder `Users/<username>/.docker/config.json`

OR

```bash
kubectl create secret docker-registry regcred --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
# Example: kubectl create secret docker-registry regcred --docker-server=https://example.com/ --docker-username=docker --docker-password=password --docker-email=example@mail.com
```

You can get the secret:
```
kubectl get secret regcred --output=yaml
```

Now you can add it to your pods:

```yml
…
spec:
  containers:
  …
  imagePullSecrets:
  - name: regcred
```


## Troubleshooting

### Use local docker image

Minikube runs in a VM hence it will not see the images you've built locally on a host machine, but... as stated in https://github.com/kubernetes/minikube/blob/master/docs/reusing_the_docker_daemon.md you can use `eval $(minikube docker-env)` to actually utilise docker daemon running on minikube, and henceforth build your image on the minikubes docker and thus expect it to be available to the minikubes k8s engine without pulling from external registry
