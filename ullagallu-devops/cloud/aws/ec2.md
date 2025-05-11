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
Absolutely, Konka! Here's the **enhanced version** of your EBS notes with **Throughput** and **IOPS** details clearly explained and included in the right context.

---

## 💾 EBS – Elastic Block Store

### ✅ Overview

* Block-level storage used with EC2.
* Provides **persistent**, **durable**, and **highly available** storage.
* Suitable for **boot volumes**, **databases**, **app data**, and **backups**.

---

### ⚙️ Key Features

* **Elasticity** – Can resize volumes on the fly.
* **Durability** – 99.999% availability within a single AZ.
* **Snapshotting** – Create point-in-time backups.
* **Encryption** – Optional (KMS or default key).
* **AZ-Scoped** – Volumes must be attached to EC2s in the same AZ.
* **Availability** – Replicated automatically within the AZ.

---

### 📊 Performance Metrics

| Metric                                          | Description                                                                                                             |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **IOPS** *(Input/Output Operations Per Second)* | Measures number of read/write operations per second. Important for **transaction-heavy workloads** like databases.      |
| **Throughput**                                  | Measures **data transfer rate** (MB/s). Important for **large, sequential workloads** like video streaming or big data. |
> gp2, gp3, io1, and io2 support **IOPS**
> gp3, st1 support high **throughput**
---
### 📦 Volume Types
| Type                               | IOPS                                  | Throughput       | Use Case                                   |
| ---------------------------------- | ------------------------------------- | ---------------- | ------------------------------------------ |
| **gp2** (General Purpose SSD)      | Baseline 3 IOPS/GB (burst up to 3000) | Up to 250 MB/s   | Default volume, general use                |
| **gp3** (General Purpose SSD)      | Up to 16,000 (configurable)           | Up to 1,000 MB/s | Better cost & performance than gp2         |
| **io1/io2** (Provisioned IOPS SSD) | Up to 64,000                          | Up to 1,000 MB/s | Databases, critical apps needing high IOPS |
| **st1** (Throughput-Optimized HDD) | 500 IOPS (burst)                      | Up to 500 MB/s   | Big data, log processing, data lakes       |
| **sc1** (Cold HDD)                 | 250 IOPS (burst)                      | Up to 250 MB/s   | Archive, infrequently accessed data        |
> ✅ *We are currently using `gp2` volumes and planning to migrate to `gp3`.*
---
### 🔒 Encryption
* Default disabled. Can enable during volume or snapshot creation.
* **Snapshots of encrypted volumes remain encrypted.**
* To encrypt an unencrypted volume:
  1. Take snapshot
  2. Copy snapshot with encryption enabled
  3. Create a new encrypted volume from it
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
* You can resize volumes (e.g., 400GB → 500GB → 600GB).
* Need to **wait \~6 hours** before another resize.
* For `gp3`, you can **adjust size, IOPS, and throughput** independently.
---
### 📌 Detaching Best Practices
* Always **unmount** before detaching.
* Detaching directly without unmounting may cause **data corruption**.
---
### 📸 Snapshots
* **Point-in-time backup** stored in **S3**.
* **Incremental** – Only changed blocks are backed up.
* Used for:
  * Disaster Recovery
  * Backup
  * Region migration
---
### 🧱 Instance Store (Ephemeral Storage)
| Feature     | Description                                                     |
| ----------- | --------------------------------------------------------------- |
| Type        | **Temporary** block storage physically attached to the EC2 host |
| Lifecycle   | Data is lost if the instance is stopped/terminated              |
| Use Case    | High-speed cache, buffer, temp files                            |
| Performance | Extremely fast (NVMe/SSD-based)                                 |
| Backup      | ❌ Not possible                                                  |
| Data Safety | ❌ Volatile – Do not use for persistent data                     
> Not available on all instance types (e.g., T2). Use only for **non-critical temp storage**.
---
### 📦 AMI – Amazon Machine Image
* Pre-configured template with OS + settings + app config.
* Launch as many EC2s as needed – ensures **immutability**.
* To copy AMI to another region:
  * Snapshot → Copy snapshot to region → Create AMI from it
