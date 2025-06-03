Here are detailed and organized notes based on your points, with additional clarifications and minor corrections added for completeness and clarity:

---

# **Elastic Block Store (EBS) – AWS**

Amazon EBS is a **block storage service** designed to persist data used with Amazon EC2 instances. EBS volumes are **network-attached**, durable, and reliable, providing **high availability** and **scalability** for critical workloads.

---

## **Key Features of EBS**

* **Elasticity**: Easily increase capacity and IOPS to meet application performance demands.
* **Durability**: Highly durable; ensures data persistence and reduces data loss.
* **Snapshots**: Support for **incremental backups** to capture point-in-time volume data.
* **Encryption**: Volumes can be encrypted for enhanced data security (can be enabled during creation or copied to a new encrypted volume).
* **Availability**: High availability within a single **Availability Zone (AZ)**.

> ⚠️ **Note:** EBS volumes **can only be mounted within the same AZ**. They are **not shared across AZs or regions** directly.

---

## **Types of EBS Volumes**

### **SSD-backed Volumes (Low Latency & High IOPS)**

| Type    | Use Case                        | Characteristics                                                                |
| ------- | ------------------------------- | ------------------------------------------------------------------------------ |
| **gp2** | General Purpose                 | Baseline 3 IOPS/GB, bursts allowed                                             |
| **gp3** | General Purpose (Preferred)     | Custom IOPS and throughput independent of volume size. **Lower cost than gp2** |
| **io1** | Provisioned IOPS                | Supports high IOPS, up to 64,000 IOPS per volume                               |
| **io2** | Provisioned IOPS (more durable) | Better durability and SLA compared to io1                                      |

> ✅ **Note**: In your organization, **99% of workloads use gp3** due to better cost-efficiency and performance.

### **HDD-backed Volumes (Throughput-intensive workloads)**

| Type    | Use Case                 | Characteristics                                                    |
| ------- | ------------------------ | ------------------------------------------------------------------ |
| **sc1** | Cold HDD                 | Low-cost, infrequent access                                        |
| **st1** | Throughput Optimized HDD | Good for large, sequential workloads like big data, log processing |

---

## **Volume Migration and Changes**

### **Migrating from gp2 to gp3**

* Plan migration with **stakeholders** before proceeding.
* Use AWS Console, CLI, or SDK to modify the volume type to gp3.
* Ensure that **IOPS and throughput are configured** as per application requirements.
* Minimal downtime; AWS allows online resizing and modification in most cases.

### **Encryption**

* **Unencrypted → Encrypted**:

  1. Create a snapshot of the unencrypted volume.
  2. Copy the snapshot with encryption enabled.
  3. Create a new encrypted volume from the copied snapshot.
* **Encrypted → Unencrypted**:

  * Create a snapshot and copy it with encryption disabled (only if permitted).

### **Cross-AZ and Cross-Region Volume Transfer**

* EBS volumes cannot be directly moved across AZs or regions.
* Steps:

  1. Create a **snapshot** of the volume.
  2. **Copy** the snapshot to the desired region (or AZ).
  3. Create a new volume from the copied snapshot.

### **Volume Update Wait Time**

* After modifying the volume (size/type/IOPS), **AWS may take up to 6 hours** to fully apply the update depending on volume state and data size.

---

## **Snapshots**

### **What is a Snapshot?**

* **Point-in-time backup** of an EBS volume.
* **Incremental**: Only changes since the last snapshot are saved, reducing storage costs.
* Stored in **Amazon S3** (managed by AWS).

### **Snapshot Behavior**

* If an EC2 instance has **3 volumes**, taking a snapshot creates **3 separate snapshots** (one per volume).
* You can **exclude specific volumes** when automating snapshots.

---

## **Amazon Data Lifecycle Manager (DLM)**

* Automates **snapshot creation, retention, copy, and deletion**.
* Simplifies backup management for EBS volumes and AMIs.
* Example policy: **Take a snapshot every 4 hours** for critical applications.
* Helps ensure compliance and recovery objectives.

---

## **Amazon Machine Image (AMI)**

* An **AMI** is a preconfigured template that contains:

  * Operating system
  * Application server
  * Application software
  * EBS volume configurations

### **Use Cases**

* Launch new EC2 instances with predefined configurations.
* Create a backup image of an EC2 instance.
* Share and replicate environments.

---

## ✅ **Summary**

| Feature               | Notes                                    |
| --------------------- | ---------------------------------------- |
| EBS Scope             | Only within the same AZ                  |
| Preferred Volume      | gp3 (in your organization)               |
| Backup                | Snapshots (incremental, stored in S3)    |
| Automation            | Use Data Lifecycle Manager               |
| Encryption            | Supported, can be applied via snapshots  |
| Cross-Region Transfer | Use snapshot copy                        |
| AMI                   | Complete instance backup with OS and app |

---
### **What is Instance Store in AWS?**

**Instance Store** (also known as **ephemeral storage**) is a **temporary block-level storage** that is **physically attached** to the host computer where the EC2 instance runs.

---

## ✅ **Key Characteristics of Instance Store**

| Feature              | Description                                                                 |
| -------------------- | --------------------------------------------------------------------------- |
| **Type**             | Temporary block storage (not persistent)                                    |
| **Attachment**       | Directly attached to the **EC2 host hardware**                              |
| **Performance**      | Very high IOPS and low latency (good for temporary data)                    |
| **Data Persistence** | **Data is lost** when the instance is stopped, terminated, or fails         |
| **Cost**             | Included in the cost of specific instance types (no separate cost like EBS) |
| **Backup**           | Cannot be backed up with snapshots                                          |
| **Use Cases**        | Caching, buffers, scratch data, temporary files                             |

---

## ⚠️ **When is Data Lost?**

Data stored in an instance store **is lost** when:

* The instance is **stopped or terminated**
* The underlying **hardware fails**
* The instance is **rebooted to a different host**

---

## 📌 **When to Use Instance Store?**

Use **only when you don’t need data persistence**, such as:

* Temporary storage
* Application caches
* Intermediate processing data (like Hadoop jobs)
* Buffer files

---

## 🚫 **Limitations**

* Cannot be detached or re-attached like EBS
* Not available for all instance types (mostly supported by older generation EC2 instances or some high-performance types like **i3, d2, h1**)
* No support for encryption at rest (without manual setup)

---

## 🔁 **Difference: Instance Store vs EBS**

| Feature              | Instance Store             | EBS                               |
| -------------------- | -------------------------- | --------------------------------- |
| Persistence          | **Temporary**              | **Persistent**                    |
| Backup               | ❌ Not possible             | ✅ Snapshot supported              |
| Available Across AZs | ❌ No                       | ✅ Snapshots allow cross-AZ/region |
| Performance          | Very high for short term   | Configurable via IOPS             |
| Cost                 | Included in some EC2 types | Pay-as-you-go                     |
| Encryption           | Manual                     | Integrated with AWS KMS           |

---

## ✅ **Summary**

* **Instance store is temporary, high-performance storage.**
* Ideal for **non-critical**, fast-access temporary data.
* Use **EBS** for any data you want to **persist beyond instance lifecycle**.
---

