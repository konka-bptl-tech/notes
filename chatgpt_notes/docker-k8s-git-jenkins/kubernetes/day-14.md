### ✅ **Scheduler & Pod Placement**

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
