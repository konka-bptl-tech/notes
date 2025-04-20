### ✅ **Scheduler & Pod Placement**

## ⚙️ Kubernetes Scheduler: Internal Algorithm

When a Pod is created, the **Kube Scheduler** decides which node it should run on using two main steps:

### 1️⃣ **Filtering Phase**
🔍 The scheduler filters out nodes that **don’t meet the pod’s requirements**, like:
- Node taints & tolerations
- NodeSelector / NodeAffinity
- Resource requests (CPU, memory)
- Pod topology constraints

👉 The result is a **list of nodes that are eligible** for scheduling.

---

### 2️⃣ **Scoring Phase**
🔢 Each node from the filtered list gets a **score** based on how well it fits the pod.

Scoring considers:
- 🧠 Available CPU & Memory (least requested resources get higher scores)
- 🔗 Pod affinity/anti-affinity
- ⚖️ Custom weights (if defined)
- 📦 Image locality

👉 The node with the **highest total score** is selected to place the pod.

---

Want to see this flow visually or with a real example YAML?

- **Kube-Scheduler** is responsible for deciding which **node** a **Pod** should be scheduled on.
- **Pod stays in a `Pending` state** if no suitable node is found.

---

### ✅ **NodeSelector**

- Basic way to constrain pods to be scheduled only on nodes with a specific label.
- If the label doesn’t match, the Pod will **remain in Pending**.
```yaml
nodeSelector:
  disktype: ssd
```

```bash
kubectl get nodes --show-labels
```
---
### 🔍 To get labels of a specific node:
```bash
kubectl get node <node-name> --show-labels
```
---
### 📦 Or to get a cleaner list using `kubectl describe`:
```bash
kubectl describe node <node-name> | grep -A 10 "Labels"
```

```markdown
🏷️ **Label the node**
```bash
kubectl label nodes <your-node-name> disktype=ssd
```

```markdown
❌ **Unlabel the node**
```bash
kubectl label nodes <your-node-name> disktype-
```
---

### ✅ **Affinity & Anti-Affinity**

More advanced way to control pod placement than `nodeSelector`.

#### **1. requiredDuringSchedulingIgnoredDuringExecution**
- **Hard constraint** (like `nodeSelector`)
- Scheduler places the pod **only** if the condition matches.
- **Ignored during execution** – if node labels change after scheduling, the pod is **not moved**.

#### **2. preferredDuringSchedulingIgnoredDuringExecution**
- **Soft constraint**
- Scheduler **tries** to match the condition but **not mandatory**.
- Pod can still be scheduled elsewhere if no node matches.
- Again, ignored after the pod is running.

---

### ✅ **Pod Affinity and Anti-Affinity**

- Allows **pod-to-pod placement rules** based on labels.
- **Pod Affinity**: Place this pod **near another pod** (e.g., same node).
- **Pod Anti-Affinity**: Avoid placing this pod **on the same node** as another.

**Example use case:**
- If a **backend** pod requires a **cache** pod on the same node for better performance → use **Pod Affinity**.
Absolutely! Here's a clean and simple example showing both **Pod Affinity** and **Anti-Affinity** with explanations and emojis 🧲🚫

---
Sure! Here's the **exact context** for how `topologyKey: "kubernetes.io/hostname"` works inside a Pod Affinity or Anti-Affinity rule 🔍

---

### ✅ **Pod Affinity with `topologyKey: "kubernetes.io/hostname"`**

```yaml
affinity:
  podAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
    - labelSelector:
        matchExpressions:
        - key: app
          operator: In
          values:
          - backend
      topologyKey: "kubernetes.io/hostname"
```

### 🔄 What does this mean?

➡️ The pod will **only be scheduled** on a **node** where there is **already a pod running** with `label: app=backend`.

🧠 Because `topologyKey = kubernetes.io/hostname`, the scheduler **matches pods that are on the same node (host)**.

---

### 🚫 **Pod Anti-Affinity with the same `topologyKey`**

```yaml
affinity:
  podAntiAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
    - labelSelector:
        matchExpressions:
        - key: app
          operator: In
          values:
          - web
      topologyKey: "kubernetes.io/hostname"
```

### ❌ What does this mean?

