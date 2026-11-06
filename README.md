<p align="center">
  <img src="https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.svg" alt="Kubernetes Logo" width="110" />
</p>

<h1 align="center">Kubernetes</h1>

<p align="center">
  Learning Kubernetes through hands-on practice
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Progress-Day%2005-6A5ACD?style=flat-square" alt="Progress: Day 05" />
</p>

---

## Learning Progress

| Day | Topics | Status |
|---|---|---|
| Day 01 | Kubernetes fundamentals, architecture, deployment models, Minikube installation, and Nginx Pod practice | Completed |
| Day 02 | Kind setup, cluster and node management, kubectl, YAML manifests, imperative and declarative approaches, namespaces, and service ports | Completed |
| Day 03 | Minikube command revision, Minikube vs Kind, imperative and declarative workflows, YAML manifests, and Kubernetes workload resources | Completed |
| Day 04 | AWS EC2 two-node kubeadm cluster, containerd runtime, Calico CNI, Worker node joining, Task Manager Deployment, replicas, and NodePort access | Completed |
| Day 05 | DeployWatch on Amazon EKS, Docker image optimization, GitHub Actions CI/CD, AWS OIDC, Kubernetes RBAC, LoadBalancer, and live workload monitoring | Completed |

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


### Problems Faced & Solutions

- **Challenge:** Understanding how the control plane, worker nodes, and Pods work together.
- **Solution:** Reviewed the Kubernetes architecture and deployment models, then practiced running an Nginx Pod on Minikube.

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


### Problems Faced & Solutions

- **Challenge:** Understanding how Kind cluster configuration, Kubernetes YAML resources, namespaces, and service ports fit into the workflow.
- **Solution:** Practiced creating and inspecting Kind clusters, applying Namespace/Deployment manifests, and checking resources with kubectl.

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

### Problems Faced & Solutions

- **Challenge:** Confusing a Kind cluster configuration YAML with a Kubernetes resource manifest.
- **Solution:** Clarified that Kind configuration is used with `kind create cluster --config`, while Kubernetes resources are managed with `kubectl apply -f`.

## Day 04 - Learning Summary

- Moved from local Kubernetes environments to a real multi-node cluster on AWS EC2.
- Created two Ubuntu 24.04 EC2 instances in the same VPC and Security Group: one Control Plane (Master) and one Worker node.
- Prepared both nodes for Kubernetes by disabling swap, loading the `overlay` and `br_netfilter` kernel modules, and enabling the required networking sysctl settings.
- Installed and configured containerd as the container runtime, including enabling the systemd cgroup driver.
- Installed Kubernetes components: kubeadm, kubelet, and kubectl (v1.37.2).
- Initialized the Control Plane with kubeadm and configured the local kubeconfig file.
- Installed Calico v3.33.0 as the CNI plugin to provide Pod networking.
- Generated a kubeadm join command on the Control Plane and joined the Worker node to the cluster.
- Verified that both nodes reached the `Ready` state.
- Created a dedicated `kubernetes-cluster` Namespace and deployed the Task Manager application using the `bongodev/task-manager:latest` image with two replicas.
- Exposed the Deployment through a NodePort Service on port `30080`.
- Verified Pod placement, Service endpoints, and HTTP connectivity from the Control Plane to the Worker node.
- Accessed the application from the local browser through an SSH tunnel connected to the Control Plane.

### Commands Practiced

Node preparation (run on both Master and Worker):

```bash
# Disable swap
sudo swapoff -a

# Configure kernel modules
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# Configure Kubernetes networking requirements
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system

# Check loaded kernel modules
lsmod | grep br_netfilter
lsmod | grep overlay
```

Containerd runtime setup (run on both nodes):

```bash
# Generate the containerd configuration
containerd config default | sudo tee /etc/containerd/config.toml

# Enable the systemd cgroup driver
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml

# Restart and verify containerd
sudo systemctl restart containerd
sudo systemctl status containerd
containerd --version
```

Kubernetes Control Plane initialization (run only on Master):

```bash
# Initialize the Control Plane with the Pod network CIDR
sudo kubeadm init --pod-network-cidr=192.168.0.0/16

# Configure kubectl access for the current user
mkdir -p "$HOME"/.kube
sudo cp -i /etc/kubernetes/admin.conf "$HOME"/.kube/config
sudo chown "$(id -u)":"$(id -g)" "$HOME"/.kube/config

# Generate the Worker join command
kubeadm token create --print-join-command
```

