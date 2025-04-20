# 🧭 What is Kubernetes?

Kubernetes (also called k8s) is an open-source platform to automate deploying, scaling, and managing containerized applications. Think of it like an operating system for your data center.

Imagine you have 100 containers running your app. Kubernetes helps you:

- Start them on different machines (nodes)
- Keep them alive (self-healing)
- Scale them up or down automatically
- Handle traffic
- Store persistent data

Kubernetes abstracts away the underlying machines and gives you a clean, declarative way to run apps.

---

## 🧱 1. Core Concepts

### Cluster
- A **Cluster** is a group of machines (nodes) running containers.
- It consists of a **Control Plane** (manages the cluster) and **Nodes** (run workloads).

### Node
- A **Node** is a physical or virtual machine in the cluster.
- It provides the resources (CPU, memory, storage) needed to run applications.
- Each node runs:
  - **Kubelet**: Ensures containers are running in pods.
  - **Kube-proxy**: Manages networking for pods.

### Pod
- A **Pod** is the smallest deployable unit in Kubernetes.
- It represents one or more containers that share:
  - Network namespace (IP address).
  - Storage volumes.
- Pods are ephemeral and are managed by higher-level controllers like Deployments.

### Deployment
- A **Deployment** ensures that the desired number of pod replicas are running.
- It manages updates to pods and their underlying containers.
- Example use case: Rolling updates to deploy a new version of an application.

### Service
- A **Service** provides a stable network endpoint to access pods.
- It abstracts the dynamic nature of pods and offers:
  - **ClusterIP**: Internal communication within the cluster.
  - **NodePort**: Exposes the service on a specific port of each node.
  - **LoadBalancer**: Exposes the service externally using a cloud provider's load balancer.

### ConfigMap & Secret
- **ConfigMap**: Injects configuration data (non-sensitive) into pods.
- **Secret**: Injects sensitive data (e.g., passwords, API keys) into pods securely.

### Volume
- A **Volume** is a storage resource attached to a pod.
- It persists data beyond the lifecycle of a container but not beyond the lifecycle of the pod.

### Namespace
- A **Namespace** is a virtual cluster within a physical cluster.
- It is used for organizing resources and enabling multi-tenancy.

### ReplicaSet
- A **ReplicaSet** ensures a specified number of pod replicas are running at all times.
- It is typically managed by a Deployment.

### StatefulSet
- A **StatefulSet** is used for stateful applications (e.g., databases).
- It provides stable network identities and persistent storage for each pod.

### DaemonSet
- A **DaemonSet** ensures that a copy of a pod runs on all (or specific) nodes.
- Example use case: Running log collection or monitoring agents on every node.

### Job
- A **Job** runs a pod to completion.
- It is used for batch processing tasks.

### CronJob
- A **CronJob** schedules jobs to run at specific times or intervals.
- Example use case: Running a backup script every night.

### Horizontal Pod Autoscaler
- Automatically scales the number of pods based on CPU/memory usage or custom metrics.

### Custom Resource Definitions (CRDs)
- CRDs allow you to define your own resource types in Kubernetes.
- They extend Kubernetes' functionality to manage custom applications.

### Kubelet
- A **Kubelet** is an agent that runs on each node.
- It ensures that containers are running in pods as specified by the Control Plane.

### Kube-proxy
- **Kube-proxy** manages network rules on nodes to allow communication between pods and services.

### Control Plane
- The **Control Plane** is the brain of the cluster.
- It manages the state of the cluster and schedules workloads.
- Key components:
  - **API Server**: Exposes the Kubernetes API.
  - **etcd**: Stores cluster state and configuration.
  - **Scheduler**: Assigns pods to nodes.
  - **Controller Manager**: Ensures the desired state of the cluster.

### etcd
- A distributed key-value store used to store all cluster data.
- It is the source of truth for the cluster's state.

### Scheduler
- The **Scheduler** assigns pods to nodes based on resource availability and constraints.

