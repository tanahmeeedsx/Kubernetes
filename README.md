<p align="center">
  <img src="https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.svg" alt="Kubernetes Logo" width="110" />
</p>

<h1 align="center">Kubernetes</h1>

<p align="center">
  Learning Kubernetes through hands-on practice
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Progress-Day%2002-6A5ACD?style=flat-square" alt="Progress: Day 02" />
</p>

---

## Learning Progress

| Day | Topics | Status |
|---|---|---|
| Day 01 | Kubernetes fundamentals, architecture, deployment models, Minikube installation, and Nginx Pod practice | Completed |
| Day 02 | Kind setup, cluster and node management, kubectl, YAML manifests, imperative and declarative approaches, namespaces, and service ports | Completed |

## Day 01 - Learning Summary

- Understood what Kubernetes is and why container orchestration is needed.
- Explored automated scheduling, self-healing, scaling, and workload management.
- Learned the high-level history of Kubernetes, including Google's Borg and Omega systems.
- Compared physical server, virtual machine, container, and Kubernetes-based deployment models.
- Studied cluster architecture, control plane components, worker nodes, and Pods.
- Learned about Minikube, kind, and kubeadm.
- Explored managed Kubernetes services: Amazon EKS, Google GKE, and Azure AKS.
- Installed Minikube and practiced running an Nginx workload in a local cluster.

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

---

<p align="center">
  Learning by doing • One concept at a time
</p>
