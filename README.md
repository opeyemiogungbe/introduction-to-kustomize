# Introduction to Configuration Management in Kubernetes with Kustomize

## Project Overview

This project introduces configuration management in Kubernetes using Kustomize. The goal is to understand how Kubernetes
configurations are managed, why manual YAML management does not scale, and how Kustomize solves this problem using a declarative,
template-free approach.

The project is executed locally using Minikube and Docker Desktop, making it suitable for learning, testing, and development
purposes.

### Project Objectives

- Understand Kubernetes configuration objects

- Learn the importance of configuration management

- Install and use Kustomize

- Create reusable base configurations

- Create environment-specific overlays

- Deploy Kubernetes resources locally using Minikube

### Project Environment

This project is designed to run on a local machine, not on a virtual server or cloud Kubernetes cluster.

### Recommended Setup

```
Local Machine
├── Docker Desktop
├── Minikube (Docker driver)
├── kubectl
├── Kustomize
└── VS Code
```
No cloud VM or managed Kubernetes service is required for this project.

### Prerequisites

- Technical Requirements

## Requirement                                               	Description

Kubernetes Basics	                                      Understanding of Pods, Deployments, Services

Docker Desktop	                                        Container runtime for Minikube

kubectl	Kubernetes                                      Command-line tool

Minikube	                                              Local Kubernetes cluster

Kustomize	Kubernetes                                    Configuration customization

Code Editor	                                            Visual Studio Code recommended

System Resources	                                      Minimum 8GB RAM and 2 CPU cor


### Step 1: Verify Docker Installation

Ensure Docker Desktop is installed and running.

```
kubectl version --client
```

### Step 3: Install and Start Minikube

Start Minikube using Docker as the driver:

```
minikube start --driver=docker
```

### Verify the cluster

```
kubectl get nodes
```


### Step 4: Verify Kustomize Installation

#### Kustomize is built into kubectl.

```
kubectl kustomize --help
```

### Step 5: Create Project Directory

#### Create and move into the project directory:

```
mkdir kustomize-project
cd kustomize-project
```
### Step 6: Create a Sample Kubernetes Pod

#### Create a file named mypod.yaml:

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: nginx
```

#### Deploy the Pod:

```
kubectl apply -f mypod.yaml
```
#### Verify:

```
kubectl get pods
```

### Step 7: Understanding the Need for Kustomize

#### At this stage:

- Configurations are managed manually

- Scaling across environments (dev, staging, prod) would require duplication

- Configuration drift becomes difficult to control

This is the problem Kustomize solves.