Calico CNI installation (run on Master):

```bash
# Create the Calico custom resource definitions
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.33.0/manifests/v1_crd_projectcalico_org.yaml

# Install the Tigera operator
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.33.0/manifests/tigera-operator.yaml

# Apply the Calico custom resources
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.33.0/manifests/custom-resources.yaml

# Check cluster nodes and system Pods
kubectl get nodes
kubectl get pods -A
```

Worker node join (run on Worker using the generated command from Master):

```bash
# Paste the generated kubeadm join command and include the containerd CRI socket
sudo kubeadm join <MASTER_PRIVATE_IP>:6443 --token <TOKEN> --discovery-token-ca-cert-hash sha256:<HASH> --cri-socket unix:///run/containerd/containerd.sock
```

Application deployment and Service (run on Master):

```bash
# Create a Namespace
kubectl create namespace kubernetes-cluster

# Deploy two Task Manager replicas
kubectl create deployment task-manager --image=bongodev/task-manager:latest --replicas=2 -n kubernetes-cluster

# Expose the Deployment through a NodePort Service
kubectl expose deployment task-manager --name=task-manager-service --type=NodePort --port=80 --target-port=80 -n kubernetes-cluster

# Set the NodePort to 30080
kubectl patch service task-manager-service -n kubernetes-cluster -p '{"spec":{"ports":[{"port":80,"targetPort":80,"nodePort":30080}]}}'

# Inspect the cluster and application resources
kubectl get nodes -o wide
kubectl get pods -n kubernetes-cluster -o wide
kubectl get svc -n kubernetes-cluster
kubectl get endpoints -n kubernetes-cluster

# Test the application through the Worker's private IP
curl -I --connect-timeout 5 http://<WORKER_PRIVATE_IP>:30080
```

SSH tunnel for browser access (run on the local machine, not on an EC2 node):

```bash
# Forward local port 30080 through the Master to the Worker's private NodePort
ssh -i <PATH_TO_KEY.pem> -L 30080:<WORKER_PRIVATE_IP>:30080 ubuntu@<MASTER_PUBLIC_IP>
```

Keep the SSH session open, then visit `http://localhost:30080` in the local browser.

### Problems Faced & Solutions

- **Challenge:** The Worker node could not initially communicate with the Kubernetes API Server.
- **Solution:** Reviewed the AWS Security Group configuration and allowed the required Kubernetes API traffic (TCP port 6443) within the VPC.
- **Challenge:** The Control Plane initially showed the `NotReady` status after `kubeadm init`.
- **Solution:** Installed and configured Calico as the CNI plugin. After the networking components became healthy, both nodes reached the `Ready` state.
- **Challenge:** The Task Manager was reachable through the Worker's private IP from the Control Plane, but direct access through the Worker's public IP timed out. The home public IP also changed during troubleshooting, making a fixed /32 Security Group allowlist unreliable.
- **Solution:** Verified the NodePort internally and used an SSH local port-forward through the Master node. The application then became accessible at `http://localhost:30080` without opening the NodePort to the entire internet.
- **Learning:** The local machine's Ubuntu version does not need to match the EC2 Ubuntu version for this SSH tunnel workflow; the important parts are the remote cluster configuration, network path, and SSH connectivity.

## Day 05 - Learning Summary

- Moved from a self-managed EC2 kubeadm cluster to a managed Kubernetes environment using Amazon EKS.
- Deployed **DeployWatch**, a Flask-based Kubernetes workload monitoring dashboard, to the existing `full-stack-cluster` EKS cluster in the `ap-south-1` region.
- Built and optimized the application container using `python:3.12-slim`, environment settings for Python containers, and a `.dockerignore` file to reduce unnecessary build context and image size.
- Implemented a GitHub Actions CI/CD workflow that builds the DeployWatch Docker image, tags it with the Git commit SHA, pushes it to Docker Hub, updates the EKS Deployment, and verifies the rollout.
- Configured GitHub Actions authentication to AWS using OIDC and an IAM role, avoiding long-lived AWS access keys in GitHub Actions.
- Restricted the OIDC trust policy to the intended GitHub repository and the `main` branch.
- Configured EKS access for the CI role and namespace-scoped Kubernetes RBAC so the pipeline can update only the `two-tier-app` Deployment in the `two-tier-ns` namespace.
- Configured the dashboard to read Kubernetes resources using in-cluster configuration and a read-only ServiceAccount/RBAC access model.
- Exposed the application through a Kubernetes LoadBalancer Service and accessed the dashboard using its AWS-provided external DNS hostname.
- Verified the deployed image tag against the successful GitHub Actions commit and confirmed the new application Pod reached `1/1 Running`.
- Verified the live dashboard reported 3 ready Deployments, 9 Running Pods, 5 Namespaces, and 100% Deployment health at the time of the check.
- Explored Kubernetes Events to confirm the new image was pulled and the updated container was created and started.

