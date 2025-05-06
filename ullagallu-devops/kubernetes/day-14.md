### ✅ Resource Quotas:
* **Purpose**: To **limit the total resources** (like CPU, memory, number of pods, etc.) that can be used in a **namespace**.
* **Example**: You can say: *"In the `dev` namespace, only 4 CPUs and 8Gi memory can be used in total."*
* It helps prevent one team from consuming all cluster resources.
* Defined using `ResourceQuota` YAML object.
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "6"
    limits.memory: 12Gi
```
---
### ✅ What Are `requests` and `limits`?
#### 🟢 `requests`:
* The **minimum** amount of CPU or memory a container is **guaranteed**.
* The **scheduler** uses this to decide **where to place the pod**.
* If a node doesn't have enough resources for the **request**, the pod won't be scheduled there.
#### 🔴 `limits`:
* The **maximum** amount of CPU or memory a container is **allowed to use**.
* If the container tries to use more than this:
  * For **CPU**, it gets throttled.
  * For **memory**, it can get **killed** (OOMKilled).
---
### 🧠 Example:
```yaml
resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
  limits:
    memory: "512Mi"
    cpu: "500m"
```
👉 This means:
* The container is **guaranteed 250m CPU** and **256Mi memory**.
* It **can’t go beyond 500m CPU** and **512Mi memory**.
---
### 🔍 Verify Resource Usage:
```bash
kubectl describe pod <pod-name> -n <namespace>
```
```bash
kubectl get pod -n <namespace> -o=jsonpath="{range .items[*]}{.metadata.name}{'\t'}{.spec.containers[*].resources}{'\n'}{end}"
```

1. Yes, you can assign multiple ResourceQuotas to a single namespace.
2. If you apply quotas with different names but the same resource (e.g., CPU), Kubernetes will sum them up (e.g., 2 CPU + 2 CPU = 4 CPU total).

---
### ✅ Limit Ranges:

* **Purpose**: To set **default and max/min limits** for **individual containers/pods** in a namespace.
* **Example**: You can say: *"Every container in this namespace should not use more than 500Mi memory and 1 CPU."*
* Prevents developers from creating pods with very high or very low resource requests.
* Defined using `LimitRange` YAML object.
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: dev-limit
  namespace: dev
spec:
  limits:
  - default:
      cpu: 500m
      memory: 512Mi
    defaultRequest:
      cpu: 250m
      memory: 256Mi
    max:
      cpu: 1
      memory: 1Gi
    min:
      cpu: 100m
      memory: 128Mi
    type: Container
```
---
### ⚙️ Difference Between ResourceQuota vs LimitRange:
| Feature  | ResourceQuota                          | LimitRange                        |
| -------- | -------------------------------------- | --------------------------------- |
| Scope    | Namespace-wide                         | Per container/pod                 |
| Controls | Total usage of CPU, memory, pods, etc. | Default, min, max for containers  |
| Use case | Prevent namespace overuse              | Set container-level safety limits |
---
### ✅ To Get ResourceQuotas in a Namespace:
```bash
kubectl get resourcequota -n <namespace>
```
**Example:**
```bash
kubectl get resourcequota -n dev
```
To see detailed info:
```bash
kubectl describe resourcequota <name> -n <namespace>
```
---
### ✅ To Get LimitRanges in a Namespace:
```bash
kubectl get limitrange -n <namespace>
```
**Example:**
```bash
kubectl get limitrange -n dev
```
To describe a specific limit range:
```bash
kubectl describe limitrange <name> -n <namespace>
```
---
Great explanation, Konka! You’ve captured the essence of the Kubernetes scheduler's behavior quite well. Let me break it down further, and I’ll fill in a few additional details:

