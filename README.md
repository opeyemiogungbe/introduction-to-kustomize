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
![kubectl version](https://i.postimg.cc/SsBKNy3n/Screenshot-2026-01-12-052616.png)

### Step 3: Install and Start Minikube

![check minikube](https://i.postimg.cc/vZV3cX47/Screenshot-2026-01-12-063141.png)

Start Minikube using Docker as the driver:

```
minikube start --driver=docker
```
![minikube start](https://i.postimg.cc/pdk8TnJn/Screenshot-2026-01-12-063218.png)

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
![k8 pod](https://i.postimg.cc/R0qsv9cv/Screenshot-2026-01-12-064725.png)

#### Deploy the Pod:

```
kubectl apply -f mypod.yaml
```
#### Verify:

```
kubectl get pods
```
![deploy pod](https://i.postimg.cc/Rhvb5tdw/Screenshot-2026-01-12-070431.png)

### Step 7: Understanding the Need for Kustomize

#### At this stage:

- Configurations are managed manually

- Scaling across environments (dev, staging, prod) would require duplication

- Configuration drift becomes difficult to control

This is the problem Kustomize solves.

## Step 8: Create a Kustomize Base

### Create a base directory:

```
mkdir base
mv mypod.yaml base/
```
### Create base/kustomization.yaml:

```
resources:
- mypod.yaml
```
![base/kustomization.yaml](https://i.postimg.cc/RhS5gbbk/Screenshot-2026-01-13-010210.png)

### Apply the base configuration:

```
kubectl apply -k base/
```
![Apply base configuration](https://i.postimg.cc/mDd8GXrY/Screenshot-2026-01-13-012154.png)

## Step 9: Create an Environment Overlay (Dev)

### Create overlay directories:

```
mkdir -p overlays/dev
```

### Create overlays/dev/kustomization.yaml file:

```
resources:
- ../../base

namePrefix: dev-
```

Apply the overlay:

```
kubectl apply -k overlays/dev/
```

Verify:

```
kubectl get pods
```

Expected output:

```
dev-mypod
```

![overlay](https://i.postimg.cc/4ycG7hqS/Screenshot-2026-01-12-073220.png)

- An overlay is a layer of customization applied on top of a base. The base contains the common, reusable resource definitions (Pods, Deployments, Services, etc.). The overlay points to the base and applies environment-specific tweaks (like prefixes, image tags, resource limits, or labels).

Think of it like:
- Base = the blueprint of our app.
- Overlay = the environment-specific adjustments to that blueprint.

📂 Why We Use an overlays/ Directory
The directory structure is mostly about organization and clarity:
kustomize/
├── base/
│   ├── mypod.yaml
│   └── kustomization.yaml
└── overlays/
    ├── dev/
    │   └── kustomization.yaml
    ├── staging/
    │   └── kustomization.yaml
    └── prod/
        └── kustomization.yaml


- overlays/ acts as a container for all environment-specific configurations, this makes it easy to see at a glance: “Here are all my overlays (dev, staging, prod) that customize the base.” It avoids clutter at the top level — instead of having dev/, prod/, staging/ scattered alongside base/, they’re grouped neatly under overlays/.


## Project Structure

```
kustomize-project/
├── base/
│   ├── mypod.yaml
│   └── kustomization.yaml
├── overlays/
│   └── dev/
│       └── kustomization.yaml
```

### Verification Steps

- Run the following commands to confirm setup:

```
kustomize version
kubectl version
minikube status
```
![confirm setup](https://i.postimg.cc/wjyr6pKs/Screenshot-2026-01-13-014750.png)

On our Docker Desktop, we can see our minikube container live and active. The minikube container is our cluster, and all pods we deploy will run inside it.

![Docker desktop](https://i.postimg.cc/5yvkJ5Q4/Screenshot-2026-01-13-020030.png)

## Key Learnings

Manual YAML management does not scale

Kustomize promotes reuse without duplication

Overlays simplify multi-environment deployments

Kustomize integrates directly with kubectl

## Conclusion

This project demonstrates how Kustomize improves Kubernetes configuration management by introducing structure, reusability, and
environment separation. It provides a strong foundation for more advanced Kubernetes workflows and CI/CD integration.



