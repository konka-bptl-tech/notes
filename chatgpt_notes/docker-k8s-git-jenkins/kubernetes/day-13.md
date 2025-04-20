## 🔐 **Linux Authentication & Authorization**

### 👥 Users
- **System users**:  
  - No shell (`/sbin/nologin` or `/bin/false`)  
  - Identity only (used by system services like `nginx`, `mysql`)
- **siva** (Admin):  
  - Has shell  
  - Requires authentication (password)  
  - Admin access via `sudo`
- **ram** (Normal user):  
  - Has shell  
  - Limited command execution

### ⚙️ `sudoers` File
Edit with:
```bash
sudo visudo
```

#### 🔑 Example:
```bash
# Admin user (siva)
siva ALL=(ALL) ALL

# Normal user (ram) – can only run ls and cat
ram ALL=(ALL) /usr/bin/ls, /usr/bin/cat
```
---

## ☁️ **AWS Authentication & Authorization**

### 👤 Identity:
- **Users & Groups**  
  - Use long-term credentials (Access Key + Secret)
- **Roles**  
  - No credentials needed  
  - Used by AWS services, IAM users, or federated identities  
  - Assume roles via **trust policy**

### 📜 Permissions:
- **IAM Policies**  
  - JSON document specifying actions/resources  
  - Can be attached to users, groups, or roles

### 🌐 OIDC Provider (OpenID Connect)
- Enables **federated identity** (external authentication provider)  
- Used to **link Kubernetes ServiceAccounts to AWS IAM Roles**
- Essential for **fine-grained access control** in EKS using `IRSA` (IAM Roles for Service Accounts)

#### ✅ Why Use OIDC in EKS?
- Allows **pods** to access AWS services **securely without long-term secrets**
- Example: a pod can access S3 using a role mapped via OIDC without storing AWS keys

---

## ☸️ **Kubernetes Authentication & Authorization**

### 👤 Identity:
- **ServiceAccount**  
  - Provides pod identity inside the cluster  
  - Often linked with AWS IAM Role via OIDC in EKS

### 🔐 Authorization:
- **RBAC (Role-Based Access Control)**  
  - Uses `Role` and `ClusterRole` to define permissions  
  - Bound to users or service accounts using `RoleBinding` or `ClusterRoleBinding`
---
# Netowrk Policies
A **Network Policy** in Kubernetes is used to control the traffic flow between pods, allowing you to specify which pods can communicate with each other. By default, all pods can communicate with each other, but when a network policy is defined, it enforces restrictions on the allowed communication. It is a powerful tool for securing your applications by controlling which pods can talk to each other based on labels, namespaces, and IPs.

- NetworkPolicies work at the IP and port level between Pods, not Services.

### Key Concepts of Network Policy:
1. **Pod Selector**: Selects the pods to which the policy applies.
2. **Ingress**: Specifies which sources (other pods or IPs) can access the pods defined by the policy.
3. **Egress**: Specifies which destinations (other pods or IPs) the pods defined by the policy can access.
4. **Policy Types**: Can be either `Ingress`, `Egress`, or both.

### Example Use Cases for Network Policies:
- Restrict traffic to only certain services within a namespace.
- Prevent unauthorized access to certain pods.
- Allow only certain pods to communicate with each other.

### Example of a Basic Network Policy

Let's say we have two pods:
1. **frontend** pod that should be accessible only from **backend** pods.
2. **backend** pod that should not be accessible from any other pods.

### 1. **Allow Only Backend Pods to Access Frontend Pods**

This network policy ensures that only the **backend** pods (with the label `role=backend`) can access the **frontend** pods (with the label `role=frontend`).

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-backend-to-frontend
spec:
  podSelector:
    matchLabels:
      role: frontend  # Apply this policy to pods with label 'role=frontend'
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: backend  # Allow only pods with label 'role=backend' to access
  policyTypes:
  - Ingress  # Only apply ingress rules
```

- **podSelector**: Targets pods labeled with `role=frontend`.
- **ingress**: Allows traffic only from pods labeled with `role=backend`.

### 2. **Deny All Ingress Traffic to Backend Pods**

This policy ensures that no pod can access the **backend** pods unless otherwise specified.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all-backend
spec:
  podSelector:
    matchLabels:
      role: backend  # Apply this policy to pods with label 'role=backend'
  ingress: []  # Empty ingress array denies all incoming traffic
  policyTypes:
  - Ingress  # Only apply ingress rules
```

- **podSelector**: Targets pods labeled with `role=backend`.
- **ingress: []**: An empty ingress array denies all incoming traffic to the backend pods.

### 3. **Allow Egress Traffic to Specific IP Range**

This policy allows **frontend** pods to access an external IP range (e.g., external databases or APIs).

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-external
spec:
  podSelector:
    matchLabels:
      role: frontend  # Apply this policy to pods with label 'role=frontend'
  egress:
  - to:
    - ipBlock:
        cidr: 192.168.1.0/24  # Allow traffic to this specific IP range
  policyTypes:
  - Egress  # Only apply egress rules
```

- **podSelector**: Targets pods labeled with `role=frontend`.
- **egress**: Allows traffic to the IP range `192.168.1.0/24`.

### 4. **Allow Traffic from Specific Namespace**

This policy allows pods in the **frontend** namespace to accept traffic from pods in the **backend** namespace.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-from-backend-namespace
spec:
  podSelector:
    matchLabels:
      role: frontend  # Apply this policy to pods with label 'role=frontend'
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: backend  # Allow traffic from the 'backend' namespace
  policyTypes:
  - Ingress  # Only apply ingress rules
```

- **namespaceSelector**: Allows traffic from pods in the `backend` namespace.

### 5. **Allow All Traffic Between Pods in the Same Namespace**

This policy allows all pods within the same namespace to communicate freely with each other.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-all-same-namespace
spec:
  podSelector: {}  # Apply to all pods in the namespace
  ingress:
  - from:
    - podSelector: {}  # Allow traffic from all pods within the same namespace
  policyTypes:
  - Ingress  # Only apply ingress rules
```

- **podSelector: {}**: Targets all pods within the same namespace.
- **ingress**: Allows all pods in the namespace to access each other.

### Important Considerations:
- **Default Behavior**: If no network policies are applied, all pods can communicate with each other. Once a network policy is applied, it enforces the rules, and by default, denies all other traffic unless explicitly allowed.
- **CNI (Container Network Interface)**: Network policies require a CNI plugin that supports them, such as Calico, Cilium, or Weave.
- **Order of Policies**: Network policies are additive; multiple policies can be applied to pods, and all rules are merged.

### Conclusion:
Kubernetes Network Policies provide a way to secure communication between pods and services by restricting traffic based on labels, namespaces, and IPs. By using **ingress** and **egress** rules, along with **selectors** for pods, namespaces, and IP blocks, you can achieve very granular control over how your services interact within the cluster.