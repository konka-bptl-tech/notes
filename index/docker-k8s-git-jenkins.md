---
# Docker
---
# Day-1
- Deployment on physical,vm and containers and it's challenges
- Containerization and Docker
- Docker Architecture
- Container & LifeCycle
- Docker Basic Commands
  [logs,inspect,stats,run,rmi,rm,stop,pause,unpause,prune,system]
# Day-2
- Dockerfile instructions
- Docker Layers
- Docker Compose
# Day-3
- volumes
  - named & Annoymous volumes
  - BindMounts
- Dockerize Expense Project
- Dockerize Instana Project[Try YourSelf]
# Day-4
- Networking
  - Bridge Network[Default&Custom]
  - host
  - none
# Day-5
- Docker Compose to run expense and instana
- namespaces & cGroups
- Docker Best practices
---
# Kubernetes[Phase-1]
---
# Day-1[6]
- Introdcution
- Architecture 
- K8s Features
- kubeadm manual
- Namespaces
# Day-2[7]
- Pod
- Basic Commands
- init container
- Pause Container
# Day-3[8]
- Labels & Selectors
- ReplicaSet & Replication Controller
- Deployment
# Day-4[9]
- Resource Management
- Metrics-Server
- HPA
- Job Controller & CronJob Controller
# Day-5[10]
- Services
  - ClusterIP
  - NodePort
  - LoadBalancer
  - ExternalName
- CoreDNS pods
- kube-dns service
# Day-6[11]
- StatefulSet
- Headless Service
# Day-7[12]
- emptyDir
- hostPath
- Storage Orchestration
 - Static Volume Provisioning
 - Dynamic Volume Provisioning
- Volume Access Modes
- Reclaim Policy
# Day-8[13]
- ConfigMap
- Secrets
- Probes
# Day-9[14][09]
- Manifest files creation for expense and instance[try yourself]
# Day-10[15][10]
- Helmify expense and instana[try yourself]
# Day-11[16][11]
- CRD's & Operators
- Ingress
- Annotations
- HashiCorp Vault
- ExternalSecretOperator
- ExternalDNS
# Day-12[17][Admin][12]
- Serivce Account
- RBAC
# Day-13[18][Admin][13]
- Network Policies
- Resource Quotas
- Limit Ranges
# Day-14[19][Adv Scheduling][14]
- NodeSelector
- Affinity & Anti-Affinity
- Pod Affinity & Anti-Affinity
# Day-15[20][Adv Scheduling][15]
- Taints & Tolerations
# Day-16[21][16]
- k8s errors simulation
# Day-17[22][17]
- EKS setup with Terraform

---
Git&Github
---
# Day-1[23][18]
- What is Git,Github & Why
- Git Basics
- Branches creation
- Merge,Rebase & Cherry-Pick
- Git Stash
# Day-2[24][19]
- Reset & Revert
- Brnaching Strategy
- Git Tags
- GitHub Actions basics
---
# Jenkins
---
# Day-1[25][20]
- SDLC
  - waterfall
  - agile
  - DevOps[CI-CD]
  - DevSecOps[CI-CD]
- Jenkins Introduction
# Day-2[26][21]
- Jenkins Distributed Architecture[Control-Plane+Build-Node]
- FreeStyle Jobs
- Pipelines Examples
  - Scripted
  - Declarative
- Declarative Pipeline Syntax go through
# Day-3[27][22]
- Jenkins Shared Libs
- Multi Branch pipelines
# Day-4[28][23]
- Prometheus Grafana[Metrics]
# Day-5[29][24]
- ELK[Logs]
# Day-6[30][25]
- CI pipeline
# Day-7[31][26]
- CD Pipeline
# Day-8[32][Adminstrator]
- RBAC
- Backup & Restore and Version Upgrade
# Day-9[33][27]
- GitOps
- ArgoCD
---
# Kubernetes[Phase-2]
# Day-1[34]
- Monitoring K8s Cluster With Prometheus and Grafana
# Day-2[35]
- Daemonset
- hostPath
- FileBeats to collect Logs and push to LogStash
# Day-3[36]
- Karpenter
# Day-4[37]
- Pod Security
- Pod Security Admission (PSA)
# Day-5[38]
- Istio[Tracing]
# Day-6[39]
- Velero
# Day-7[40]
- Cluster Upgradation
# Day-8
- AWS CLI[Application Modernization]
# Day-9
- Terraform[Application Modernization]




























































Got it! Here's the explanation with emojis but **without bold or markdown styling**:

---

🌱 CRD (Custom Resource Definition)

- What is it?  
  A CRD lets you create your own custom resource types in Kubernetes — just like how Pods, Services, or Deployments are resource types.

- Why use it?  
  When the default Kubernetes resources aren't enough for your app's needs, you can define your own.

- Example:  
  You can create a custom resource called MyDatabase with fields like replicas, storageSize, etc.

---

🤖 Operator

- What is it?  
  An Operator is a controller that understands how to manage your custom resource (usually built around a CRD).

- What does it do?  
  It automates operational tasks like:  
  - Deploying  
  - Backing up  
  - Upgrading  
  - Scaling

- How?  
  It watches for changes to your custom resource and reacts accordingly — just like how the Deployment controller watches Deployments and creates Pods.

---

🎯 Real-world Analogy

- CRD = A new type of form you introduce in an office, like "VacationRequestForm".
- Operator = A smart assistant who watches for these forms, processes them, sends approval emails, and books flights.

---

🧠 Summary

| Concept   | Purpose                                   |
|-----------|-------------------------------------------|
| CRD       | Defines a new type of Kubernetes resource |
| Operator  | Automates how to manage that resource     |

Let me know if you want this explained with YAML or a visual diagram!