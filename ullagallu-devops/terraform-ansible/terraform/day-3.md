# ✅ Why we use `locals` in Terraform:

The `locals` block is used to **define local variables** within your Terraform configuration. These are **read-only** values that you want to reuse multiple times but **don’t need to pass from outside** (like input variables).

---

### 🔧 Key Benefits of Using `locals`:

1. **Avoid repetition**:
   Instead of repeating long expressions or values, define once and reuse.

2. **Improve readability**:
   Complex logic can be simplified using named local variables.

3. **Organize logic**:
   You can break down expressions into smaller parts for better maintenance.

---

### ✅ Example:

```hcl
locals {
  app_name     = "myapp"
  environment  = "dev"
  full_name    = "${local.app_name}-${local.environment}"
}

resource "aws_s3_bucket" "example" {
  bucket = local.full_name
  acl    = "private"
}
```

➡️ In this example:

* You avoid repeating `"myapp"` and `"dev"` everywhere.
* You use `local.full_name` to combine them once and reuse.

---

### 📌 Summary:

* `locals` = **internal variables for logic reuse**
* They are **not passed** from outside and are **not returned** as output.
* They are **helpful in writing clean, reusable, and DRY code**.

Here's a **simple explanation** of **Terraform provisioners** — especially the common ones: `file`, `local-exec`, and `remote-exec`.

---

# ✅ What are Provisioners in Terraform?

Provisioners are used to **run scripts or commands** on a resource **after it is created** (or destroyed).
They're useful for **bootstrapping**, like installing software or configuring a server.

---

## 🔧 Types of Provisioners

### 1. 📁 `file` Provisioner

Used to **copy files** from your local machine to the target server (like an EC2 instance).

```hcl
provisioner "file" {
  source      = "app.sh"
  destination = "/tmp/app.sh"
}
```

---

### 2. 💻 `local-exec` Provisioner

Runs a **command on your local machine** (where Terraform is running).

```hcl
provisioner "local-exec" {
  command = "echo ${self.public_ip} >> ip_list.txt"
}
```

📌 Useful for updating files, running scripts, or sending notifications **locally**.

---

### 3. 🌐 `remote-exec` Provisioner

Runs **commands on the remote server** (like an EC2 instance) via SSH.

```hcl
provisioner "remote-exec" {
  inline = [
    "sudo apt update",
    "sudo apt install nginx -y"
  ]
}
```

✅ You must also configure a `connection` block for SSH:

```hcl
connection {
  type     = "ssh"
  user     = "ec2-user"
  private_key = file("~/.ssh/id_rsa")
  host     = self.public_ip
}
```

---

## ⚠️ When to Use (and Avoid) Provisioners

* ✅ Good for **initial bootstrapping** or one-time config.
* ❌ Avoid for **day-to-day automation** — use tools like **Ansible** or **user\_data** instead.
* Terraform's **core idea is declarative**, so use provisioners **only when necessary**.

---