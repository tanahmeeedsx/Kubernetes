![Kubernetes](https://img.shields.io/badge/Kubernetes-Learning-326CE5?logo=kubernetes&logoColor=white)

# Kubernetes Fundamentals

A hands-on learning repository documenting my journey into Kubernetes, starting with core concepts, architecture, local cluster setup, and basic workload practice.

The goal is to keep clear notes, useful commands, and practical examples in one place so I can revise concepts and track my progress over time.

## Learning Progress

| Day | Topic | Status |
|---|---|---|
| Day 01 | Kubernetes fundamentals, architecture, deployment models, Minikube setup, and Nginx practice | Completed |

## Repository Structure

```text
k8s-fundamentals/
├── README.md
└── docs/
    └── day-01-fundamentals.md
```

## Day 01 - What I Learned

- What Kubernetes is and why container orchestration is useful.
- Core capabilities: scheduling, self-healing, scaling, and workload management.
- A brief history of Kubernetes and its roots in Google's Borg experience.
- The evolution from physical servers to virtual machines, containers, and orchestration.
- The difference between a Kubernetes cluster, control plane, and worker nodes.
- The roles of the API server, scheduler, controller manager, etcd, kubelet, and Pods.
- Local Kubernetes options: Minikube, kind, and kubeadm.
- Managed Kubernetes services: Amazon EKS, Google GKE, and Azure AKS.
- Installed Minikube, explored the local cluster, and ran an Nginx workload.

Detailed notes and commands: [Day 01 - Kubernetes Fundamentals](docs/day-01-fundamentals.md)

## Useful Commands

Check Minikube:

```bash
minikube status
minikube profile list
```

Inspect the cluster:

```bash
kubectl get nodes -o wide
kubectl get pods -A
```

Run a simple Nginx Pod (example):

```bash
kubectl run nginx --image=nginx
kubectl get pods
```

Inspect the container runtime from the Minikube node:

```bash
minikube ssh
crictl info
crictl ps
crictl images
```

> Note: `docker ps` lists containers managed by the Docker Engine it is connected to. Kubernetes workload containers may be managed by containerd inside the Minikube node, so use `crictl` from inside the node to inspect them. The Minikube driver and the node's container runtime are related but different concepts.

## Learning Roadmap

- [ ] Pods and multi-container Pods
- [ ] Labels and Selectors
- [ ] Deployments and ReplicaSets
- [ ] Services and service discovery
- [ ] ConfigMaps and Secrets
- [ ] Namespaces and resource requests/limits
- [ ] Volumes and persistent storage
- [ ] Ingress and networking
- [ ] Health probes and rollout strategies
- [ ] Horizontal Pod Autoscaling
- [ ] Helm and package management

## Cloud Kubernetes Services

| Cloud Provider | Managed Kubernetes Service |
|---|---|
| AWS | Amazon Elastic Kubernetes Service (EKS) |
| Google Cloud | Google Kubernetes Engine (GKE) |
| Microsoft Azure | Azure Kubernetes Service (AKS) |

## Notes

- These are personal learning notes, not official Kubernetes documentation.
- Commands are examples for a local Minikube environment. Review the context and resource names before running commands.
- I will add future topics and screenshots as I continue practicing.

## References

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- [Kubernetes Concepts](https://kubernetes.io/docs/concepts/)