---
### **Scheduler Phases:**
#### 1. **Filtering Phase:**
* The **scheduler** first needs to filter out nodes that **cannot run** the pod. It checks the following criteria:
  * **Taints**: Ensures that nodes are not tainted in a way that would prevent the pod from being scheduled.
  * **Affinity**: Checks if the node matches the **node affinity** rules defined in the pod (such as location in a specific zone or region).
  * **ResourceQuota**: Ensures that there are enough resources available (e.g., CPU, memory) within the namespace's **ResourceQuota** limits.
  * **Pod Tolerations**: Ensures that the pod can tolerate the taints on the node, if any.
After filtering, only the nodes that meet the pod's requirements move to the next phase.
#### 2. **Scoring Phase:**
* The scheduler ranks the remaining **nodes** based on how well they meet the pod’s resource and affinity requests. The scoring is a process of ranking nodes **from the most suitable to the least suitable**.
* **Compute Resources**: Nodes are ranked based on available CPU and memory. For example:
  * **Node-2**: 4 vCPU, 6GB RAM (Best fit)
  * **Node-1**: 2 vCPU, 4GB RAM (Second best)
  * **Node-3**: 1 vCPU, 1GB RAM (Least preferred)
* The **node with the highest score** becomes the **target node** for pod placement.
#### 3. **Pod Placement**:
* Once the scheduler selects the best node, it **binds the pod to that node** and tells the **API server** to schedule it there.
* The pod is then **scheduled** to run on the chosen node, and the **kubelet** on that node will start the container.
---
### **In Simple Terms**:
* **Filtering**: The scheduler checks for the **availability of nodes** that match the pod’s requirements (taints, affinity, resource quotas, etc.).
* **Scoring**: It then **ranks nodes** based on available resources (CPU, memory).
* **Placement**: Finally, it **assigns** the pod to the best-suited node.
---
### **1. Priority**
Priority in Kubernetes is used to determine the **importance** of pods when multiple pods need to be scheduled. It allows you to define which pods should have a higher chance of being scheduled in case of resource constraints (e.g., when nodes are over-saturated).
* **How it works**:
  * Every pod can have a **priority** value. Pods with **higher priority** are preferred over those with lower priority when resources are limited.
  * You can set **priority** by assigning a **PriorityClass** to the pod.
* **PriorityClass**: This is a Kubernetes resource that defines the priority of pods. It can define a `value` for priority and other settings like `preemptionPolicy`.
  * Example: If you have a critical pod that needs to run (like a database), you can assign it a higher priority.
* **Priority Class Example**:
  ```yaml
  apiVersion: scheduling.k8s.io/v1
  kind: PriorityClass
  metadata:
    name: high-priority
  value: 1000000
  globalDefault: false
  description: "This priority class is for critical applications."
  ```
