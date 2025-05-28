# 🚢 Docker – Limitations of a Single-Host Container System
1. **Docker is a tool used to containerize applications.**
   It packages code, dependencies, and environment together so applications can run consistently across environments.
2. **Docker runs containers on a single host machine.**
   It is **not distributed** and introduces a **single point of failure** –
   If the Docker host goes down, **all running containers will stop**.
3. **Containers are ephemeral and each gets its own IP address.**
   * They are **temporary** by default – deleted when stopped unless configured otherwise.
   * Exposing them to the outside world requires **manual port mapping**.
   * This makes **networking and service discovery complex** in larger systems.
4. **Storage issues in containers**
   * By default, containers use **ephemeral storage**, which means:
     * **Data inside the container is lost** if the container crashes or restarts.
     * There's no built-in mechanism to **persist data across restarts**.
   * Solution like **volumes or bind mounts** is needed, but managing storage at scale is complex.
5. **Containers do not auto-heal on failure.**
   * If a container crashes, Docker doesn't automatically restart or replace it unless configured with restart policies.
   * There is **no built-in self-healing** mechanism for failed applications.
6. **Docker lacks built-in features like load balancing and auto-scaling.**
   * Applications may support those features internally, but Docker alone **does not provide cluster-level orchestration**.
   * You need tools like **Kubernetes, Docker Swarm**, or **ECS** to enable:
     * Load balancing
     * Auto-scaling
     * Health checks
     * Self-healing
     * Service discovery
---
# K8s Features
- refer official documentation
---
## 🌐 Kubernetes (K8s) Architecture
Kubernetes is a **powerful container orchestration platform** designed to manage distributed systems across a cluster of machines. The architecture consists of:
* **Master Nodes** – Control and manage the cluster.
* **Worker Nodes** – Run the application workloads.
---
### 🧠 Master Node – Brain of the Cluster
Master nodes are responsible for **cluster management**, **orchestration**, and **scheduling** of workloads. Key responsibilities:
* Pod scheduling & placement
* Cluster configuration storage
* Health checks and auto-healing
* Ensuring high availability
#### 🔧 Components of a Master Node
1. **API Server (`kube-apiserver`)**
   * **Central communication hub** for the cluster
   * Accepts and validates REST requests (e.g., kubectl)
   * Performs authentication & authorization
   * The **only component that interacts with all others**
2. **ETCD**
   * **Key-value store** (distributed, consistent)
   * Stores **entire cluster state** and configuration (e.g., YAMLs, secrets, objects)
   * Acts as the **single source of truth**
3. **Scheduler**
   * Responsible for **pod placement**
   * Uses a **filtering and scoring algorithm** to pick the best node
   * Considers resource requirements, taints, affinities, etc.
4. **Controller Manager**
   * Runs multiple **controllers** to manage cluster state
   * Examples:
     * **ReplicaSet Controller** – Maintains desired number of pods
     * **Deployment Controller** – Handles rollout & rollback of deployments
     * **ServiceEndpoints Controller** – Manages endpoints of services
5. **Cloud Controller Manager**
   * Interacts with **cloud provider APIs**
   * Manages:
     * Load balancers (e.g., **ALB Ingress Controller**)
     * DNS records
     * Cloud storage and nodes
---
### 🏃‍♂️ Worker Node – Execution Engine
Worker nodes are where **applications run**. They receive instructions from master nodes and manage the **containerized workloads**.
#### 🔩 Components of a Worker Node
1. **Kubelet**
   * Communicates with the API Server
   * Ensures containers are running as expected
   * Reports pod status back to the master
2. **Kube-proxy**
   * Manages **network traffic routing** to the correct pod/service
   * Applies **iptables** or **IPVS** rules
3. **Container Runtime Interface (CRI)**
   * Manages **container lifecycle**
   * Examples: containerd, Docker (deprecated), CRI-O
📌 *Note: Master nodes can also run workloads, so they contain worker node components as well.*
---
### 🏗 Cluster Summary
| Role            | Key Components                                                    | Purpose                            |
| --------------- | ----------------------------------------------------------------- | ---------------------------------- |
| **Master Node** | API Server, ETCD, Scheduler, Controller Manager, Cloud Controller | Control plane management           |
| **Worker Node** | Kubelet, Kube-proxy, CRI                                          | Run and manage container workloads |
---
# kubeadm cluster
# Kubernetes Namespaces
**Namespaces** provide isolation for Kubernetes objects like **Pods**, **Services**, and **Deployments**.
They help organize and manage cluster resources efficiently and enable **multitenancy** within a single cluster.
---
## 🧵 Default Namespaces in Kubernetes
Kubernetes includes the following **4 default namespaces**:
* **`kube-system`** – Contains system components (e.g., `kube-dns`, `kube-proxy`).
* **`default`** – Used for objects created without specifying a namespace.
* **`kube-public`** – Holds publicly accessible data (e.g., via `kubectl cluster-info` `kubectl cluster-info dump`).
* **`kube-node-lease`** – Manages node heartbeats using lease objects to monitor node availability.
---
## 🔧 Namespace Management Commands
```bash
# Create a namespace
kubectl create ns siva
# Describe a namespace
kubectl describe ns siva
# Delete a namespace
kubectl delete ns siva
```
---
## 📄 YAML Example
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: siva
```
---
## 📌 Use Cases of Namespaces
* **Isolate resources** of different teams or projects – supports multitenancy.
* **Avoid naming conflicts** – same object names can exist in different namespaces.
* **Control resource usage** – by applying `ResourceQuotas`.
* **Access control** – enforce `RBAC` policies to restrict access per namespace.
---
✅ **Tip:** Always group related resources using namespaces for better **organization and management**.
---