---
### 🔁 Amazon Data Lifecycle Manager (DLM)
* Automates:
  * Snapshot creation
  * AMI lifecycle
  * Retention & deletion
---
### ✅ 1. **What is IOPS?**
IOPS = **Input/Output Operations Per Second**
* Measures **how many read/write operations** happen in 1 second.
* Think of it as **speed for small tasks** like reading/writing rows in a database.
📌 Example: Reading 100 small records from MySQL per second → **needs high IOPS**
---
### ✅ 2. **What is Throughput?**
Throughput = **How much data (MB/s) can be transferred per second**
* Measures **data transfer rate** – ideal for large files.
* Think of it as **speed for big data transfers**.
📌 Example: Copying a 1GB log file continuously → **needs high Throughput**
---
### 🔁 Key Difference (Short and Sweet)
| Term           | Best for...                  | Think of it like...        |
| -------------- | ---------------------------- | -------------------------- |
| **IOPS**       | Many **small reads/writes**  | How fast you can do tasks  |
| **Throughput** | Fewer **large reads/writes** | How much data you can move |
---
### ✅ 3. **Which EBS Volumes Support What?**
| EBS Volume  | IOPS Focused?               | Throughput Focused?      | Use Case             |
| ----------- | --------------------------- | ------------------------ | -------------------- |
| **gp3**     | ✅ Yes (up to 16,000)        | ✅ Yes (up to 1,000 MB/s) | Web apps, DBs        |
| **gp2**     | ✅ Yes (scales with size)    | 🚫 Not optimized         | General use          |
| **io1/io2** | ✅ Best (up to 256,000 IOPS) | ✅ High throughput too    | High-end DBs         |
| **st1**     | 🚫 Not many IOPS            | ✅ Best (up to 500 MB/s)  | Big data/logs        |
| **sc1**     | 🚫 Very low                 | 🚫 Very low              | Archive/cold storage |
---
### 🔚 In One Line:
* **Choose IOPS** when you need **speed for small operations** (databases).
* **Choose Throughput** when you need **fast transfer of large files** (big data).
---
### ✅ 1. **What is Latency?**
**Latency = Delay.**
It means **how long it takes to start a read/write operation** after a request is made.
📌 Think of it as **waiting time** before the action begins.
---
### 🔁 Difference between IOPS, Throughput, and Latency:
| Term           | Meaning                               | Example                              | Focus Area                           |
| -------------- | ------------------------------------- | ------------------------------------ | ------------------------------------ |
| **IOPS**       | Number of operations per second       | Reading 100 small records per second | Speed of processing many small tasks |
| **Throughput** | Amount of data transferred per second | Copying a 1GB video in 2 seconds     | Speed of moving large files          |
| **Latency**    | Time delay before operation starts    | 5 ms delay before DB query starts    | How quickly the system reacts        |
---
### ✅ 2. **Analogy (Fast Food Drive-Thru)**
* **Latency** = Time to reach the counter and place the order.
* **IOPS** = How many small meals the kitchen can serve per second.
* **Throughput** = How many **kgs of food** are coming out per second.
---
### ✅ 3. Why Latency Matters?
* **Low latency** = Faster **response time**, critical for **real-time apps** like:
  * Databases
  * Web applications
  * Financial systems
📌 AWS premium volumes like **io2 Block Express** offer **ultra-low latency (sub-millisecond)** for high-performance use cases.
---
### ✅ 4. Summary (When to Focus on What?)
| Situation                          | You Care About...           |
| ---------------------------------- | --------------------------- |
| Real-time app (DB, APIs)           | **Low Latency** + High IOPS |
| Heavy read/write DB (Postgres)     | **High IOPS**               |
| Big file processing (logs, videos) | **High Throughput**         |
---
Would you like a real-world scenario (e.g., database vs. data pipeline) to connect all three terms together?

