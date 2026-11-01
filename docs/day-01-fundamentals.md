# Day 01 - Kubernetes Fundamentals

Date: November 1, 2026

## 1. What Is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform. It helps deploy, manage, and scale containerized applications across a cluster of machines.

Instead of manually managing every container, I can describe the desired state of my application. Kubernetes works to keep the actual state close to that desired state.

### Why Is Kubernetes Useful?

When applications run across multiple containers and machines, manual management becomes difficult. Kubernetes helps with:

- Automated scheduling: places Pods on suitable worker nodes.
- Self-healing: replaces or restarts failed workloads according to the workload configuration.
- Scaling: supports increasing or decreasing the number of application replicas.
- Rolling updates: updates application workloads gradually and can support rollbacks.
- Service discovery and load balancing: provides stable ways for workloads to communicate.
- Configuration and storage management: provides resources for application configuration and persistent data.

Important: Kubernetes does not automatically fix application bugs or update application code by itself. It manages the workloads according to the configuration and desired state I provide.

## 2. A Brief History

Google operated large-scale container workloads internally and developed systems such as Borg. Experience from operating those systems influenced Kubernetes.

A simplified timeline:

- Borg: Google's internal cluster-management system and an important source of experience.
- Omega: a later Google cluster-management research system exploring different scheduling and control approaches.
- 2014: Google announced Kubernetes as an open-source project.
- 2015: Kubernetes 1.0 was released, and the project was contributed to the Cloud Native Computing Foundation (CNCF).

Kubernetes was influenced by Google's earlier work; it is not simply a renamed version of Borg or Omega.

## 3. Evolution of Application Deployment

### Traditional / Physical Server Deployment

Applications run directly on physical servers. Hardware resources are often tied to specific workloads, and scaling can require additional physical machines.

### Virtual Machine Deployment

A hypervisor allows multiple virtual machines to run on one physical server. Each VM has its own operating system environment.

### Container Deployment

Containers package an application and its dependencies while sharing the host operating-system kernel. Docker is a widely used platform for building and running containers.

### Container Orchestration

When many containers must run across multiple machines, an orchestration platform helps schedule, scale, monitor, and manage them. Kubernetes is one such platform.

| Model | Main idea |
|---|---|
| Physical servers | Applications run on physical machines |
| Virtual machines | Multiple isolated VMs run on a host through a hypervisor |
| Containers | Applications run in isolated containers |
| Kubernetes | Containerized workloads are orchestrated across a cluster |

## 4. Kubernetes Cluster Architecture

A Kubernetes cluster is a group of machines, called nodes, that work together to run containerized applications.

A cluster has two main parts:

1. Control plane: makes cluster-wide decisions and manages the desired state.
2. Worker nodes: run application workloads inside Pods.

The terms "master node" and "worker node" are common in older learning material. The current preferred term is "control plane."

### Control Plane Components

| Component | Responsibility |
|---|---|
| kube-apiserver | Exposes the Kubernetes API. Components and clients communicate with the cluster through the API server. |
| etcd | Stores Kubernetes cluster state and configuration data. |
| kube-scheduler | Selects an appropriate node for newly created, unscheduled Pods. |
| kube-controller-manager | Runs controllers that help move the cluster toward the desired state. |
| cloud-controller-manager | Integrates with a cloud provider when that integration is needed. |

### Worker Node Components

| Component | Responsibility |
|---|---|
| kubelet | Runs on each node and makes sure the Pods assigned to that node are running as expected. |
| Container runtime | Pulls images and runs containers. Examples include containerd and CRI-O. |
| kube-proxy | Provides Service-related network forwarding on many clusters; some networking implementations replace this functionality. |

### How kubectl Works

`kubectl` is the command-line client used to communicate with the Kubernetes API.

A simplified flow:

1. I run a command with `kubectl`.
2. `kubectl` sends an API request to the kube-apiserver.
3. The API server validates and processes the request and stores relevant state in etcd.
4. Kubernetes controllers and the scheduler work to make the cluster match the desired state.
5. The kubelet on the selected worker node works with the container runtime to run the Pod.

The API server is the central communication point; `kubectl` is a client, not a cluster controller.

## 5. Pods

A Pod is the smallest deployable unit in Kubernetes.

- A Pod contains one or more containers.
- Containers in the same Pod share the Pod's network namespace and can communicate through localhost.
- Containers in a Pod can share storage volumes.
- A Pod is scheduled onto one worker node; its containers are not individually scheduled across different nodes.

Most Pods contain one application container. Multiple containers are used when they need to work closely together, such as an application container and a helper/sidecar container.

## 6. Kubernetes Installation Options

I learned about three common tools for creating Kubernetes environments.

