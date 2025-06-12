# Manual Provision,Iac and Terraform

When we provision infrastructure manually—like setting up servers, networks, storage, or databases—it often leads to several problems. Manual provisioning is **time-consuming** and increases the **chances of errors or misconfigurations**, which can affect application availability and performance. It’s also difficult to **replicate the same setup across multiple environments** like development, testing, and production. Keeping track of what resources were created, managing dependencies, and doing **cost analysis** becomes challenging without any centralized tracking. Performing actions like create, update, or delete manually can lead to **dependency issues** or accidental changes.

To solve these problems, the industry moved to a better approach called **Infrastructure as Code (IaC)**. With IaC, infrastructure is defined using code and stored in files. This allows teams to **version control** their infrastructure, just like application code, and makes **collaboration easier** among developers. It also becomes simple to **replicate infrastructure**, scale it when needed, and maintain **consistency across environments**. Cost tracking and auditing are easier because everything is written in a readable format.

**Terraform** is a widely-used IaC tool that supports this approach. It uses a **state file** to keep track of all resources it creates, which helps in managing and updating infrastructure smoothly. Terraform also provides **modules** so we can reuse code for similar setups, saving time and effort. It automatically handles **internal dependencies** between resources, and if needed, we can define **external dependencies** using `depends_on`. Overall, Terraform makes infrastructure **automated, reliable, repeatable, and easier to manage**.

# Terraform Blocks

### 🔧 1. `terraform` Block

This block is used to configure **Terraform settings**, such as the backend (where state is stored) and required provider versions.

```hcl
terraform {
  required_version = ">= 1.5.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  backend "s3" {
    bucket = "my-tf-state"
    key    = "env/dev/terraform.tfstate"
    region = "us-east-1"
  }
}
```

---

### 🌍 2. `provider` Block

Defines the **cloud provider or service** you are using (e.g., AWS, Azure, GCP).

```hcl
provider "aws" {
  region = "us-east-1"
}
```

---

### 🏗️ 3. `resource` Block

This is used to create actual **infrastructure resources** like EC2 instances, S3 buckets, etc.

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0abcd1234abcd5678"
  instance_type = "t2.micro"
}
```

---

### 📤 4. `output` Block

Used to **display values** after Terraform execution (e.g., instance IP, URL).

```hcl
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

---

### 📥 5. `variable` Block

Defines **input variables** so values can be passed dynamically (e.g., from CLI or tfvars file).

```hcl
variable "instance_type" {
  description = "Type of EC2 instance"
  type        = string
  default     = "t2.micro"
}
```

---

### 🪝 6. `null_resource` Block

Used to **run scripts or triggers** that don’t involve real infrastructure (commonly used for provisioners).

```hcl
resource "null_resource" "example" {
  provisioner "local-exec" {
    command = "echo Hello, Terraform"
  }
}
```

---

### 🔍 7. `data` Block

Used to **fetch data** from existing resources (read-only). For example, get latest AMI ID.

```hcl
data "aws_ami" "latest_amazon_linux" {
  most_recent = true
  owners      = ["amazon"]

  filter {
    name   = "name"
    values = ["amzn2-ami-hvm-*-x86_64-gp2"]
  }
}
```
# Variable Types
---

### ✅ 1. `string`

A plain text value.

```hcl
variable "instance_type" {
  type    = string
  default = "t2.micro"
}
```

---

### ✅ 2. `number`

A numeric value (integer or float).

```hcl
variable "instance_count" {
  type    = number
  default = 2
}
```

---

### ✅ 3. `bool`

True or false value.

```hcl
variable "create_bucket" {
  type    = bool
  default = true
}
```

---

### ✅ 4. `list` (or `list(string)`, `list(number)` etc.)

An ordered sequence of values (like an array).

```hcl
variable "availability_zones" {
  type    = list(string)
  default = ["us-east-1a", "us-east-1b"]
}
```

---

### ✅ 5. `map` (or `map(string)`)

A set of key-value pairs (like a dictionary in Python).

```hcl
variable "tags" {
  type = map(string)
  default = {
    Name        = "MyServer"
    Environment = "dev"
  }
}
```

---

### ✅ 6. `set`

Like a list but ensures **unique values** (unordered).

```hcl
variable "subnet_ids" {
  type    = set(string)
  default = ["subnet-123", "subnet-456"]
}
```

---

### ✅ 7. `object`

Used to define a structured group of attributes.

```hcl
variable "instance_config" {
  type = object({
    instance_type = string
    ami_id        = string
    volume_size   = number
  })

  default = {
    instance_type = "t3.micro"
    ami_id        = "ami-12345678"
    volume_size   = 8
  }
}
```

---

### ✅ 8. `tuple`

Like a list, but each element can have **different types and positions are fixed**.

```hcl
variable "my_tuple" {
  type    = tuple([string, number, bool])
  default = ["web-server", 2, true]
}
```
---

## 🔐 What is a Sensitive Variable or Output?

Sometimes you deal with secrets like passwords, API keys, or private keys. You don’t want them to be printed in logs, CLI output, or your Terraform plan.
To **hide them**, Terraform provides the `sensitive = true` setting.

---

### ✅ Sensitive **Variable**

```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```

➡️ Even if you use `terraform plan` or `apply`, it will hide the password like this:

```
+ db_password = (sensitive value)
```

---

### ✅ Sensitive **Output**

```hcl
output "database_password" {
  value     = var.db_password
  sensitive = true
}
```

➡️ Even after the apply is successful, this will **not show the actual value** in the output.

```
Outputs:

database_password = (sensitive value)
```

---

### 📌 Notes:

* Sensitive variables can still be seen if someone opens the state file (`terraform.tfstate`). For full security, **encrypt the state file** using backends like S3 with KMS.
* Marking a variable or output as sensitive only **hides it from Terraform CLI output**, not from logs if you’re writing values using provisioners or scripts.

---

# Terraform Commands

---

### 📦 `terraform init`

**Initializes** your working directory with Terraform configuration.
It downloads the required **providers** and sets up the **backend** if configured.

```bash
terraform init
```

---

### 🧹 `terraform fmt`

**Formats** your `.tf` files in a **standard style** so it's clean and readable.

```bash
terraform fmt
```

---

### ✅ `terraform validate`

**Checks** whether your Terraform files are **syntactically correct** and all blocks are properly written.

```bash
terraform validate
```

---

### 🔍 `terraform plan`

**Previews** the changes Terraform will make (Create, Update, Delete) without actually applying them.
Helps you **review the actions** before execution.

```bash
terraform plan
```

---

### 🚀 `terraform apply`

**Applies the changes** to your infrastructure. It creates or updates resources as shown in the plan.

```bash
terraform apply
```

➡️ You can use `-auto-approve` to skip the prompt:

```bash
terraform apply -auto-approve
```

---

### 💥 `terraform destroy`

**Destroys** all infrastructure managed by Terraform.
Used when you want to tear down everything.

```bash
terraform destroy
```

➡️ You can also skip confirmation with:

```bash
terraform destroy -auto-approve
```

---

### 📄 `terraform show`

**Displays** the current state or plan in a readable format.
You can check what Terraform has created and the values stored.

```bash
terraform show
```

---