➡️ The pod will **never be scheduled** on a node that already has a pod with `label: app=web`.

---

### 🔧 Summary

| `topologyKey`                     | Meaning                        |
|----------------------------------|--------------------------------|
| `"kubernetes.io/hostname"`       | Apply rule **per node**        |
| `"topology.kubernetes.io/zone"`  | Apply rule **per AZ/zone**     |
| `"topology.kubernetes.io/region"`| Apply rule **per region**      |

---

Let me know if you'd like a full YAML manifest using these! 📝
---

### ✅ **Taints and Tolerations**

Used to **repel pods from nodes** unless they explicitly tolerate the taint.

#### **Taints** (Applied on Node):
- `NoSchedule` → New pods that don’t tolerate the taint **will not be scheduled**.
- `PreferNoSchedule` → Tries to avoid scheduling if possible, but **not guaranteed**.
- `NoExecute` → **Evicts existing pods** that don’t tolerate the taint, and **prevents new ones** too.

#### **Tolerations** (Applied on Pod):
- Allows the pod to be scheduled onto a node **with matching taint**.
- Example:
```yaml
tolerations:
- key: "key1"
  operator: "Equal"
  value: "value1"
  effect: "NoSchedule"
```

---

### ✅ Summary Table

| Feature                     | Type      | Effect                                             |
|----------------------------|-----------|----------------------------------------------------|
| `nodeSelector`             | Hard      | Must match node label                              |
| `requiredDuring...`        | Hard      | Must match during scheduling                       |
| `preferredDuring...`       | Soft      | Best-effort match during scheduling                |
| `Pod Affinity`             | Soft/Hard | Places pod near other pods                         |
| `Pod Anti-Affinity`        | Soft/Hard | Avoids placing pod near other pods                 |
| `Taint` - NoSchedule       | Hard      | Blocks pod unless tolerated                        |
| `Taint` - PreferNoSchedule | Soft      | Avoids node if possible                            |
| `Taint` - NoExecute        | Hard      | Evicts pods if not tolerated                       |
| `Toleration`               | Allow     | Enables pod to stay on or be placed on tainted node|

---

If you want, I can also provide YAML examples for each case to help visualize it better. Want me to?
---
### **Taint**  
A **taint** is applied to a node to **repel pods** from being scheduled onto it, unless those pods **tolerate** the taint. It lets a node **reject pods** that don't meet certain criteria.
---
### **Toleration**  
A **toleration** is applied to a pod to **allow it to be scheduled** on nodes with matching taints. It tells the scheduler, “This pod is okay with the taint on that node.”

---

Combining **Node Affinity**, **Taints**, and **Tolerations** together provides a more refined control over pod scheduling. This combination ensures that you can:

1. **Taints**: Block pod scheduling on nodes unless explicitly allowed (via tolerations).
2. **Tolerations**: Allow specific pods to be scheduled on nodes that have certain taints.
3. **Node Affinity**: Ensure that a pod is scheduled on nodes that meet certain criteria based on labels.

### Example: Perfect Scheduling with Affinity, Taints, and Tolerations

Let's walk through a scenario where we want to ensure that:

1. Pods with specific labels only get scheduled on nodes with a particular disk type (e.g., SSD).
2. Nodes with a taint (`disktype=ssd:NoSchedule`) should only allow pods that have the corresponding toleration.
3. The pods should respect **node affinity** (e.g., they should only be scheduled on nodes with the label `disktype=ssd`).

### 1. Apply Taint to Node (NoScheduling)
This taint prevents pods from being scheduled on nodes with the taint unless they tolerate it.

```bash
kubectl taint nodes <node-name> disktype=ssd:NoSchedule
```

This taints the node with `disktype=ssd` and prevents any pod from being scheduled unless it has a matching toleration.

### 2. Define Pod with Node Affinity, Toleration, and Affinity

Here’s an example of a pod specification using **Node Affinity** and **Tolerations** together:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  # Toleration: Allows the pod to be scheduled on tainted nodes
  tolerations:
  - key: "disktype"
    operator: "Equal"
    value: "ssd"
    effect: "NoSchedule"

  # Node Affinity: Ensures the pod is scheduled only on nodes with the 'disktype=ssd' label
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: "disktype"
            operator: "In"
            values:
            - "ssd"

  containers:
  - name: nginx
    image: nginx
