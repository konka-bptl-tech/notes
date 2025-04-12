## 🧠 **Ansible 10-Day Learning Plan (From Zero to Hero)**

---

### ✅ **📅 Day 1: Why Ansible? – The Foundation**
- ❓ Problems with manual configuration
- 🧩 What is Configuration Management
- 🔧 What is Ansible?
- ✨ Ansible vs other tools (Puppet, Chef, Terraform)
- ⚙️ Agentless Architecture (SSH based)
- 🔁 Push-based vs Pull-based

---

### ✅ **📅 Day 2: Setup and First Run**
- 💻 Installing Ansible (on Linux/macOS/WSL)
- 📄 Ansible inventory (`/etc/ansible/hosts`)
- 🔐 SSH connectivity to remote nodes
- 🧪 First ad-hoc commands using `ansible`
- 🔁 Modules: `ping`, `copy`, `command`, `shell`, `yum/apt`, `file`

---

### ✅ **📅 Day 3: Playbooks Basics**
- 📘 What is a playbook?
- ✅ YAML syntax
- 🧱 Playbook structure: `hosts`, `tasks`, `become`, `vars`
- 🧪 Hands-on writing your first playbook

---

### ✅ **📅 Day 4: Variables & Facts**
- 💡 Defining variables in:
  - Playbooks
  - Inventory
  - Group/host vars
  - `vars_files`
- 📋 `ansible_facts` (`setup` module)
- ⚙️ `gather_facts: false` for performance

---

### ✅ **📅 Day 5: Conditionals, Loops, and Handlers**
- 🔄 `when` conditions (based on facts, variables)
- 🔁 `with_items`, `loop`, `with_dict`, `with_nested`
- 🚨 Handlers & `notify`
- 🪓 `block`, `rescue`, `always` blocks

---

### ✅ **📅 Day 6: Roles**
- 📂 Role directory structure (`roles/`)
- 🎭 Creating reusable roles
- 📥 Importing community roles (Ansible Galaxy)
- 🔁 Role dependencies

---

### ✅ **📅 Day 7: Templates and Jinja2**
- 🧾 `templates/` directory
- ✨ Jinja2 syntax (`{{ variable }}`, loops, ifs)
- 📄 Using `template:` module
- 🧪 Deploy dynamic config files (like `nginx.conf`, `httpd.conf`)

---

### ✅ **📅 Day 8: Ansible Vault**
- 🔒 Encrypt secrets (`ansible-vault encrypt`)
- 🔐 Commands: `edit`, `view`, `decrypt`
- 🔐 Vault with playbooks (`--ask-vault-pass`)
- 🔁 Best practices for secret handling

---

### ✅ **📅 Day 9: Advanced Inventory and Tags**
- 📁 Inventory formats: INI, YAML, dynamic
- 🏷️ Using tags for selective task execution (`--tags`, `--skip-tags`)
- ⚙️ Grouping hosts, using patterns
- 🌐 Dynamic inventories (AWS EC2, GCP)

---

### ✅ **📅 Day 10: Real-World Stuff**
- 🧪 `check` mode (`--check`)
- 🧩 `ansible-lint`, idempotency checks
- ✅ Testing with `molecule` (optional)
- 🛠️ Troubleshooting with `-vvv`
- 📦 CI/CD integration
- 💡 Best practices for directory structure

---

## 🧰 BONUS Topics (If time allows):
- 🧠 Custom modules
- 📦 Collections
- ☁️ Ansible with AWS, Azure, GCP modules
- 🔁 `delegate_to`, `serial`, `run_once`
- 🔃 Pull-based automation using Ansible Pull

---
# Terraform
---
## ✅ Your Current Plan with Suggested Enhancements

### **📅 Day 1: Foundation**
- ✔️ Problems with manual infra
- ✔️ What is IaC
- ✔️ What is Terraform  
✅ *Perfect intro day.*

---

### **📅 Day 2: Basic Blocks & CLI**
- ✔️ Blocks: `resource`, `variable`, `output`, `locals`, `data`, `null_resource`, `terraform`
- ✔️ Terraform commands: `init`, `plan`, `apply`, `destroy`, `fmt`, `validate`  
🔁 **Add:** `terraform.tfvars`, `*.auto.tfvars` → helps with variable practice

---

### **📅 Day 3: Variables Deep Dive**
- ✔️ Variable types: `string`, `list`, `map`, `object`, `tuple`
- ✔️ Precedence: CLI > env vars > tfvars > defaults  
🔁 **Optional Add:** `sensitive`, `nullable`, and `validation` blocks

---

### **📅 Day 4: Meta Arguments & Connection**
- ✔️ `count`, `for_each`, `provider`, `depends_on`, `lifecycle`, `provisioner`, `connection`  
✅ *All essential meta arguments covered.*

---

### **📅 Day 5: State Management**
- ✔️ Local & Remote State
- ✔️ State Locking (e.g., via S3 + DynamoDB)
🔁 **Add:** `terraform state` subcommands: `list`, `show`, `mv`, `rm`

---

### **📅 Day 6: Workspaces**
- ✔️ Named workspaces
- ✔️ Differences between `default`, `dev`, `prod`, etc.  
🔁 **Add:** Why workspaces are **not** always ideal for environments

---

### **📅 Day 7: Built-in Functions**
- ✔️ Functions: `merge`, `split`, `join`, `length`, `lookup`, `element`, etc.
🔁 **Add:** `flatten`, `concat`, `toset`, `tomap`, `zipmap`, `file`, `templatefile` for real muscle

---

### **📅 Day 8: Modules**
- ✔️ Creating modules
- ✔️ Reusing modules
- ✔️ Module structure (inputs/outputs)
🔁 **Add:** `source = "../"` vs. Git modules  
🔁 **Add:** `terraform get`, `init -upgrade`

---

### **📅 Day 9: Lifecycle Commands**
- ✔️ `terraform import`
- ✔️ `taint`, `untaint`, `replace`  
🔁 **Add:** Real-world `import` scenarios (e.g., existing S3, EC2)

---

### **📅 Day 10: Code Quality & Scanning**
- ✔️ `tfsec` or `tfscan` (good for security scanning)  
🔁 **Also add:**  
  - `checkov` (another popular IaC scanner)  
  - `terraform fmt` & `terraform validate` as part of CI/CD  
  - `.terraform.lock.hcl` → lock file importance

---

## 🏁 Optional Bonus Topics (If Time Permits)

| Topic | Why? |
|-------|------|
| `terraform graph` | Visualize dependency graphs |
| `backend` block deep dive | With config-driven backend setup |
| Terraform Cloud/Enterprise | For team collab |
| CI/CD with Terraform | Using GitHub Actions or Jenkins |
| Handling secrets | With Vault or SSM Parameter Store |
| Error troubleshooting tips | Real-world debugging (timeouts, dependency cycles) |
---