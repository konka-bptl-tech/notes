# 🛡️ AWS IAM (Identity and Access Management) - Notes
---
## 1. 🔐 **What is Authentication?**
* **Authentication** means **verifying who you are**.
* Example: Logging in with a username and password or access key in AWS.
---
## 2. ✅ **What is Authorization?**
* **Authorization** means **what you are allowed to do**.
* Example: A user may be authenticated but only allowed to read from S3, not write.
---
## 3. 🌍 **IAM Basics**
* IAM is a **Global AWS service** (not region-specific).
* Helps **control access** to AWS services and resources securely.
* Uses **Users**, **Groups**, **Roles**, and **Policies** to manage access.
---
## 4. 🔧 **Key IAM Features**
* **Centrally manage** users, roles, groups, and permissions.
* Enforce **least privilege**: give only the minimum access needed.
* Supports **MFA (Multi-Factor Authentication)** for added security.
* Shares access securely through **roles**, especially for applications or cross-account access.
---
## 5. 👤 **IAM Components**
### 🧑‍💻 IAM Users
* Represents a person.
* Has **long-term credentials**: username/password or access keys.
### 👥 IAM Groups
* Collection of users with common permissions.
* **Policies attached to group** are inherited by all users in it.
### 🎭 IAM Roles
* Used by AWS services, users, or external identities.
* **No long-term credentials**; generates **short-lived credentials** via STS.
* Not tied to any specific user or group.
* Can be **assumed** by trusted entities using a **Trust Policy**.
---
## 6. 🆔 **Identity in IAM**
* An **identity** is a user, group, or role that can have permissions to use resources.
* All identities are controlled through policies.
---
## 7. 🔑 **STS & Temporary Credentials**
### 📥 `sts:GetCallerIdentity`
* Returns:
  * `UserId`
  * `Account`
  * `ARN`
* Helps confirm **who is calling the API** — useful for debugging access issues.
### ⏳ STS Tokens
* Temporary credentials used by roles or federated users.
* Include:
  * Access key
  * Secret key
  * Session token
  * Expiration time
---
## 8. 🤝 **Trust Policies**
* Define **who can assume a role**.
* Example: An EC2 instance or another AWS account can assume a role if allowed in the trust policy.
---
## 9. 🔐 **Identity Providers**
* Used to **federate external identities** (like Google, AD, Okta) with IAM roles.
* IAM supports:
  * SAML 2.0
  * OIDC (e.g., Cognito)
  * Social identity providers
