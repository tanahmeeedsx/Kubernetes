<p align="center">
  <img src="https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.svg" alt="Kubernetes Logo" width="110" />
</p>

<h1 align="center">Kubernetes</h1>

<p align="center">
  Learning Kubernetes through hands-on practice
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Progress-Day%2003-6A5ACD?style=flat-square" alt="Progress: Day 03" />
</p>

---

## Learning Progress

| Day | Topics | Status |
|---|---|---|
| Day 01 | Kubernetes fundamentals, architecture, deployment models, Minikube installation, and Nginx Pod practice | Completed |
| Day 02 | Kind setup, cluster and node management, kubectl, YAML manifests, imperative and declarative approaches, namespaces, and service ports | Completed |
| Day 03 | Minikube command revision, Minikube vs Kind, imperative and declarative workflows, YAML manifests, and Kubernetes workload resources | Completed |

## Day 01 - Learning Summary

- Understood what Kubernetes is and why container orchestration is needed.
- Explored automated scheduling, self-healing, scaling, and workload management.
- Learned the high-level history of Kubernetes, including Google’s Borg and Omega systems.
- Compared physical server, virtual machine, container, and Kubernetes-based deployment models.
- Studied cluster architecture, control plane components, worker nodes, and Pods.
- Learned about Minikube, kind, and kubeadm.
- Explored managed Kubernetes services: Amazon EKS, Google GKE, and Azure AKS.
- Installed Minikube and practiced running an Nginx workload in a local cluster.

### Commands Practiced

Minikube cluster management:

```bash
# Check Minikube status
minikube status

# List Minikube profiles
minikube profile list

# View Minikube configuration
minikube config view

# Start the local cluster
minikube start
```

Kubernetes workload and node inspection:

```bash
# List nodes with additional details
kubectl get nodes -o wide

# Run an Nginx Pod
kubectl run mynginx --image=nginx:latest

# List Pods
kubectl get pods

# Check the container runtime version reported by the node
kubectl get nodes -o jsonpath='{.items[0].status.nodeInfo.containerRuntimeVersion}'

# Inspect Minikube node runtime information
kubectl describe node minikube | grep -i "Container Runtime"
```

Container runtime inspection (from inside the Minikube node):

```bash
# Display runtime information
crictl info

# List containers
crictl ps

# List container images
crictl images
```

## Day 02 - Learning Summary

- Installed Kind and explored how it creates local Kubernetes clusters using Docker containers as nodes.
- Learned the basic commands for creating, listing, and checking the status of Kind clusters and nodes.
- Explored kubectl as the command-line tool for communicating with and managing Kubernetes clusters.
- Practiced working with YAML configuration files and learned that Kubernetes resources can be managed through both imperative commands and declarative manifests.
- Explored the purpose and basic structure of Kind cluster configuration, Namespace, Pod, Deployment, and Service YAML files.
- Created and inspected Kubernetes namespaces using both kubectl commands and YAML manifests.
- Learned the basic purpose of a Service: providing a stable access point to reach matching Pods.
- Explored basic port concepts, including containerPort, targetPort, and hostPort, and how ports connect traffic to an application.
- Practiced checking cluster information, nodes, namespaces, and services through kubectl.

These were introductory, hands-on exercises. The goal was to understand the purpose of the commands and manifests before moving into their detailed configuration and networking behavior.

### Commands Practiced

Kind installation and cluster setup:

```bash
# Run the installer script
./installer.sh

# Create a named multi-node Kind cluster from a configuration file
kind create cluster --config=kind-config.yml --name=my-cluster

# Create a Kind cluster from a configuration file
kind create cluster --config kind-config.yml

# List Kind clusters
kind get clusters

# List cluster nodes
kind get nodes
```

Cluster and namespace inspection:

```bash
# Check nodes
kubectl get nodes

# Get cluster information for the named Kind context
kubectl cluster-info --context kind-my-cluster

# List namespaces
kubectl get namespace

# Create a namespace imperatively
kubectl create namespace tomal

# List namespaces again
kubectl get namespaces
```

