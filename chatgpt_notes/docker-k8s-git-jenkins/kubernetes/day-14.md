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

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      nodeSelector:
        disktype: ssd
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
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

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 25
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
      affinity:
        nodeAffinity:
          # requiredDuringSchedulingIgnoredDuringExecution:
          #   nodeSelectorTerms:
          #   - matchExpressions:
          #     - key: disktype
          #       operator: In
          #       values:
          #       - ssd
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 1
            preference:
              matchExpressions:
              - key: disktype
                operator: In
                values:
                - ssd

---

### ✅ **Pod Affinity and Anti-Affinity**

- Allows **pod-to-pod placement rules** based on labels.
- **Pod Affinity**: Place this pod **near another pod** (e.g., same node).
- **Pod Anti-Affinity**: Avoid placing this pod **on the same node** as another.

**Example use case:**
- If a **backend** pod requires a **cache** pod on the same node for better performance → use **Pod Affinity**.
Absolutely! Here's a clean and simple example showing both **Pod Affinity** and **Anti-Affinity** with explanations and emojis 🧲🚫

---

## 🧲 **Pod Affinity Example**
> *Schedule a pod **close to other pods** with a specific label.*

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-pod
  labels:
    app: web
spec:
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
  containers:
  - name: busybox
    image: busybox
    command: ["sleep", "3600"]
```

### 🔍 What's happening here?

- Pod wants to be **scheduled on the same node** as pods with `app=backend`.
- `topologyKey: kubernetes.io/hostname` means it matches at the **node level**.

---

## 🚫 **Pod Anti-Affinity Example**
> *Avoid scheduling a pod on the same node as certain other pods.*

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: backend-pod
  labels:
    app: backend
spec:
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
  containers:
  - name: busybox
    image: busybox
    command: ["sleep", "3600"]
```

### 🧠 Explanation:

- This pod **must avoid** nodes that already have a pod labeled `app=web`.
- Used to **spread workloads** or avoid conflict between apps.

---

Want me to combine both in one YAML or explain `preferredDuringScheduling` for these too?

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