```

### Breakdown:

1. **Tolerations**:
   - The `tolerations` section ensures that the pod can be scheduled on nodes that have the `disktype=ssd:NoSchedule` taint.
   - Without this, the pod would be blocked from scheduling on nodes with the taint.
   
2. **Node Affinity**:
   - The `nodeAffinity` section ensures that the pod can only be scheduled on nodes that have the label `disktype=ssd`.
   - The `requiredDuringSchedulingIgnoredDuringExecution` ensures that only nodes with the `disktype=ssd` label are eligible for scheduling.

### 3. Combine Taints and Affinity in Real-World Scenarios

#### Scenario 1: Scheduling Pods Based on Node Type (Disk Type)
- **Node Affinity** ensures pods are scheduled on nodes with the required disk type (SSD).
- **Taints** and **Tolerations** ensure that only pods with the proper toleration can be scheduled on nodes with the `ssd` taint.
  
#### Scenario 2: Sensitive Workloads on Specific Nodes
You could set up taints and tolerations to restrict certain workloads (like database pods) to nodes with extra CPU or memory resources. Affinity could then ensure the pod goes to the right set of nodes, and tolerations would allow only those pods that need to be scheduled despite taints.

### 4. Example Scenario with Both Taints and Affinity for Two Sets of Pods

Imagine you want to schedule:

- **Frontend pods** should go to nodes with **SSD** disks (`disktype=ssd`), and these nodes must not have high-memory taints.
- **Backend pods** should be scheduled only on nodes that **must** have high-memory resources.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: frontend-pod
spec:
  tolerations:
  - key: "disktype"
    operator: "Equal"
    value: "ssd"
    effect: "NoSchedule"
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: "disktype"
            operator: "In"
            values:
            - "ssd"
  containers:
  - name: nginx
    image: nginx
---
apiVersion: v1
kind: Pod
metadata:
  name: backend-pod
spec:
  tolerations:
  - key: "high-memory"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: "memory"
            operator: "In"
            values:
            - "high"
  containers:
  - name: mysql
    image: mysql
```

### Conclusion:
- **Node Affinity** ensures that pods are scheduled on the right nodes based on node labels.
- **Taints** prevent undesired scheduling on nodes, and **Tolerations** allow specific pods to override those taints.
- Combining them allows you to achieve sophisticated and fine-grained control over pod scheduling, making sure your workloads run on the most appropriate nodes with optimal resource allocation.
---

## 🗺️ **Pod Topology Spread Constraints**

**Pod Topology Constraints** let you control **how pods are distributed** across your cluster’s topology — like nodes, zones, or regions — to achieve **high availability** and **fault tolerance**.

### 🎯 Purpose:
Avoid putting all replicas of a deployment on the same node or zone — this way, if one node or zone fails, your app still runs elsewhere.

---

### 🔧 Key Fields in `topologySpreadConstraints`:

```yaml
topologySpreadConstraints:
- maxSkew: 1
  topologyKey: topology.kubernetes.io/zone
  whenUnsatisfiable: ScheduleAnyway
  labelSelector:
    matchLabels:
      app: myapp
```

#### 🧩 Explanation:

- `maxSkew`:  
  The max difference in number of pods across topologies (e.g., one zone has 3 pods, another has 2 = skew of 1 ✅).

- `topologyKey`:  
  The label key on nodes to define the topology.  
  Examples:
  - `kubernetes.io/hostname` → per node  
  - `topology.kubernetes.io/zone` → per zone

- `whenUnsatisfiable`:  
  What to do if the constraint can't be met:
  - `ScheduleAnyway` → still schedule, just warn
  - `DoNotSchedule` → strictly enforce

- `labelSelector`:  
  Which pods this constraint applies to (usually by label).

---

### 📦 Example in a Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spread-demo
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      topologySpreadConstraints:
      - maxSkew: 1
        topologyKey: kubernetes.io/hostname
        whenUnsatisfiable: ScheduleAnyway
        labelSelector:
          matchLabels:
            app: myapp
      containers:
      - name: nginx
        image: nginx
```
---
