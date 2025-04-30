- Dairyam
- Mundu Choopu
- TeliviThetalu
- Samardhata
# Application Stack
- Frontend: ReactJS  
- Backend: Node.js  
- Caching: Redis  
- Database: RDS (Amazon Relational Database Service)

# VM based Architecture
www.vm.konkas.tech (DNS)
    ↓
CloudFront (CDN, SSL, WAF, Caching)
    ↓
Internal ALB (Private ALB)
    ↓
Frontend (ReactJS on VM, ASG)
    ↓
Internal ALB (Backend)
    ↓
Backend (NodeJS on VM, ASG)
    ↓
Elasticache (Redis)
    ↓
RDS (Database)

CI/CD: Jenkins Pipelines
Packer: AMI Build
Terraform: Infra Automation
Prometheus&Grafana: Metrics
Elastic Stack: Logs
# Serverless-Hosted Architecture
www.serverless.konkas.tech (DNS - Route53 or similar)
    ↓
CloudFront (CDN + SSL + WAF + Caching)
    ↓
Frontend (ReactJS static files hosted on S3, CORS enabled)
    ↓
Internal ALB (Private)
    ↓
Backend (NodeJS app running on ECS - Fargate or EC2 launch type)
    ↓
Elasticache (Redis for caching)
    ↓
RDS (Database)
# ECS Hosted
www.ecs.konkas.tech (DNS)
    ↓
CloudFront (SSL Termination + CDN + WAF)
    ↓
Internal ALB (Private ALB)
    ↓
ECS Service (Frontend + Backend containers)
        - Frontend container (ReactJS)
        - Backend container (NodeJS)
    ↓
Elasticache (Redis)
    ↓
RDS (Database)
# EKS Hosted
www.eks.konkas.tech (DNS)
    ↓
CloudFront (SSL Termination + CDN + WAF)
    ↓
ALB (Ingress Controller for EKS)
    ↓
EKS Cluster (Kubernetes)
    ↓
Frontend + Backend (Pod containers in EKS)
    ↓
Elasticache (Redis for caching)
    ↓
RDS (Relational Database)
---

### **Project 1: VM-Based Architecture**

#### **Architecture Overview:**
- **Frontend:** ReactJS running on VM (Autoscaling Group).
- **Backend:** NodeJS running on VM (Autoscaling Group).
- **Caching:** Elasticache (Redis).
- **Database:** RDS.

#### **Flow:**
```
www.vm.konkas.tech (DNS)
    ↓
CloudFront (CDN, SSL, WAF, Caching)
    ↓
Internal ALB (Private ALB)
    ↓
Frontend (ReactJS on VM, ASG)
    ↓
Internal ALB (Backend)
    ↓
Backend (NodeJS on VM, ASG)
    ↓
Elasticache (Redis)
    ↓
RDS (Database)
```

#### **Key Features:**
- **Managed Autoscaling:** VMs in an ASG allow autoscaling based on demand.
- **Private ALB:** Ensures backend communication stays internal.
- **CloudFront:** Provides CDN, caching, SSL termination, and security (WAF).

#### **Advantages:**
- Good for applications requiring custom server configurations.
- Can integrate with legacy systems or specialized setups.

#### **Challenges:**
- **Higher maintenance overhead** compared to serverless or containerized solutions.
- Scaling might be slower than containerized services.

---

### **Project 2: Serverless-Hosted Architecture**

#### **Architecture Overview:**
- **Frontend:** ReactJS hosted on S3 with CORS enabled.
- **Backend:** NodeJS app running on ECS (Fargate or EC2).
- **Caching:** Elasticache (Redis).
- **Database:** RDS.

#### **Flow:**
```
www.serverless.konkas.tech (DNS)
    ↓
CloudFront (CDN + SSL + WAF + Caching)
    ↓
Frontend (ReactJS static files hosted on S3, CORS enabled)
    ↓
Internal ALB (Private)
    ↓
Backend (NodeJS app running on ECS - Fargate or EC2)
    ↓
Elasticache (Redis for caching)
    ↓
RDS (Database)
```

#### **Key Features:**
- **Frontend** is fully **static** and hosted on **S3** (using CloudFront for CDN).
- **Backend** is running on **ECS**, either using **Fargate** (serverless) or EC2 launch types for more control over the environment.
- **Internal ALB** routes traffic to the backend.

#### **Advantages:**
- **Serverless frontend** with no server management required.
- **ECS for backend:** Flexible and scalable, with the option to use **Fargate** for serverless compute.
- **Lower operational overhead** compared to VMs.

