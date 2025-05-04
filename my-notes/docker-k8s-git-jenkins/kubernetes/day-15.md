# 13 common errors
- Before trouble shooting first look into status of the pod
- decribe the pod kubectl describe pod <pod-name>
- check the logs of pod kubectl logs <pod-name>

- Pod not scheduled
- Pod Scheduled Container Creation error
- Pod Scheduled Container run time error

### ✅ **Scheduling Errors**
These errors occur when the Kubernetes scheduler is unable to place a pod on any node.

1. **Insufficient Resources (requests)**
   - Not enough CPU or memory available on any node to meet the pod’s resource `requests`.

2. **Node Affinity**
   - Pod specifies node affinity rules that no current node satisfies.

3. **Unbound Persistent Volume (PVC issues)**
   - PVC not bound to a PV.
   - No matching StorageClass.
   - PVC defined but not mounted in the pod.
   - Delay in provisioning (e.g., dynamic provisioning issues).

4. **Node Taint**
   - Nodes have taints that the pod doesn't tolerate.

5. **Resource Quota Exceeded**
   - Namespace-level quotas restrict the number or size of resources, and the pod exceeds the limit.

---

### ⚙️ **Runtime Errors**
These happen **after** a pod has been scheduled onto a node.

6. **Unavailable ConfigMap**
   - ConfigMap referenced by the pod doesn’t exist or is in the wrong namespace.

7. **Unavailable Secret**
   - Secret used by the pod is missing or misconfigured.

8. **ImagePullBackOff**
   - Kubernetes is retrying to pull the image but failing.
     - Image doesn’t exist or wrong tag used.
     - Image registry authentication issues.
     - Network/DNS issues.

9. **CrashLoopBackOff – Out of Memory (OOMKilled)**
   - Container exceeds its memory limit and is killed by the kernel.

10. **CrashLoopBackOff – Health Check Failure**
   - Liveness/readiness probes fail repeatedly, causing restarts.

11. **CrashLoopBackOff – Init Container Fails**
   - One or more init containers fail, blocking pod startup.

12. **CrashLoopBackOff – Runtime Failure**
   - Main application process crashes or exits unexpectedly.

13. **Runtime Error – Service Issues**
   - Service misconfigured (wrong selector).
   - Pod is running but not reachable due to:
     - Network policy blocking.
     - DNS issues.
     - Not passing readiness probes.

---
That's a solid list 👇 Here's a bit more structure and detail to help you (or anyone reviewing this) troubleshoot **Pod Scheduling** and **Pod Execution** issues in Kubernetes more effectively:

---

### 🚫 **Pod Scheduling Errors**

These errors prevent the pod from being scheduled onto a node.

#### 1. **Taints and Tolerations**
- **Symptom**: `pod has unbound PersistentVolumeClaims` or `No nodes are available that match all of the following predicates...`
- **Fix**: Add tolerations in pod spec to match node taints.

#### 2. **Node Affinity / Anti-Affinity**
- **Symptom**: Pod remains in `Pending` state.
- **Fix**: Check `nodeAffinity` settings in your deployment and make sure there are nodes matching the rules.

#### 3. **Resource Quotas & Limits**
- **Symptom**: Pod stuck in `Pending`, or admission errors.
- **Fix**: Check namespace-level resource quotas (`kubectl describe quota`) and ensure pod requests/limits are within them.

#### 4. **Unschedulable Node Conditions**
- **Examples**:
  - Not enough CPU or memory
  - Node selectors not matching
- **Fix**: Use `kubectl describe pod <pod-name>` and look at `Events`.

---

### 💥 **Pod Execution Errors**

These happen **after** scheduling — pod lands on a node but fails to run properly.

#### 1. **ImagePullBackOff / ErrImagePull**
- **Common Causes**:
  - ❌ Tag mismatch
  - ❌ Image doesn't exist in registry
  - ❌ Private registry → lack of pull secret
- **Fix**:
  - Check container image URL and tag.
  - Check if ImagePullSecret is correctly defined and accessible.

#### 2. **CrashLoopBackOff**
- **Common Causes**:
  - App crashed repeatedly
  - Misconfiguration in env vars, command, args
  - Not enough resources (OOMKilled)
- **Fix**:
  - Inspect logs: `kubectl logs <pod-name> -c <container>`
  - Check exit code, increase memory/CPU limits if needed.

#### 3. **ConfigMap / Secret Errors**
- **Symptoms**:
  - Pod fails to start
  - Mount failures or env vars not resolved
- **Fix**:
  - Check if the ConfigMap/Secret exists.
  - Ensure it's mounted or injected properly.

#### 4. **Labels & Selectors Mismatch**
- **Symptoms**:
  - Service doesn’t route traffic
  - HPA doesn’t scale pods
- **Fix**: Ensure the labels on pods match the selectors defined in Service/Deployment/HPA.

#### 5. **Backend Pods Not Running**
- **Symptoms**:
  - Frontend pods show `Connection refused` or timeouts.
- **Fix**:
  - Check if backend pods are actually `Running` and `Ready`.
  - Verify readiness probes, service configurations, and DNS.
---