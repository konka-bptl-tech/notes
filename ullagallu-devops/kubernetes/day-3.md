# 🚀 Kubernetes Deployment
A **Deployment** is a high-level controller in Kubernetes used to manage application **rollouts**, **rollbacks**, and scaling in a declarative or imperative way.
---
### 🎯 Key Features of Deployment:
* Manages the **entire lifecycle** of an application.
* **Rollouts** new versions of Pods and **rolls back** to previous versions if needed.
* Automatically manages a **ReplicaSet**, which in turn maintains the actual Pods.
* Ensures **zero-downtime deployments** via rolling updates.
---
### 🧱 Deployment Architecture:
```
Deployment → ReplicaSet → Pods
```
* The **Deployment** creates and updates **ReplicaSets**.
* The **ReplicaSet** ensures the right number of **Pods** are running.
---
### 📦 Benefits:
* Easy to manage **application versions**.
* Supports:
  * ✅ Rolling Updates
  * ✅ Rollbacks
  * ✅ Scaling
  * ✅ Declarative and Imperative management
---
### 🛠 Imperative Command to Create a Deployment:
```bash
kubectl create deployment <deployment-name> --image=<container-image>
```
**Example:**
```bash
kubectl create deployment nginx-deploy --image=nginx
```
To scale:
```bash
kubectl scale deployment nginx-deploy --replicas=5
```
To edit:
```bash
kubectl edit deployment nginx-deploy
```
To undo (rollback):
```bash
kubectl rollout undo deployment nginx-deploy
```
---
# ⚙️ Kubernetes Resource Management
**Resource Management** in Kubernetes helps ensure that each container gets the **right amount of CPU and memory** (RAM), and prevents any one container from over-consuming system resources and affecting others.
---
### 🎯 Why Resource Management Matters
* Prevents **resource starvation** for other Pods
* Helps achieve **fair usage** on shared nodes
* Enables **auto-scaling** and efficient scheduling
* Improves **cluster stability and predictability**
---
## 📐 Key Concepts
### 1. **Requests**
* Minimum amount of **CPU** or **memory** a container is guaranteed.
* Used by the scheduler to decide **which node** to place the pod on.
```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
```
### 2. **Limits**
* Maximum amount of **CPU** or **memory** a container is allowed to use.
* If the container exceeds this, it may be **throttled** (CPU) or **terminated** (memory).
```yaml
resources:
  limits:
    memory: "128Mi"
    cpu: "500m"
```
---
## 📦 Full Example (in Pod/Deployment YAML)
```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
  limits:
    memory: "128Mi"
    cpu: "500m"
```
* `250m` means **250 millicores** (0.25 vCPU)
* `"64Mi"` means **64 Mebibytes** of memory
---
### 🧪 To See Pod Resource Usage:
```bash
kubectl top pod
```
---
# 📊 What is **Metrics Server** in Kubernetes?
The **Metrics Server** is a lightweight, cluster-wide aggregator of **resource usage data** such as **CPU and memory**. It collects metrics from the **Kubelets** running on each node and makes them available through the Kubernetes API.
---
### 🎯 Purpose of Metrics Server:
* Enables resource monitoring of Pods and Nodes.
* Required for **Horizontal Pod Autoscaler (HPA)** and **Vertical Pod Autoscaler (VPA)**.
* Supports the `kubectl top` command to view real-time CPU and memory usage.
---
### 🔧 What It Collects:
* **CPU usage** (millicores)
* **Memory usage** (bytes)
* From **Kubelet summary API** on each node.
---
### 📌 Common Commands:
* View Pod usage:
  ```bash
  kubectl top pod
  ```
* View Node usage:
  ```bash
  kubectl top node
  ```