### Controller Manager
- The **Controller Manager** runs controllers that regulate the state of the cluster.
- Example controllers:
  - **Node Controller**: Monitors node health.
  - **Replication Controller**: Ensures the desired number of pod replicas.

### Admission Controller
- Validates and modifies requests to the API server before they are persisted in etcd.

### Network Policy
- A **Network Policy** controls the communication between pods.
- It defines rules for ingress and egress traffic.

### Resource Quotas
- **Resource Quotas** limit the amount of resources (CPU, memory) a namespace can use.

### **LimitRange**
- A **LimitRange** sets default resource requests and limits for containers in a namespace.

### Pod Security Policies
- **Pod Security Policies** control security settings for pods, such as:
  - Privileged access.
  - Host network usage.

### ServiceAccount
- A **ServiceAccount** provides an identity for processes running in a pod to interact with the Kubernetes API.

### RBAC (Role-Based Access Control)
- **RBAC** manages permissions for users and service accounts.
- It defines roles and role bindings to control access to resources.

### Network Plugins
- **Network Plugins** extend Kubernetes networking capabilities using the Container Network Interface (CNI).

---

## 💾 2. Storage & Persistence

### PersistentVolume (PV)
- A **PersistentVolume** is a piece of cluster storage provisioned by an administrator or dynamically.

### PersistentVolumeClaim (PVC)
- A **PersistentVolumeClaim** is a request for storage by a pod.

### StorageClass
- A **StorageClass** defines how storage is provisioned dynamically.

---

## 📦 3. Advanced Workloads

### StatefulSets
- For stateful apps (like databases), gives stable network & storage.

### DaemonSets
- Run one pod per node (e.g., for log collection).

### Jobs & CronJobs
- For batch processing or scheduled tasks.

### Init Containers
- Run before the main container starts.

---

## 🔌 4. Extending Kubernetes

### Custom Resources (CRDs)
- Add new object types to the cluster.

### Operators
- Controllers that manage complex apps using CRDs.

### Admission Controllers
- Hook into request lifecycle to validate/mutate requests.

---

## 🎛️ 5. Configuration & Packaging

### Helm
- The package manager for Kubernetes.
    - **Helm Chart**: A templated set of YAMLs that define a k8s app.
    - Great for reusability and sharing configurations.

### Kustomize
- Built-in tool for customizing YAML configurations.

---

## 🔒 6. Security & Access

### RBAC
- Role-Based Access Control.

### ServiceAccounts
- Access identity for pods.

### Network Policies
- Control pod-to-pod communication.

---

## 📈 7. Observability

### Logs
- `kubectl logs`, or tools like Fluentd, Loki.

### Metrics
- Prometheus, Grafana.

### Tracing
- Jaeger, OpenTelemetry.

### Probes
- Liveness and readiness checks.

---

## ⚙️ 8. CI/CD & GitOps

### ArgoCD / Flux
- GitOps deployment tools.

### Jenkins / GitHub Actions + kubectl
- Push apps to k8s.

### Secrets Management
- Use sealed-secrets, external-secrets, etc.

---

## ☁️ 9. Cloud-Native Ecosystem

### Ingress Controllers
- NGINX, Traefik, or cloud-specific ones.

### Service Mesh
- A dedicated infrastructure layer for managing service-to-service communication.
- Features:
  - Traffic management (routing, load balancing).
  - Observability (metrics, logs, tracing).
  - Security (mTLS).
- Examples: Istio, Linkerd.

### Container Runtimes
- containerd, CRI-O.

---

## 📘 Recommended Learning Order

1. Pods, Deployments, Services ✅
2. ConfigMaps, Secrets, Volumes
3. PVCs & StorageClass
4. Helm charts
5. CRDs & Operators
6. Monitoring & Probes
7. Security basics (RBAC, NetworkPolicy)
8. GitOps or Helm-based CI/CD
9. Advanced topics (Service Mesh, Ingress Controllers)
10. Cloud-native tools (Fluentd, Prometheus, Grafana)
11. Explore the Kubernetes ecosystem (Istio, ArgoCD, etc.)