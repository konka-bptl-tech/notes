### ✅ **Basic to Intermediate Scenarios**

1. **Your team wants to provision infrastructure for dev, staging, and production environments. How would you structure your Terraform code to avoid duplication?**

   * (Hint: Modules, `backend.tf`, `*.tfvars`, and environment-specific workspaces or directories)

2. **You updated a Terraform module and ran `terraform apply`, but it deleted a resource unexpectedly. What steps would you take to debug and prevent this in the future?**

3. **You need to share Terraform state between team members. What solution would you implement and why?**

   * (Hint: Remote backend like S3 + DynamoDB for state locking)

4. **You made changes in the `main.tf` file, but after running `terraform plan`, nothing is detected. What could be the reasons?**

5. **How do you import existing AWS infrastructure (e.g., an EC2 instance) into your Terraform state file without recreating it?**

---

### ✅ **Advanced Scenarios**

6. **Your Terraform apply fails due to dependency errors. How do you identify and resolve these dependency issues?**

   * (Hint: `depends_on`, module relationships, circular dependencies)

7. **You are managing infrastructure using multiple modules and developers are making changes in parallel. How do you avoid Terraform state file conflicts?**

8. **You need to create a reusable module for deploying an ECS cluster. The module should be customizable (e.g., instance size, number of tasks). How would you design it?**

   * (Hint: Input variables, outputs, locals, version pinning)

9. **Your organization wants to enforce tagging policies (e.g., all resources must have `Environment`, `Owner`, and `Project`). How would you implement this using Terraform?**

10. **You have two modules: `network` and `compute`. `compute` depends on values (like subnet IDs) from `network`. How do you pass outputs from one module to another?**

---

### ✅ **CI/CD + Terraform Integration Scenarios**

11. **How would you integrate Terraform into a GitHub Actions pipeline for infrastructure-as-code deployments?**

* (Hint: Stages like `init`, `validate`, `plan`, `apply`, and approvals)

12. **You want to perform `terraform plan` on every PR and `terraform apply` only on merge to main. How would you configure this?**

13. **Terraform apply in the pipeline failed because of a drift in infrastructure. How do you investigate and handle this drift?**

---

### ✅ **Real-Time Problem Solving Scenarios**

14. **Your team has mistakenly deleted the Terraform state file. How do you recover and ensure no infrastructure is lost?**

15. **You deployed a VPC, EC2, and RDS instance, but now want to delete only EC2. How do you do this without affecting other resources?**

16. **You used hard-coded AMI IDs in your Terraform code. What are the risks, and how would you improve this setup?**

* (Hint: `data "aws_ami"` lookup, SSM parameters)

17. **You need to rotate AWS secrets periodically. How can you manage secrets rotation using Terraform along with AWS Secrets Manager?**

18. **Terraform fails due to API rate limits from AWS. How do you prevent or handle this situation efficiently?**

---
### ❓ Interview Question:

**"In a Terraform project, you updated a naming convention in your `local.name` variable which is used across multiple resources (e.g., changing from `deve-3-tier` to `deve-aws-3tier`). After running `terraform apply`, you noticed that some resources were simply updated with the new tags, while others were destroyed and recreated. Can you explain why this happened, and how you would avoid unnecessary resource destruction in such cases?"**


Konka, this is a **very sharp observation**, and you're touching on a **common but critical issue in Terraform**: how resource names (or `tags`, especially `Name`) affect **resource identity and lifecycle**.

---

### 🔍 What Happened:

You changed a naming convention, for example:

```diff
- "deve-3-tier"
+ "deve-aws-3tier"
```

And noticed that:

* ✅ Some resources **got updated in-place** (tags changed)
* ❌ Some resources were **destroyed and recreated**

---

### 💡 Why This Happens

Terraform tracks resources by:

* The **resource block** (`aws_instance.example`)
* The **provider-specific identifier** (like `id`, `name`, etc.)

When you **change inputs like `name`, `tags`, or identifiers** that are **used in resource arguments**, it behaves differently based on **how the provider treats those fields**:

---

### 🔄 1. **Tag change only → In-place update (no destroy)**

If you only changed a **tag**:

```hcl
tags = {
  Name = "deve-aws-3tier"
}
```

Terraform updates the tag in-place:

```shell
~ tags.Name: "deve-3-tier" => "deve-aws-3tier"
```

✅ Resource is updated **without destruction**

---

### ❌ 2. **Name used in resource argument → Resource is destroyed and recreated**

If you used `name` inside a field like:

```hcl
name = local.name
```

And that `local.name` changed from `deve-3-tier` to `deve-aws-3tier`, then:

* AWS thinks it's a **different resource**
* Terraform destroys the old and creates a new one

❗ This often happens with:

* RDS `identifier`
* IAM role names
* S3 bucket names
* Security group names (when explicitly named)
* ECS cluster names

---

### ✅ Best Practices to Avoid Unwanted Resource Destruction

1. **Avoid using dynamic names in `name` or `identifier` fields** unless absolutely needed.
2. For critical resources (e.g., RDS, EBS, S3), use **static names** or keep naming conventions consistent.
3. For flexibility, prefer using `tags.Name` over setting `name` directly.
4. Use `lifecycle` blocks if needed to **prevent destroy**, like this:

```hcl
lifecycle {
  prevent_destroy = true
}
```
---
### 🔍 Example

```hcl
resource "aws_db_instance" "default" {
  identifier = "${var.environment}-${var.project}-${var.db_name}" # changes here = destroy

  tags = {
    Name = "${var.environment}-${var.project}-${var.db_name}"     # changes here = safe update
  }
}
```
---
