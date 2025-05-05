```markdown
# AWS vs Azure vs GCP - Service Mapping

| **AWS Service**              | **Azure Equivalent**                        | **GCP Equivalent**                            | **Purpose / Notes** |
|-----------------------------|---------------------------------------------|-----------------------------------------------|----------------------|
| **Organizations**           | Management Groups / Subscriptions           | Organizations / Folders / Projects            | Central account and permission management |
| **IAM**                     | Azure Active Directory (AAD), RBAC          | Cloud IAM                                     | Identity and access management |
| **VPC**                     | Virtual Network (VNet)                      | Virtual Private Cloud (VPC)                   | Private networking |
| **NACL & SG**               | Network Security Group (NSG)                | Firewall Rules / Security Policies            | Network access control |
| **EC2**                     | Virtual Machine (VM)                        | Compute Engine                                | Virtual servers |
| **EBS**                     | Managed Disks                               | Persistent Disks                              | Block storage |
| **ALB**                     | Application Gateway                         | HTTP(S) Load Balancer                         | Layer 7 load balancing |
| **NLB**                     | Azure Load Balancer                         | Network Load Balancer                         | Layer 4 load balancing |
| **ASG & Launch Template**   | Virtual Machine Scale Sets (VMSS)           | Instance Groups + Instance Templates          | Auto-scaling compute resources |
| **S3**                      | Blob Storage                                | Cloud Storage                                 | Object storage |
| **EFS**                     | Azure Files                                 | Filestore                                     | Shared file storage |
| **Route53**                 | Azure DNS                                   | Cloud DNS                                     | Domain Name System management |
| **RDS**                     | Azure SQL / Azure Database for PostgreSQL   | Cloud SQL                                     | Managed relational databases |
| **DynamoDB**                | Cosmos DB (with Table API)                  | Firestore / Bigtable                          | NoSQL database |
| **ACM (Certs)**             | App Service Certificates / Key Vault        | Certificate Manager / Managed certs           | TLS certificate management |
| **Parameter Store**         | App Configuration / Key Vault               | Runtime Configurator / Secret Manager         | Config and parameter management |
| **Secrets Manager**         | Azure Key Vault                             | Secret Manager                                | Secure secret storage |
| **KMS**                     | Azure Key Vault (Keys)                      | Cloud KMS                                     | Encryption key management |
| **CloudFront**              | Azure Front Door / CDN                      | Cloud CDN                                     | Content Delivery Network |
| **CloudWatch**              | Azure Monitor / Log Analytics               | Cloud Monitoring + Logging                    | Monitoring & logging |
| **EventBridge**             | Event Grid / Event Hub                      | Eventarc / Pub/Sub                            | Event-driven architecture |
| **CloudTrail**              | Azure Activity Logs / Diagnostic Settings   | Audit Logs                                    | API activity logging |
| **CloudFormation**          | ARM Templates / Bicep / Terraform           | Deployment Manager / Terraform                | Infrastructure as code |
| **CodeCommit**              | Azure Repos                                 | Cloud Source Repositories                     | Git repositories |
| **CodeBuild**               | Azure Pipelines (Build)                     | Cloud Build                                   | Build automation |
| **CodeDeploy**              | Azure Pipelines / Deployment Center         | Cloud Deploy                                  | Deployment automation |
| **CodePipeline**            | Azure DevOps Pipelines                      | Cloud Build + Workflows                       | CI/CD pipeline orchestration |
| **ECR**                     | Azure Container Registry (ACR)              | Artifact Registry / Container Registry        | Docker image registry |
| **ECS**                     | Azure Container Instances / AKS             | Cloud Run / GKE                               | Container orchestration |
| **EKS**                     | Azure Kubernetes Service (AKS)              | Google Kubernetes Engine (GKE)                | Managed Kubernetes service |
| **Lambda Functions**        | Azure Functions                             | Cloud Functions                               | Serverless compute |
| **Elastic Cache**           | Azure Cache for Redis                       | Memorystore                                   | In-memory caching |
| **SQS**                     | Azure Queue Storage / Service Bus           | Pub/Sub                                       | Messaging and queuing |
| **GuardDuty**               | Microsoft Defender for Cloud                | Security Command Center                       | Threat detection and security monitoring |
```
---

