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
# watch mode
kubectl get pods -w
# View pod logs
kubectl logs <pod-name>
# Access pod terminal
kubectl exec -it <pod-name> -- bash
# or if bash is not available
kubectl exec -it <pod-name> -- sh
# Access container terminal in a pod if we have multiple containers
kubectl exec -it <pod-name> -c <container-name> -- sh or bash
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

# Labels and selectos commands
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
# ReplicationController Examples
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
spec:
  replicas: 2
  selector:
    app: myapp              # <- Flat map, not matchLabels
  template:
    metadata:
      labels:
        app: myapp          # <- Must match the selector exactly
    spec:
      containers:
      - name: myapp-container
        image: nginx
```

```yaml
# Replication Controller does not support selector based operators like below it gives error
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp            # ❌ This is NOT allowed in ReplicationController
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp-container
        image: nginx
```
# ReplicaSet
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-rs
spec:
  replicas: 5
  selector:
    matchLabels:
      env: dev
  template:
    metadata:
      labels:
        env: dev
    spec:
      containers:
      - name: myapp-container
        image: nginx

```
# Exercise-1
- Create a pod
- Create a replicaSet with labels env=dev
- update pod with label env=dev
- now observe the behaviour
- remove the label of pod now observe the behaviour pod

#### My observations
- if add label to pod env: dev RS controller brings the nginx-pod under RS
- if we remove label of the RS again creates another pod
- RS is responsible maintain desired no of pods

# Exercise-2
- create a pod with 2 containers nginx and httpd
- now curl to httpd from nginx and vice-versa
- also create a pods with can sharable storage between containers