| Tool | Purpose |
|---|---|
| Minikube | Runs a local Kubernetes cluster, commonly for learning and development. |
| kind | Runs Kubernetes clusters using Docker container nodes; useful for local development and testing. |
| kubeadm | Helps bootstrap and configure Kubernetes clusters; commonly used for self-managed cluster setup. |

These tools serve different purposes. Minikube and kind are convenient local learning options, while kubeadm is a cluster bootstrap tool rather than a managed Kubernetes service.

### Minikube Practice

Today I:

- Installed Minikube.
- Started exploring the local Kubernetes environment.
- Checked the Minikube cluster and profile.
- Ran an Nginx workload inside the local cluster.

## 7. Commands

The following are useful commands for the topics practiced today. Some are reference commands for revision; they are not a claim that every command was run during the session.

### Minikube

```bash
# Check Minikube status
minikube status

# List Minikube profiles
minikube profile list

# Start the default local cluster
minikube start
```

### kubectl

```bash
# Show cluster nodes
kubectl get nodes -o wide

# Show Pods in the current namespace
kubectl get pods

# Show Pods across all namespaces
kubectl get pods -A

# Describe the Minikube node
kubectl describe node minikube

# Run an example Nginx Pod
kubectl run nginx --image=nginx

# Check the Nginx Pod status
kubectl get pods
```

### Inspect the Container Runtime

The node's container runtime can be inspected from inside the Minikube node:

```bash
# Enter the Minikube node
minikube ssh

# Show runtime information
crictl info

# List running containers known to the CRI runtime
crictl ps

# List images known to the CRI runtime
crictl images
```

To check the runtime version through Kubernetes:

```bash
kubectl describe node minikube | grep -i "Container Runtime"
```

### Docker vs. crictl

- `docker ps` lists containers managed by the Docker Engine selected by the Docker CLI context.
- `crictl ps` lists containers known to a CRI-compatible runtime.
- A Minikube node may use containerd even when Minikube itself was created with the Docker driver.
- Therefore, `docker ps` does not necessarily show the Kubernetes workload containers inside the node.

## 8. When Should I Use Docker or Kubernetes?

Docker is useful for building images and running containers, especially for local development and simpler deployments.

Kubernetes becomes useful when an application needs orchestration across multiple nodes, declarative workload management, service discovery, scaling, rolling updates, and self-healing.

Kubernetes adds operational complexity and infrastructure overhead. It is not necessary for every small application or every single-container deployment.

## 9. Monolithic and Microservices Architecture

### Monolithic Architecture

A monolithic application is built and deployed as one main unit. Its features may be organized into modules, but they are typically packaged and released together.

It can be a good choice when an application is small, the team is small, and the system does not need independent service deployment.

### Microservices Architecture

A microservices application is split into smaller services that can be developed, deployed, and scaled independently. This can improve flexibility, but it also introduces distributed-system complexity.

Kubernetes can run both monolithic and microservices applications. Kubernetes does not require an application to use microservices.

## 10. Managed Kubernetes Services

Cloud providers offer managed Kubernetes services so users do not have to operate every part of the cluster themselves.

| Provider | Service | Full name |
|---|---|---|
| AWS | EKS | Amazon Elastic Kubernetes Service |
| Google Cloud | GKE | Google Kubernetes Engine |
| Microsoft Azure | AKS | Azure Kubernetes Service |

The cloud provider manages important parts of the Kubernetes control plane. Users are still responsible for their workloads, configurations, access, and many operational decisions.

## 11. Key Takeaways

- Kubernetes is an open-source container orchestration platform.
- A cluster contains a control plane and worker nodes.
- The API server is the main entry point for Kubernetes API requests.
- etcd stores cluster state.
- The scheduler assigns Pods to nodes.
- kubelet works on each node to keep assigned Pods running.
- A Pod can contain one or more closely related containers.
- Minikube, kind, and kubeadm are different Kubernetes setup tools.
- EKS, GKE, and AKS are managed Kubernetes services.
- Kubernetes is powerful, but it is not required for every application.

## 12. Next Learning Topics

- [ ] Pods in more detail
- [ ] Labels and Selectors
- [ ] ReplicaSets
- [ ] Deployments
- [ ] Services
- [ ] Namespaces
- [ ] ConfigMaps and Secrets
- [ ] Requests, limits, and resource management
- [ ] Volumes and persistent storage
- [ ] Ingress and networking
- [ ] Health probes
- [ ] Scaling and rolling updates

## References

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Kubernetes Concepts](https://kubernetes.io/docs/concepts/)
- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- [kind Documentation](https://kind.sigs.k8s.io/)
- [kubeadm Documentation](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)