#### **Challenges:**
- Some complexity in managing ECS, especially when handling tasks like networking and autoscaling.
- Costs could increase with high compute usage on ECS.

---

### **Project 3: ECS Hosted Architecture**

#### **Architecture Overview:**
- **Frontend:** ReactJS running in a Docker container in ECS.
- **Backend:** NodeJS running in a Docker container in ECS.
- **Caching:** Elasticache (Redis).
- **Database:** RDS.

#### **Flow:**
```
www.ecs.konkas.tech (DNS)
    ↓
CloudFront (SSL Termination + CDN + WAF)
    ↓
Internal ALB (Private ALB)
    ↓
ECS Service (Frontend + Backend containers)
    ↓
Elasticache (Redis)
    ↓
RDS (Database)
```

#### **Key Features:**
- Both **Frontend** and **Backend** run as **containers** in ECS, which can be managed with **EC2 instances** or **Fargate**.
- **Internal ALB** ensures secure routing to the backend services.
- **CloudFront** provides CDN and caching.

#### **Advantages:**
- **Containerized services:** Provides a consistent environment for both frontend and backend.
- **ECS** allows fine control over scaling, networking, and task definitions.
- **Efficient scaling** based on traffic.

#### **Challenges:**
- More complex to manage compared to serverless options (e.g., requires careful setup of task definitions, scaling policies, etc.).
- Slightly higher overhead compared to purely serverless models.

---

### **Project 4: EKS Hosted Architecture**

#### **Architecture Overview:**
- **Frontend:** ReactJS and NodeJS backend running in **EKS** pods.
- **Backend:** NodeJS running in **EKS** pods.
- **Caching:** Elasticache (Redis).
- **Database:** RDS.

#### **Flow:**
```
www.eks.konkas.tech (DNS)
    ↓
CloudFront (SSL Termination + CDN + WAF)
    ↓
ALB (Ingress Controller for EKS)
    ↓
EKS Cluster (Kubernetes)
    ↓
Frontend + Backend (Pod containers in EKS)
    ↓
Elasticache (Redis for caching)
    ↓
RDS (Relational Database)
```

#### **Key Features:**
- Both **frontend** and **backend** run inside **Kubernetes** (EKS) pods.
- **Ingress Controller** in the ALB routes traffic to the appropriate services inside the EKS cluster.
- **CloudFront** provides global CDN, SSL, and WAF capabilities.

#### **Advantages:**
- **Kubernetes-based** for **advanced container orchestration** and management.
- Flexible scaling and resource management within EKS.
- **Increased control** over the infrastructure (compared to ECS or serverless).

#### **Challenges:**
- More complex to set up and manage compared to ECS or serverless models.
- Requires deep knowledge of Kubernetes for effective management and scaling.

---

### **Summary of Differences:**

| **Feature**            | **VM-Based**            | **Serverless**         | **ECS Hosted**         | **EKS Hosted**         |
|------------------------|-------------------------|------------------------|------------------------|------------------------|
| **Frontend Hosting**    | VM (ASG)                | S3 (Static)            | ECS (Containerized)    | EKS (Pod containers)   |
| **Backend Hosting**     | VM (ASG)                | ECS (Fargate/EC2)      | ECS (Containerized)    | EKS (Pod containers)   |
| **Scaling**             | Manual/ASG              | Auto with Fargate      | ECS Auto-Scaling       | Kubernetes Horizontal Pod Auto-Scaling |
| **Infrastructure**      | Virtual Machines        | Serverless (No VM)     | Containers (ECS)       | Containers (EKS)       |
| **Complexity**          | High                    | Low                    | Medium                 | High                   |
| **Flexibility**         | High (Custom VMs)       | Low                    | Medium (ECS)           | High (Kubernetes)      |
| **Cost**                | Moderate to High        | Low for low usage      | Moderate to High       | High for large workloads |
| **Management Overhead** | High                    | Low                    | Medium                 | High                   |

---

### 🚀 Next Steps:

- **VM-Based**: If you prefer legacy systems or need full control over VMs, this is great. But, it requires more manual management.
- **Serverless**: Great if you want minimal management and can adapt to a fully serverless architecture (with S3 for the frontend and ECS for the backend).
- **ECS**: If you want to run containerized workloads but need full control over containers, ECS is a balanced option with scalability.
- **EKS**: For highly scalable and advanced container orchestration using Kubernetes, this is a great option — but it comes with added complexity.
---