---
## 10. 📜 **IAM Policies (Permissions)**
| Type                 | Description                                     |
| -------------------- | ----------------------------------------------- |
| **AWS Managed**      | Predefined by AWS, reusable across accounts     |
| **Customer Managed** | Created by you, reusable and customizable       |
| **Inline**           | One-to-one: strictly bound to a single identity |
### 🧾 **Policy Example**
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:PutObject", "s3:GetObject"],
      "Resource": "arn:aws:s3:::my-bucket/*"
    }
  ]
}
```
🔍 **Explanation**:
* **Effect**: Allow or Deny.
* **Action**: List of allowed actions (S3 operations here).
* **Resource**: Specifies which resources this applies to (all objects in `my-bucket`).
---
## 11. 🛡️ **Permission Boundary**
* A **guardrail** for IAM roles or users.
* Even if a policy allows an action, the permission boundary must also allow it.
* Think of it like a **maximum permission limit**.
---
## 12. 🏷️ **What is ARN (Amazon Resource Name)?**
* A **unique identifier** for every AWS resource.
* Format:
  ```
  arn:partition:service:region:account-id:resource
  ```
* Example:
  ```
  arn:aws:iam::123456789012:user/konka
  ```
---
## 13. 🕵️ **Access Analyzer**
* Helps **detect unintended access** to your AWS resources.
* Can analyze IAM policies and suggest improvements.
---
## 14. 📄 **Credential Report**
* A downloadable report showing:
  * Users
  * Password and access key usage
  * MFA status
  * Last login time
* Great for auditing.
---
## ✅ **IAM Best Practices**
1. **Enable MFA** for all users.
2. Follow **Least Privilege** principle.
3. Use **Roles instead of Users** for applications and EC2 instances.
4. Rotate **Access Keys** regularly.
5. Monitor activity using **CloudTrail + IAM Access Analyzer**.
6. Use **Groups** to manage permissions, not attaching policies directly to users.
7. **Review and clean up** unused users, roles, and keys regularly.
8. Avoid using the **root user**; lock it down with MFA.
---
## 🔐 IAM Access Control Models
### 1. 🎭 **RBAC – Role-Based Access Control**
* Access is **based on roles**.
* Users are assigned to **roles**, and **roles have permissions**.
* Easy to manage for a **fixed team structure** (e.g., Dev, QA, Admin).
✅ **Example:**
```bash
DevOps role → access to EC2, S3  
QA role → access to S3 only
```
✅ **Use Case:**
> "Give all developers read/write access to S3, but only admins can access EC2."
---
### 2. 🧬 **ABAC – Attribute-Based Access Control**
* Access is based on **tags (attributes)**, not just roles.
* IAM policies use **conditions** based on tags like:
  * `aws:username`
  * `aws:ResourceTag`
  * `aws:PrincipalTag`
✅ **Example:**
```json
"Condition": {
  "StringEquals": {
    "aws:ResourceTag/Project": "${aws:PrincipalTag/Project}"
  }
}
```
→ This allows a user to access resources **only if their project tag matches**.
✅ **Use Case:**
> "Allow users to access only the S3 buckets that match their department tag."
---
## 🔄 RBAC vs ABAC Summary
| Feature                 | RBAC                         | ABAC                                              |
| ----------------------- | ---------------------------- | ------------------------------------------------- |
| Access Control Based On | **Roles**                    | **Tags/Attributes**                               |
| Flexibility             | Less flexible (static)       | More flexible (dynamic)                           |
| Scaling                 | Manual role management       | Tag-based automation                              |
| Example                 | Dev role → EC2, QA → S3      | Allow access to `Project=A` tagged resources only |
| Best Use                | Small teams, fixed structure | Large orgs, dynamic workloads                     |
---
✅ **In AWS:**
* ABAC is powerful when using **tags in policies**, especially with **organizations, federated users, and automation**.
* RBAC is easier to start with, especially for **small or static setups**.
---
### ❓**Q1: What is the use case you are solving with ABAC in IAM?**
**🅰️ Answer:**
We have 2 teams — **Dev Team** and **DB Team**, and we want to control access based on the environment:
* **Dev DB Team** should have **full access only to Dev DB**.
* **Prod DB Team** should have **full access to Prod DB**, but **only read-only access (describe)** to Dev DB.
This fine-grained access control is **dynamic and scalable**, which is why we used **ABAC (Attribute-Based Access Control)** based on tags.
---
### ❓**Q2: What tags are used to control access?**
**🅰️ Answer:**
We used two tags for both IAM identities and resources:
* `Team` → To differentiate between dev, db, etc.
* `Environment` → To separate `dev`, `prod`, etc.
---
### ❓**Q3: How are users/roles and resources tagged?**
**🅰️ Answer:**
| Identity/Resource       | Tag\:Team | Tag\:Environment |
| ----------------------- | --------- | ---------------- |
| Dev Engineer (IAM Role) | dev       | dev              |
| DB Engineer Dev         | db        | dev              |
| DB Engineer Prod        | db        | prod             |
| Dev RDS Instance        | db        | dev              |
| Prod RDS Instance       | db        | prod             |
---
### ❓**Q4: What IAM policy is attached to the Dev DB team?**
**🅰️ Answer:**
```json
{
  "Effect": "Allow",
  "Action": "rds:*",
  "Resource": "*",
  "Condition": {
    "StringEquals": {
      "aws:ResourceTag/Team": "db",
      "aws:ResourceTag/Environment": "dev"
    },
    "StringEqualsIfExists": {
      "aws:PrincipalTag/Team": "db",
      "aws:PrincipalTag/Environment": "dev"
    }
  }
}
```
---
### ❓**Q5: What IAM policy is attached to the Prod DB team?**
**🅰️ Answer:**
```json
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "rds:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Team": "db",
          "aws:ResourceTag/Environment": "prod"
        },
        "StringEqualsIfExists": {
          "aws:PrincipalTag/Team": "db",
          "aws:PrincipalTag/Environment": "prod"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "rds:Describe*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Team": "db",
          "aws:ResourceTag/Environment": "dev"
        }
      }
    }
  ]
}
```
* Full access to **Prod DB**
* **Describe-only** access to **Dev DB**
---
### ❓**Q6: Why not use RBAC here?**
**🅰️ Answer:**
RBAC would require creating separate roles and policies for each combination (Dev DB, Prod DB, etc.), making it **hard to scale**.
ABAC allows us to **dynamically control access** using **tags**, which is easier to manage for **large and changing teams**.
---
## ✅ Scenario Summary
| IAM User   | IAM Policy             | Bucket Policy on Bucket        | Final Access Result |
| ---------- | ---------------------- | ------------------------------ | ------------------- |
| **user01** | Full access to all S3  | Explicit Deny in bucket01      | ❌ Denied            |
| **user02** | No S3 access (default) | Full Access in bucket02 policy | ✅ Allowed           |
---
## ✅ Q\&A Format
---
### ❓**Q1: What happens when `IAM-user01` has full S3 access via IAM policy, but `bucket01` has an explicit deny for this user?**
**🅰️ Answer:**
Even though `user01` has full S3 access via IAM policy, **the bucket policy has an explicit deny**.
Since **explicit deny always overrides allow**, the **final result is DENIED**.
> 🔐 IAM and bucket policies are evaluated together — **explicit deny wins** over any allow.
---
### ❓**Q2: What happens when `IAM-user02` has no IAM permission for S3, but bucket02’s policy allows full access to `user02`?**
**🅰️ Answer:**
S3 bucket policies can **grant permissions** without needing the user to have IAM permissions — if the bucket policy allows it.
Since there is **no deny in IAM policy**, and **allow in the bucket policy**, the **final result is ALLOWED**.
> ✅ S3 bucket policies can **override lack of IAM permissions**, unless IAM or SCPs explicitly deny.
---
## 🔍 IAM + Bucket Policy Evaluation Logic
| IAM Allow         | Bucket Allow | Access |
| ----------------- | ------------ | ------ |
| ✅                 | ✅            | ✅      |
| ❌                 | ✅            | ✅      |
| ✅                 | ❌ (deny)     | ❌      |
| ❌                 | ❌            | ❌      |
| ❌ (explicit deny) | ✅            | ❌      |
---
## 💡 Key Concepts:
* ✅ **Allow + Allow = Allow**
* ❌ **Explicit Deny (in either IAM or Bucket Policy) = Deny**
* ❌ **No Allow = Deny**
* ✅ Bucket policy can grant access even if IAM policy doesn’t, unless blocked by explicit deny
---