---
### 🚫 Not for Long-Term Monitoring:
* **Does not store historical data.**
* For long-term, use Prometheus, Grafana, etc.
---
### 🛠 Install Metrics Server (if not installed):
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
---
# 📊 **Horizontal Pod Autoscaler (HPA)** in Kubernetes
The **Horizontal Pod Autoscaler (HPA)** is a Kubernetes resource that automatically scales the number of Pods in a Deployment, ReplicaSet, or StatefulSet based on observed **CPU utilization** or other **custom metrics** (e.g., memory, request rate).
---
### 🎯 **Purpose of HPA**:
* **Automatically adjusts** the number of Pods in response to load (CPU, memory, or custom metrics).
* Helps to **optimize resource utilization**.
* Ensures your application can **handle varying workloads** without manual intervention.
---
### 🔧 **How HPA Works**:
1. The **Metrics Server** gathers **resource usage data** from nodes and Pods.
2. The **HPA Controller** adjusts the number of replicas in the target object (like Deployment) based on the metric (e.g., CPU utilization).
3. It can scale Pods **up** (add more) or **down** (remove) to meet the desired metric thresholds.
---
### 🧑‍💻 **HPA Basic Syntax:**
To create a Horizontal Pod Autoscaler:
```bash
kubectl autoscale deployment <deployment-name> --cpu-percent=<target-CPU-percent> --min=<min-replicas> --max=<max-replicas>
```
**Example:**
```bash
kubectl autoscale deployment nginx-deploy --cpu-percent=50 --min=2 --max=10
```
* This command creates an HPA that targets **50% CPU utilization**.
* It ensures **at least 2 replicas** and scales up to a **maximum of 10 replicas**.
---
### 📐 **HPA YAML Example:**
Here’s how you can define an HPA in YAML format for more flexibility:
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deploy
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```
* **scaleTargetRef**: Specifies which Deployment to scale.
* **metrics**: Defines the metric (CPU in this case) to monitor and the target utilization (50% CPU).
---
### 🔍 **View HPA Status**:
To check the status of the HPA:
```bash
kubectl get hpa
```
---
### 📚 **Custom Metrics for HPA**:
In addition to CPU and memory, you can also use **custom metrics** for scaling, such as:
* **Request rate**
* **Queue length**
* **Custom application metrics**
---
### 🛠 **Prerequisites for HPA**:
1. **Metrics Server** must be installed in your cluster to provide CPU and memory metrics.
2. Ensure that your **pods** are configured with **resource requests and limits**.
---
### 🧑‍💻 **Job Controller** & **CronJob Controller** in Kubernetes
Kubernetes provides two controllers for managing **batch jobs**: the **Job Controller** and the **CronJob Controller**. They help you run tasks that are intended to run to completion (Job) or on a scheduled basis (CronJob).
---
## 1. **Job Controller**
The **Job** controller ensures that a specified number of **Pods** run to completion successfully. A **Job** is typically used for tasks like batch processing, backups, or one-time tasks.
### 🎯 **Job Purpose**:
* **Run a task** to completion (e.g., batch processing, database migration).
* Ensures **successful execution** of a task and automatically retries on failure.
### ✅ **Job Features**:
* **Completions**: Run a specified number of Pods to completion (default: 1).
* **Parallelism**: Control the number of Pods running concurrently.
* **Retries**: Automatically retry failed Pods based on the specified limit.
---
### 🛠 **Create a Job**:
Here’s a basic Job definition in YAML:
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job
spec:
  completions: 1        # Number of successful completions required
  parallelism: 1        # Number of Pods to run concurrently
  template:
    spec:
      containers:
      - name: busybox
        image: busybox
        command: ["echo", "Hello, Kubernetes!"]
      restartPolicy: Never
```
#### 📌 Key Fields:
* `completions`: The number of successful Pods required for the Job to be considered successful.
* `parallelism`: The maximum number of Pods that should run simultaneously.
* `restartPolicy: Never`: Ensures Pods do not restart once they are completed (for Jobs).
### 🧑‍💻 **Command to create the Job**:
```bash
kubectl apply -f job.yaml
```
---
### 🔍 **View Job Status**:
You can monitor the Job and check if it's completed successfully:
```bash
kubectl get jobs
```
To view detailed information about the Job:
```bash
kubectl describe job my-job
```
---
## 2. **CronJob Controller**
A **CronJob** allows you to run Jobs on a scheduled basis, similar to cron jobs in Linux. This is useful for tasks that need to run periodically, such as backups, report generation, or maintenance tasks.
### 🎯 **CronJob Purpose**:
* **Schedule Jobs** to run periodically at specified times.
* Useful for periodic tasks such as database cleanup, scheduled backups, etc.
### ✅ **CronJob Features**:
* **Scheduled Execution**: Define **when** to run the Job using Cron syntax.
* **Job Creation**: Each execution of a CronJob creates a new Job resource.
---
### 🛠 **Create a CronJob**:
Here’s a basic CronJob definition in YAML:
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-cronjob
spec:
  schedule: "0 0 * * *"    # Cron expression (every day at midnight)
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: busybox
            image: busybox
            command: ["echo", "Running scheduled job"]
          restartPolicy: OnFailure
```
#### 📌 Key Fields:
* `schedule`: Cron expression specifying when the Job should run (e.g., `"0 0 * * *"` runs daily at midnight).
* `jobTemplate`: Defines the Job specification that should be run when the CronJob triggers.
### 🧑‍💻 **Command to create the CronJob**:
```bash
kubectl apply -f cronjob.yaml
```
---
### 🔍 **View CronJob Status**:
To check the CronJob status:
```bash
kubectl get cronjobs
```
To check the Jobs that have been triggered by the CronJob:
```bash
kubectl get jobs --watch
```
---
## 💡 **Use Cases for Job and CronJob**
* **Job**: Run once-off tasks like batch processing, database migrations, or one-time data exports.
* **CronJob**: Automate regular tasks like **backups**, **data cleanup**, **report generation**, or any task that needs to be run on a schedule.
---