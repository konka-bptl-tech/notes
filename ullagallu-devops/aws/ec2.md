## 🖥️ EC2 - Elastic Compute Cloud
### ✅ Overview
* Used to **create virtual servers** in the cloud.
* Offers **resizable compute capacity**.
* Integrated with **other AWS services**.
* **Secure** via:
  * **Security Groups (SGs)**
  * **Key pair** (SSH access)
* Offers **scalability** and **flexible pricing** options.
---
### 💡 Features
* Variety of **instance types** for different workloads.
* Choose **OS**: Amazon Linux, Ubuntu, etc.
* Control IPs: Use **EIP** (Elastic IP) to keep static IP even after reboot.
* Use **User Data** for running scripts at launch (only runs once).
---
### 🧠 Instance Types
1. **General Purpose** – Balanced \[e.g., `t`, `m`]
2. **Compute Optimized** – High CPU \[e.g., `c5`]
3. **Memory Optimized** – RAM-heavy workloads \[e.g., `r5`, `x1`]
4. **Storage Optimized** – High IOPS \[e.g., `d`, `i`]
5. **GPU Instances** – Graphics-heavy workloads
6. **FPGA Instances** – Custom hardware acceleration
---
### 💾 Device Mapping
* `/dev/xvda` → Amazon Linux
* `/dev/sda1` → Ubuntu
---
### 📊 Status Checks
1. **System Status Check** – Issues in AWS infra
2. **Instance Status Check** – Issues inside your instance (e.g., boot failure)
3. **Volume Status Check** – EBS volume health
---
### 🌐 EIP (Elastic IP)
* A **static public IP** that doesn’t change after reboot.
* ✅ Yes, **EIP can be attached to Spot Instances**, but it’s risky as Spot can be terminated anytime.
---
### 💸 Pricing Models
1. **On-Demand** – Pay per hour/second, no commitment
2. **Spot Instances** – Up to 90% cheaper, but can be interrupted
3. **Savings Plans** – Commitment-based discount
4. **Reserved Instances (RI)** – 1/3 year commitment, cheaper than On-Demand
5. **Dedicated Hosts** – Physical servers for license-bound workloads
6. **Dedicated Instances** – Run on hardware dedicated to you
7. **Scheduled Instances** – Run at specific times
8. **Capacity Reservations** – Reserve capacity in a specific AZ
---
### 🛠️ Common EC2 Issues – What to Do?
* Raise **AWS Support Ticket**.
* Request **Service Quotas Increase** (e.g., more VPCs, EIPs).
* **IAM-related support** requests can only be raised from **North Virginia**.
* Use a **Support Plan** (e.g., Developer, Business, Enterprise).
---
### 🧾 Notes on Scripts & Automation
* **User Data**: Runs only **once** on first boot.
* For repeated execution (e.g., config updates), use **Terraform `null_resource`** with **file hash triggers**.
---
## 💾 EBS – Elastic Block Store
### ✅ Overview
* Block storage for **EC2 instances**.
* **Persistent**, **highly available**, and **reliable**.
* Used for: OS boot volumes, app data, backups, and databases.
---
### ⚙️ Key Features
* **Elasticity** – Resize anytime
* **Durability** – 99.999% availability
* **Snapshotting** – For backup & restore
* **Encryption** – Optional (at rest + in transit)
* **AZ-Scoped** – Must attach within the same AZ
* **Availability** – Auto replication in the same AZ
---
### 📦 Volume Types
1. **General Purpose SSD**
   * `gp2`: Default, burst-based performance
   * `gp3`: Cost-efficient, configurable IOPS and throughput
     > ✅ *We use `gp2`, planning to migrate to `gp3`*
2. **Provisioned IOPS SSD**
   * `io1`, `io2`: For high-performance DB workloads
3. **Throughput Optimized HDD**
   * `st1`: For streaming large volumes of data
4. **Cold HDD**
   * `sc1`: Low-cost storage for infrequently accessed data
---
### 🔒 Encryption
* **Not used** in most real-time use cases unless compliance needed.
* **Snapshots** of encrypted volumes remain encrypted.
* To encrypt an **unencrypted volume**:
  * Create snapshot → Copy snapshot with "Enable Encryption" → Restore.
---
### 🛠️ Mounting Process
```bash
lsblk
blkid
sudo file -s /dev/xvdf
sudo yum install xfsprogs -y
sudo mkfs -t xfs /dev/xvdf
sudo mkdir /data
sudo mount /dev/xvdf /data
```
---
### 🔄 Modify Volumes
* Volume can be resized (e.g., 400GB → 500GB → 600GB)
* Must **wait \~6 hours** between modifications.
---
### 📌 Detaching Best Practices
* Always **unmount** before detaching.
* Direct detach without unmounting can cause **data corruption**.
---
### 📸 Snapshots
* **Point-in-time** backup of EBS volume.
* Stored in **S3** (not user-visible).
* **Incremental** – Only changed blocks are saved.
* Includes **data, settings, configurations**.
* Used for **backups** and **disaster recovery (DR)**.
---
### 🧱 Instance Store (Ephemeral Storage)
| Feature          | Description                                                       |
| ---------------- | ----------------------------------------------------------------- |
| Type             | Temporary block-level storage physically attached to the EC2 host |
| Use Case         | High-speed cache, buffers, temp data                              |
| Data Persistence | ❌ Lost if instance stops or fails                                 |
| Performance      | ⚡ Very high (directly attached NVMe or SSD)                       |
| Backup possible  | ❌ No                                                              |
| Volatile         | Yes – suitable only for non-critical data                         |
> 📝 Instance store is not available for all instance types (e.g., `t2.micro` doesn’t support it).
---
### 📦 AMI – Amazon Machine Image
* Pre-built template to launch EC2s (OS + apps + settings).
* Immutable – can launch as many instances as needed.
* For region copy:

  * Snapshot the root volume → Copy snapshot to new region → Create AMI there
---
### 🔁 Amazon Data Lifecycle Manager (DLM)
* Automatically manages:
  * Snapshots
  * AMI creation
  * Retention & deletion policies
---