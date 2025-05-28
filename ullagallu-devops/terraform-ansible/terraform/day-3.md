## 🔁 Overview

### ✅ `count`

* Used to **create multiple instances** of a resource using a simple **integer value**.
* Best when you want **N identical copies** of something.
* Indexing is done using `count.index`.

### ✅ `for_each`

* Used to **create multiple instances** of a resource using a **map or a set of strings**.
* Allows **named instances** (not just numbered) and gives better control and clarity.
* Indexing is done using the **key** of the map or set.

---

## 📌 Key Difference

| Feature              | `count`                         | `for_each`                      |
| -------------------- | ------------------------------- | ------------------------------- |
| Input Type           | Integer                         | Map or Set                      |
| Index Access         | `count.index` (number)          | `each.key` and `each.value`     |
| Resource Uniqueness  | Identical or indexed via number | Named instances based on keys   |
| Deletion Sensitivity | More sensitive to order changes | Less sensitive to order changes |

---

## 📘 Examples

### Example 1: `count`

```hcl
variable "instance_count" {
  default = 3
}

resource "aws_instance" "web" {
  count         = var.instance_count
  ami           = "ami-123456"
  instance_type = "t2.micro"
  tags = {
    Name = "web-${count.index}"
  }
}
```

🔹 This creates 3 instances: `web[0]`, `web[1]`, `web[2]`.

---

### Example 2: `for_each` with a map

```hcl
variable "users" {
  default = {
    alice = "admin"
    bob   = "developer"
  }
}

resource "aws_iam_user" "user" {
  for_each = var.users

  name = each.key
  tags = {
    role = each.value
  }
}
```

🔹 This creates 2 IAM users:

* One with name `alice` and role `admin`
* One with name `bob` and role `developer`

You can access:

* `aws_iam_user.user["alice"]`
* `aws_iam_user.user["bob"]`

---

## 🛠️ When to Use Which?

| Scenario                                                   | Use        |
| ---------------------------------------------------------- | ---------- |
| You just need N identical resources                        | `count`    |
| Your resources are based on unique keys or names           | `for_each` |
| You need better control over the identity of each instance | `for_each` |
| You're looping over a simple integer range                 | `count`    |
| You're looping over a map or set                           | `for_each` |

---
## 🚨 Pitfall Example: `count` Deletion Issue
Suppose you remove the **second item** from a list when using `count`. Terraform may **recreate all resources after that index** because `count.index` is positional.
With `for_each`, the **key stays stable**, so changes are safer and more predictable.
---
Great! Let's walk through an example that **starts with `count`** and then shows how to **convert to `for_each`** for better stability and flexibility.

---

## 📘 Scenario: Creating AWS IAM Users

### 🔴 Initial version using `count`

```hcl
variable "usernames" {
  default = ["alice", "bob", "charlie"]
}

resource "aws_iam_user" "user" {
  count = length(var.usernames)

  name = var.usernames[count.index]
}
```

🔹 Terraform will create:

* `aws_iam_user.user[0]` → `alice`
* `aws_iam_user.user[1]` → `bob`
* `aws_iam_user.user[2]` → `charlie`

---

## ⚠️ Problem

If you remove `"bob"` from the list:

```hcl
default = ["alice", "charlie"]
```

Terraform will:

* Recreate `charlie` at index 1 (instead of 2),
* Destroy old `charlie` and make a new one — **unnecessary disruption**.

---

## ✅ Improved version using `for_each`

Instead, switch to a set or map:

```hcl
variable "usernames" {
  type    = set(string)
  default = ["alice", "bob", "charlie"]
}

resource "aws_iam_user" "user" {
  for_each = var.usernames

  name = each.key
}
```

🔹 This creates IAM users named:

* `alice`, `bob`, `charlie`

Now, if you remove `"bob"` from the list, only `bob` is destroyed. `charlie` remains untouched because the **key is preserved**.

---

## 🛠️ How to Convert from `count` to `for_each`

1. **Switch the variable type** to a `set` or `map`:

   ```hcl
   variable "usernames" {
     type = set(string)
     default = ["alice", "bob", "charlie"]
   }
   ```

2. **Replace `count` with `for_each`**:

   ```hcl
   resource "aws_iam_user" "user" {
     for_each = var.usernames
     name     = each.key
   }
   ```

3. **Access individual resources** by name instead of index:

   ```hcl
   aws_iam_user.user["alice"]
   ```

---



3 subnets


count.index

each.key each.value
 az1=10.0.0.0/24  az3=10.0.2.0/24