Apply YAML manifests and inspect workloads:

```bash
# Create or update a Namespace from YAML
kubectl apply -f namespace.yml

# Create or update a Deployment from YAML
kubectl apply -f deployment.yml

# List Pods in the nginx namespace
kubectl get pods -n nginx

# Describe a Pod in the nginx namespace
kubectl describe pod <pod-name> -n nginx
```

Persistent volume and storage inspection:

```bash
# List PVCs in the nginx namespace
kubectl get pvc -n nginx

# List PersistentVolumes
kubectl get pv

# List StorageClasses
kubectl get storageclass

# Edit the PVC manifest
nano pvc.yaml

# Apply the PVC manifest
kubectl apply -f pvc.yaml
```

Docker image inspection:

```bash
# List locally available Docker images
docker images
```

## Day 03 - Learning Summary

- Revised essential Minikube commands for starting, stopping, pausing, deleting, and inspecting a local Kubernetes cluster.
- Explored Minikube version checks, dashboard, IP, tunnel, node management, logs, services, and Ingress.
- Compared Minikube and Kind, focusing on their similarities, differences, and local cluster management workflows.
- Learned how a Kind cluster can be created from a YAML configuration file using `kind create cluster --config`.
- Practiced imperative commands to create and manage Kubernetes resources directly through the CLI.
- Learned the declarative workflow for managing Kubernetes resources through YAML manifests using `kubectl apply -f`.
- Explored the purpose and structure of around 13 Kubernetes YAML configuration and resource files.
- Reviewed Namespace, Pod, Deployment, ReplicaSet, Service, Job, CronJob, DaemonSet, PersistentVolume, PersistentVolumeClaim, Ingress, and Dashboard access configuration.
- Applied Namespace, CronJob, and DaemonSet manifests and inspected the resulting Kubernetes resources.
- Practiced checking namespaces, CronJobs, DaemonSets, Pods, and Pod placement across nodes.
- Learned that not every YAML file is a Kubernetes resource manifest: Kind cluster configuration is passed to the Kind CLI, while Kubernetes resource manifests are generally applied with kubectl.

### Commands Practiced

Minikube cluster management:

```bash
# Check Minikube version
minikube version

# Start, stop, pause, or delete a Minikube cluster
minikube start
minikube stop
minikube pause
minikube delete

# Open the Kubernetes Dashboard
minikube dashboard

# Get the Minikube IP address
minikube ip

# Start a tunnel for LoadBalancer services
minikube tunnel

# List Minikube nodes
minikube node list

# Add or delete a Minikube node
minikube node add
minikube node delete <node-name>

# View Minikube logs
minikube logs

# Access a Service
minikube service <service-name>

# Enable the Ingress addon
minikube addons enable ingress

# List Minikube profiles
minikube profile list
```

Kind cluster configuration:

```bash
# Create a Kind cluster from its configuration file
kind create cluster --config kind-config.yml

# List Kind clusters
kind get clusters

# List Kind nodes
kind get nodes
```

Imperative and declarative resource management:

```bash
# Create a Namespace imperatively
kubectl create namespace nginx

# Apply a Kubernetes resource manifest declaratively
kubectl apply -f namespace.yml

# Inspect namespaces
kubectl get namespaces

# Inspect CronJobs across all namespaces
kubectl get cronjobs -A

# Inspect DaemonSets across all namespaces
kubectl get daemonsets -A

# Inspect Pods with node placement
kubectl get pods -A -o wide

# Delete a resource using its manifest
kubectl delete -f namespace.yml
```

Important distinction:

- Kind cluster configuration: `kind create cluster --config kind-config.yml`
- Kubernetes resource manifests: `kubectl apply -f filename.yml`

---

<p align="center">
  Learning by doing • One concept at a time
</p>
