# 🧱 Pod – The Smallest Deployable Unit in K8s
1. **Pod is a deployable unit**, not directly runnable; it's scheduled by the control plane.
2. A **Pod wraps one or more containers**, but usually it's **one Pod = one container**.
3. **Single IP per Pod** – Managed by the **pause container** which holds the network namespace.
4. **Containers share storage and network** – Can communicate via `localhost` and use shared volumes.
5. **Pods don't auto-recover** – A Pod alone won’t restart itself if it fails (use Deployments for that).
---
## 🧪 Pod Commands
```bash
# Create a pod
kubectl run <pod-name> --image=nginx
# Get list of pods
kubectl get pods
# Get pod details with node and IP info
kubectl get pods -o wide
# Describe a pod (deep inspection)
kubectl describe pod <pod-name>
# View pod logs
kubectl logs <pod-name>
# Access pod terminal
kubectl exec -it <pod-name> -- bash
# or if bash is not available
kubectl exec -it <pod-name> -- sh
```
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
```
---
# 🔖 Labels & Selectors in Kubernetes
1. **Labels** are `key=value` pairs attached to K8s objects (like Pods, Services, etc.).
2. **Selectors** are used to **filter** and **connect** K8s objects based on labels.
3. Example: A **Service** can target Pods with label `app=backend`.
4. **Types of Selectors**:
   * **Equality-based** (`=`, `==`, `!=`)
   * **Set-based** (`in`, `notin`, `exists`)
---
## ✅ Equality-based Selector Example
```yaml
selector:
  matchLabels:
    app: backend
```
🔎 Matches Pods that have the label `app=backend`.
---
## ✅ Set-based Selector Example
```yaml
selector:
  matchExpressions:
    - key: app
      operator: In
      values: [frontend, backend]
```
🔎 Matches Pods with `app=frontend` or `app=backend`.
---
### ✅ **Assign (Add or Update) Labels**
```bash
kubectl label pod <pod-name> <key>=<value>
```
**Example:**
```bash
kubectl label pod nginx-pod environment=dev
```
* This adds the label `environment=dev` to the pod named `nginx-pod`.
* If the label already exists, it will update the value.
---
### ❌ **Remove (Unlabel) a Label**
```bash
kubectl label pod <pod-name> <key>-
```
**Example:**
```bash
kubectl label pod nginx-pod environment-
```
* This removes the `environment` label from the pod.
---
### 🔍 To View Labels
```bash
kubectl get pod <pod-name> --show-labels
```
Here’s a **clean and structured version** of your notes on **ReplicaSet vs ReplicationController**, along with correct usage guidance:
---
## 📦 ReplicaSet vs ReplicationController in Kubernetes
### 🔄 Similarities:
* Both ensure that the **desired number of identical Pods** are running at any given time.
* Both automatically **reschedule** Pods if any fail or are deleted.
---
### 🆕 **ReplicaSet (RS)** – Modern Controller
* **Replaces ReplicationController** in modern Kubernetes versions.
* Supports both:
  * ✅ **Equality-based selectors** (`app=nginx`)
  * ✅ **Set-based selectors** (`app in (nginx, apache)`)
* Used by **Deployments** to manage pod replicas.
* Supports advanced strategies like:
  * ✅ Rolling updates (via Deployment)
  * ✅ Rollbacks (via Deployment)
---
### 🧓 **ReplicationController (RC)** – Legacy Controller
* Older version of replication mechanism.
* ❌ **Does not support set-based selectors**
* ❌ No native support for rolling updates or rollbacks
* Rarely used in modern Kubernetes setups.
---