* **How to use it** in a pod:
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: high-priority-pod
  spec:
    priorityClassName: high-priority
    containers:
    - name: my-container
      image: my-image
  ```
* **Effects**:
  * **Higher priority pods** will be scheduled before **lower priority pods**.
  * If resources are limited, Kubernetes will **evict lower priority pods** to free up resources for higher priority ones.
---
### **2. Preemption**
Preemption is the mechanism by which **Kubernetes evicts lower priority pods** to make room for **higher priority pods** when there are **insufficient resources**.
* **How it works**:
  * If a **high-priority pod** cannot be scheduled because there are not enough resources on a node, Kubernetes will try to **preempt (evict)** lower priority pods from the node to free up resources for the higher priority pod.
  * **Preemption only happens** if the **priority class** of the pod allows it and the resources are needed to schedule the pod.
* **Preemption Policy**:
  * A **preemption policy** determines whether a pod can **evict other pods**. It can be set to either `PreemptLowerPriority` (default) or `Never`.
  * **`PreemptLowerPriority`** means the pod can evict lower priority pods if it cannot be scheduled.
  * **`Never`** means the pod will not preempt other pods, even if it cannot be scheduled immediately.
* **Preemption Example**:
  * Let's say you have two pods:
    1. **Critical Pod** with high priority.
    2. **Low-priority Pod** running on the same node.
  * If the critical pod cannot fit on the node due to insufficient resources, Kubernetes will **evict the low-priority pod** to make room for the critical pod.
---
### **Summary**:
* **Priority** determines which pod should be scheduled first when resources are tight.
* **Preemption** is the process of evicting lower priority pods to **free up resources** for higher priority pods.
---
Great questions, Konka! Let me break down **Pod Eviction** and **Pod Disruption Budgets (PDBs)** for you:
### **1. Pod Eviction**
**Pod Eviction** refers to the process of **removing pods from nodes** in response to certain conditions. Eviction helps maintain the health and stability of the cluster, especially when resources are constrained or when nodes are under pressure.
#### **Types of Pod Eviction**:
* **Voluntary Eviction**: This is when a pod is evicted by the Kubernetes system to make room for other important workloads (like due to resource constraints or due to higher priority pods being scheduled). This could also happen when the node is going under maintenance.
* **Involuntary Eviction**: This happens due to **node pressure** (such as high CPU, memory, or disk usage) or **lack of resources**. Kubernetes will evict pods to avoid overloading the node.
#### **Reasons for Eviction**:
* **Resource Pressure**: When a node is under resource pressure (CPU, memory, or disk) and the pod cannot be scheduled, Kubernetes will evict lower-priority or non-essential pods.
* **Node Maintenance**: Pods may be evicted from a node when the node is going down for maintenance, or the node is being cordoned (marked unschedulable).
* **Taints and Tolerations**: Pods can be evicted when a taint is added to a node, and the pod does not tolerate that taint.
#### **Eviction Policy**:
Kubernetes has an **Eviction API** to handle voluntary evictions. For example, if a pod is using too much memory, it might be evicted if the node is under memory pressure. The system will try to **kill pods that have lower priority** to free up resources.
Example of eviction due to memory pressure:
* If a pod is consuming more memory than the system can handle, and the node has limited memory, Kubernetes will **evict** pods that are less critical (lower priority) to maintain stability.
#### **Eviction Command**:
You can manually evict pods in certain scenarios using the command:
```bash
kubectl delete pod <pod-name> --force --grace-period=0
```
---
### **2. Pod Disruption Budget (PDB)**
A **Pod Disruption Budget (PDB)** is a policy that defines how many **pods in a deployment or stateful set** can be **disrupted** at a time due to voluntary disruptions, like **maintenance, upgrades, or evictions**. It ensures that there are enough pods running to maintain the availability of your application, even when nodes are drained or updated.
#### **Why PDB is important**:
* When you **drain a node** (for example, for maintenance or upgrades), the pods on that node are evicted. Without a PDB, Kubernetes may evict too many pods at once, leading to service downtime or reduced availability.
* PDB helps ensure that **at least a certain number of pods** (or a percentage of pods) will remain running during voluntary disruptions, preventing the application from going down.
#### **How it works**:
A **Pod Disruption Budget** can be defined to specify:
* The **minimum number of pods** that must remain available.
* The **maximum number of pods** that can be disrupted (evicted).
#### **Example of Pod Disruption Budget (PDB)**:
```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: my-app-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: my-app
```
* **`minAvailable`**: Ensures that **at least 2 pods** of the app remain available during disruption.
* **`maxUnavailable`**: Alternatively, you could set a `maxUnavailable` to specify the maximum number of pods that can be disrupted.
### **Key Terms**:
* **Voluntary Disruption**: This refers to planned disruptions, such as rolling updates, node maintenance, or manual pod deletion.
* **Involuntary Disruption**: This refers to unplanned disruptions, such as when the pod is evicted due to resource pressure.
---
### **Summary**:
* **Pod Eviction** is when Kubernetes removes pods from nodes to free up resources or due to maintenance.
* **Pod Disruption Budget (PDB)** defines how many pods can be disrupted at a time to ensure availability during voluntary disruptions (like upgrades or node drains).
---
Would you like to go deeper into **how PDBs interact with deployments** or **what happens during node draining** in more detail?


