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