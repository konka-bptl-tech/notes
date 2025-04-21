## 🌐 Ingress in Kubernetes
---
### 📌 What is **Ingress**?
- **Ingress** is a **Kubernetes API object** that manages **external access** to services in a cluster.
- It acts as a **Layer 7 (Application Layer) Load Balancer**, routing external HTTP/HTTPS traffic to internal services **based on rules** (host/path).
---
### ⚙️ Ingress vs Traditional Load Balancers
#### 🔸 Layer 4 Load Balancer (Transport Layer)
- Just forwards traffic (e.g., TCP) to backend pods.
- **No routing intelligence** — no path or host-based decisions.
- Doesn’t inspect or modify the request.
- Works like a **traffic distributor**.

#### 🔹 Layer 7 Load Balancer (Application Layer)
- Smart routing based on:
  - **Hostnames** (e.g., `mail.google.com`)
  - **Paths** (e.g., `/api`, `/admin`)
- Supports:
  - URL rewrites
  - SSL termination
  - Authentication
  - Header-based routing

---

### 📥 Examples of Host-Based Routing
| Hostname | Service |
|----------|---------|
| `www.google.com` | Google Search |
| `drive.google.com` | Google Drive |
| `mail.google.com` | Gmail |
| `calendar.google.com` | Google Calendar |
| `meet.google.com` | Google Meet |
| `photos.google.com` | Google Photos |
| `maps.google.com` | Google Maps |

---

### 🛡️ Why not just use NodePort or LoadBalancer?

| Method | Issues |
|--------|--------|
| **NodePort** | Exposes ports on every node – **not secure**, hard to manage. |
| **LoadBalancer** | Creates a **dedicated L4 load balancer per service** – very **expensive** and **not scalable**. |

✅ **Ingress** provides a **centralized**, **secure**, and **cost-effective** solution for routing multiple apps.

---

### 🚦 Ingress Request Flow
**Browser** → **DNS Resolution** → **TCP 3-Way Handshake** →  
**Load Balancer (L7)** → **Ingress Controller** → **K8s Service** → **Pod**

---

### 📄 Summary

| Component | Description |
|----------|-------------|
| **Ingress Resource** | Defines routing rules (host/path) |
| **Ingress Controller** | Enforces rules and routes traffic accordingly |
| **Example** | `api.example.com` → routes to `backend-service` |


## 🌐 **External DNS Controller**

### ✅ What It Is:
A Kubernetes controller that **automatically manages DNS records** (like A/CNAME records) in external DNS providers based on Ingress or Service annotations.

### 🚀 Why We Use It:
- Automates DNS updates when services are deployed.
- No need to manually update DNS records.
- Super helpful in dynamic, auto-scaling environments.

### 📦 Supported DNS Providers:
- Route53 (AWS)
- Google Cloud DNS
- Azure DNS
- Cloudflare
- GoDaddy
- Many more...

### 🛠️ How It Works:
- You deploy ExternalDNS in your cluster.
- It watches Ingress or Service resources with proper annotations.
- It creates/updates/deletes DNS records in the configured DNS provider.

---

## 🧩 Quick Example Use Case

| Tool | Example |
|------|---------|
| **External Secrets Operator** | Pull DB password from AWS Secrets Manager into K8s secret |
| **External DNS Controller** | Create a DNS record `api.example.com` → points to a LoadBalancer IP |
---
### 🔄 What "Reconciliation" Means

In simple terms:

> **Reconciliation** is the process where a controller continuously checks the **actual state** of the system and compares it to the **desired state**, then **makes changes** if necessary to match them.

---

### 🧠 Example with Ingress Controller:

When you apply this Ingress YAML:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    ...
```

Here’s what happens:

1. You **declare the desired state** (an Ingress that routes traffic a certain way).
2. The Ingress controller (e.g., AWS ALB Controller, NGINX) **sees the new Ingress resource**.
3. It **reconciles**:
   - Looks at what's already deployed (e.g., is there an ALB? Are rules correct?).
   - Compares with what the Ingress spec wants.
   - Creates or updates the load balancer and routes if needed.
4. Logs:
   ```plaintext
   SuccessfullyReconciled
   ```

It means:

✅ The controller verified everything
✅ It applied any needed changes
✅ The "real world" now matches your spec

---

### 📌 TL;DR

> **Reconciliation = syncing the actual world to your desired config.**

Kubernetes controllers (Deployment, Ingress, HPA, etc.) constantly reconcile to **self-heal** and **auto-fix drift**. It's a key part of how Kubernetes stays declarative and reliable.

<pre>
You apply an Ingress 📝
   |
   v
+-----------------------+
| Desired State (YAML) |
|  - Route /api        |
|  - Service: backend  |
+-----------------------+
           |
           v
Ingress Controller starts watching 👀
           |
           v
+----------------------------+
| Actual State (Cluster)    |
|  - No ALB yet             |
+----------------------------+
           |
           v
Controller says: "Mismatch! Let me fix it..." 🔧
           |
           v
🚀 Creating ALB...
🔁 Configuring Target Groups...
🔗 Setting up routing rules...
           |
           v
✅ Done! Real state matches desired state
           |
           v
📣  `SuccessfullyReconciled`
</pre>
---
# Exercise

```bash
#!/bin/bash
# Create ALB Ingress Controller it will create ALB,TG,Listners
CLUSTER_NAME="karpenter"
INGRESS_ARN="arn:aws:iam::522814728660:policy/AWSLoadBalancerControllerIAMPolicy"
echo "Creating Service Account"
eksctl create iamserviceaccount \
    --cluster=$CLUSTER_NAME \
    --namespace=kube-system \
    --name=aws-load-balancer-controller \
    --attach-policy-arn=$INGRESS_ARN \
    --approve

echo "Add alb ingress controller helm chart"
helm repo add eks https://aws.github.io/eks-charts

echo "Install ALB ingress controller helm chart"
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
    --set clusterName=$CLUSTER_NAME -n kube-system \
    --set serviceAccount.create=false \
    --set serviceAccount.name=aws-load-balancer-controller
```

```bash
# Create External DNS Controller for auto creating Records
#!/bin/bash

EKS_CLUSTER_NAME="karpenter"
POLICY_ARN="arn:aws:iam::522814728660:policy/ExternalDNSPolicy"

echo "Creating Service Account"
eksctl create iamserviceaccount \
      --cluster=$EKS_CLUSTER_NAME \
      --namespace=kube-system \
      --name=external-dns \
      --attach-policy-arn=$POLICY_ARN \
      --approve

echo "Add external DNS helm chart"
helm repo add external-dns https://kubernetes-sigs.github.io/external-dns/

echo "Install DNS COntroller"
helm install external-dns external-dns/external-dns \
    --set serviceAccount.create=false \
    --set serviceAccount.name=external-dns \
    -n kube-system
```
- create 2 namespaces project-1 and project2

```bash
for ns in project1 project-2; do kubectl create namespace $ns;done
```

Here's a simple `README.md` snippet with your link:

```markdown
# Ingress Practice

This repository contains examples and resources for practicing Kubernetes Ingress configurations.
[View Day-12 Ingress Examples](https://vscode.dev/github/ullagallu123/k8s-practicle-guide/blob/main/day-12/)
```