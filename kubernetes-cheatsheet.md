[back to overwiev](/../..)

# Docker Cheatsheet

## Table of Contents

- [Install](#install)
- [Basics](#basics)
- [Minikube](#minikube)
- [Kubeadm](#kubeadm)
- [Kubectl](#kubectl)
- [Pods](#pods)
- [Replication Controller](#replication-controller)
- [Services](#services)
- [Deployment](#deployment)

# Install

## kubectl

### Mac

```bash
brew install kubectl
```

## Minikube (local testing)

### Mac

```bash
brew cask install minikube
```

If you want to use xhyve:

```bash
brew install docker-machine-driver-xhyve
sudo chown root:wheel /usr/local/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
sudo chmod u+s /usr/local/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
```

## kubeadm (manual kubernetes installation)

### Ubuntu machines

Node 1 : Master  
Node 2+3 : Workers  
Every node needs:

- Docker (see: /../../docker-cheatsheet.md#install)
- Kubelet
- Kubeadm
- Kubectl
- CNI

```bash
apt-get update && apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
```

```bash
apt-get install -y kubelet kubeadm kubectl kubernetes-cni
```

see: https://kubernetes.io/docs/setup/independent/install-kubeadm/

-

# Kubectl

## Basics

- Display pods

```bash
kubectl get pods
kubectl get pods/<name>
# Example: kubectl get pods/hello-pod
kubectl get pods --all-namespaces
```

- Describe pods

```bash
kubectl describe pods
```

## Expose

```bash
kubectl expose rc hello-rc --name=hello-svc --target-port=8080 --type=NodePort
```

## Endpoints

```bash
kubectl get ep
```

```bash
kubectl describe ep hello-svc
```

## Context

```bash
kubectl config current-context
```

## Nodes

### List

```
kubectl get nodes
```

-

# Minikube

## Install

### Mac

```bash
brew cask install minikube
```

If you want to use xhyve:

```bash
brew install docker-machine-driver-xhyve
sudo chown root:wheel /usr/local/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
sudo chmod u+s /usr/local/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
```

### Windows/Linux

See https://github.com/kubernetes/minikube

## Start/Stop/Delete

```bash
minikube start --vm-driver=<driver> --kubernetes-version=<version>
# Example: minikube start --vm-driver=xhyve --kubernetes-version="v1.6.0"
minikube stop
minikube delete
```

- `<driver>`: (optional) sets the driver to use
- `<version>`: (optional) sets the version to use

## Dashboard

```bash
minikube dashboard
```

-

# Kubeadm

see [install section](#install)  
All nodes have to have all requirements installed for this to work.

Node 1 : Master  
Node 2+3 : Workers

## Init

```bash
kubeadm init
```

- Copy and paste the commands as shown in the console output
- Now you can use kubectl

```bash
kubectl get nodes
kubectl get pods --all-namespaces
```

- Apply file:

```bash
kubectl apply --filename https://git.io/weave-kube-1.6
```

This is a config file that installs a pot network add-on.  
You must install a pod network add-on so that your pods can communicate with each other.

Have a look at https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#pod-network

- If you want the master to also be a worker, follow https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/#control-plane-node-isolation
- Paste the join code inside the other nodes

-

# Pods

- You usually don’t work directly with a pod (usually a replication controller, that controlls the pods)

- Smallest possible unit in a kubernetes network
- Pods have multiple containers
- Pods have their own IP

## Example Config

```yml
# pod.yml
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
        - containerPort: 8080
```

## Create/Get

```bash
kubectl create -f pod.yml
```

```bash
kubectl get pods
```

-

# Replication Controller

## Example Config

```yml
# rc.yml
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
            - containerPort: 8080
```

## Create/Get

```bash
kubectl create -f rc.yml
```

```bash
kubectl get rc
```

## Apply Changes

- Edit the config file
- Apply it:

```bash
kubectl apply -f rc.yml
```

## Delete

```bash
kubectl delete rc hello-rc
```

# Services

- Reliable endpoint between client and pods (ip, dns, port)
- Loadbalances requests
- Connected to pods via labels

## Example Description

```yml
# svc.yml
apiVersion: v1
kind: Service
metadata:
  name: hello-svc
  labels:
    app: hello-world
spec:
  type: NodePort
  nodePort: 30001
  ports:
    - port: 8080
    protocol: TCP
  selector:
    app: hello-world
```

- `NodePort`: is a ServiceType. There are other types as:
- - `ClusterIP`: Gives the service a stable IP within the cluster
- - `NodePort`: Takes the CluserIP and exposes it to the outside by adding a cluser-wide port on top of the ClusterIP
- - `LoadBalancer`: Integrates NodePort with cloud-based load balancers

## Create/Get

```bash
kubectl create -f svc.yml
```

```bash
kubectl get svc
```

-

# Deployment

Have a look at: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#creating-a-deployment

```yml
# deploy.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-deploy
spec:
  replicas: 10
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
        - name: hello-pod
          image: nigelpoulton/pluralsight-docker-ci:latest
          ports:
            - containerPort: 8080
```

## Updates

- Edit the config file

```yml
# deploy.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-deploy
spec:
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
          image: nigelpoulton/pluralsight-docker-ci:latest
          ports:
            - containerPort: 8080
```

- Apply it:

```bash
kubectl apply -f deploy.yml --record
```

- Check it:

```bash
kubectl rollout status deployment hello-deploy
```

```bash
kubectl rollout deployment hello-deploy
```

## Rollback

```bash
kubectl rollout undo deployment hello-deploy --to-revision=1
```

## Create/Get

```bash
kubectl create -f deploy.yml
```

```bash
kubectl describe deploy hello-deploy
```

```bash
kubectl get rs
```

```bash
kubectl describe rs
```