### Technology Stack

- **Cloud & Kubernetes:** AWS, Amazon EKS, Kubernetes, AWS LoadBalancer
- **Containerization:** Docker, Docker Hub, Python 3.12-slim
- **CI/CD & Identity:** GitHub Actions, AWS IAM, GitHub OIDC
- **Application:** Python, Flask, Kubernetes Python Client, HTML, CSS, JavaScript
- **Access Control:** Kubernetes ServiceAccount, Role, RoleBinding, read-only RBAC, namespace-scoped deployment permissions

### Commands Practiced

Inspect DeployWatch workloads:

```bash
# List Pods with node and IP details
kubectl get pods -n two-tier-ns -o wide

# List Deployments and replica readiness
kubectl get deployments -n two-tier-ns

# List Services and the external LoadBalancer hostname
kubectl get svc -n two-tier-ns

# Inspect the application Deployment image
kubectl get deployment two-tier-app -n two-tier-ns \
  -o=jsonpath='{.spec.template.spec.containers[?(@.name=="two-tier-app")].image}{"\\n"}'

# Verify the Deployment rollout
kubectl rollout status deployment/two-tier-app -n two-tier-ns
```

### CI/CD Workflow

1. GitHub Actions checks out the repository after a push to `main`.
2. The workflow authenticates to AWS using GitHub OIDC and the dedicated IAM role.
3. Docker Buildx builds the application image from `deploywatch-source`.
4. The image is pushed to Docker Hub with the Git commit SHA as an immutable tag.
5. The workflow updates the `two-tier-app` Deployment in the `two-tier-ns` namespace.
6. Kubernetes performs the rolling update, and GitHub Actions waits for the rollout to complete.

### Problems Faced & Solutions

- **Challenge:** The first GitHub Actions run could not assume the AWS IAM role through OIDC and failed with `Not authorized to perform sts:AssumeRoleWithWebIdentity`.
- **Solution:** Inspected the GitHub OIDC provider and IAM role trust policy, identified the repository/branch subject mismatch, updated the trust policy to match the intended GitHub repository and `main` branch, and verified the next workflow run succeeded.
- **Challenge:** The initial application Docker image was unnecessarily large.
- **Solution:** Switched to the `python:3.12-slim` base image, reviewed the container build context, and added a `.dockerignore` file. The optimized image content size was approximately 79.2 MB compared with approximately 455 MB previously.
- **Challenge:** The GitHub Actions deployment role needed Kubernetes permissions without receiving broad cluster-admin access.
- **Solution:** Added an EKS access entry and mapped the role to a dedicated Kubernetes group. Created a namespace-scoped Role and RoleBinding that allow the CI identity to update only the `two-tier-app` Deployment in `two-tier-ns`.
- **Challenge:** The dashboard needed access to Kubernetes resources while keeping monitoring access read-only.
- **Solution:** Used a dedicated ServiceAccount with read-only RBAC permissions for the required Kubernetes resources.
- **Challenge:** It was necessary to confirm that CI/CD deployed the newly built image rather than merely completing the workflow.
- **Solution:** Compared the Deployment's active image tag with the GitHub commit SHA, checked the Pod status, and reviewed Kubernetes Events for successful image pull, container creation, and startup.

### Key Learning

This hands-on deployment connected containerization, CI/CD automation, cloud identity, Kubernetes access control, rolling updates, and workload monitoring into one end-to-end Amazon EKS workflow.

---

<p align="center">
  Learning by doing • One concept at a time
</p>
