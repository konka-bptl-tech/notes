### ✅ All Terraform Variable Types – With Examples
---
### 1. **string**
* A single line of text.
```hcl
variable "env" {
  type = string
}
```
```hcl
env = "prod"
```
---
### 2. **number**
* A number value (integer or float).
```hcl
variable "replicas" {
  type = number
}
```
```hcl
replicas = 3
```
---
### 3. **bool**
* Boolean: `true` or `false`.
```hcl
variable "enable_logging" {
  type = bool
}
```
```hcl
enable_logging = false
```
---
### 4. **list(string)**
* Ordered list of values.
```hcl
variable "zones" {
  type = list(string)
}
```
```hcl
zones = ["us-east-1a", "us-east-1b"]
```
---
### 5. **set(string)**
* Similar to list, but unordered and unique values only.
```hcl
variable "tags" {
  type = set(string)
}
```
```hcl
tags = ["web", "dev", "web"] # 'web' will be stored once only
```
---
### 6. **map(string)**
* Key-value pairs with string values.
```hcl
variable "ami_map" {
  type = map(string)
}
```
```hcl
ami_map = {
  "us-east-1" = "ami-123"
  "us-west-1" = "ami-456"
}
```
---
### 7. **map(any)**
* Key-value pairs with values of any type.
```hcl
variable "config" {
  type = map(any)
}
```
```hcl
config = {
  retries = 3
  enable  = true
  name    = "app"
}
```
---
### 8. **object**
* Custom structure with named attributes.
```hcl
variable "db" {
  type = object({
    name     = string
    port     = number
    enabled  = bool
  })
}
```
```hcl
db = {
  name    = "mydb"
  port    = 5432
  enabled = true
}
```
---
### 9. **tuple**
* A fixed-length list with different types.
```hcl
variable "details" {
  type = tuple([string, number, bool])
}
```
```hcl
details = ["app", 2, true]
```
---
### 10. **any**
* Accepts any type of value.
```hcl
variable "dynamic_value" {
  type = any
}
```
```hcl
dynamic_value = "dev"
# OR
dynamic_value = 123
# OR
dynamic_value = [1, 2, 3]
```
---
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

## ✅ Terraform Conditional Expression

### Format:

```hcl
condition ? true_result : false_result
```

* Similar to ternary operator in other languages.
* Returns `true_result` if condition is true, otherwise returns `false_result`.

---

## 🔹 Examples:

---

### 1. **Set environment name**

```hcl
locals {
  env_name = var.is_prod ? "production" : "development"
}
```
---
### 2. **Choose instance type based on environment**
```hcl
variable "is_prod" {
  type = bool
  default = false
}
locals {
  instance_type = var.is_prod ? "t3.large" : "t3.micro"
}
```
---
### 3. **Enable logging only in production**
```hcl
resource "aws_s3_bucket" "logs" {
  bucket = "my-log-bucket"
  acl    = var.is_prod ? "private" : "public-read"
}
```
---
### 4. **Assign tags conditionally**
```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"

  tags = var.is_prod ? {
    "Name" = "Production-Server"
    "Env"  = "prod"
  } : {
    "Name" = "Dev-Server"
    "Env"  = "dev"
  }
}
```
---
### 5. **Count based on condition**
```hcl
resource "aws_instance" "optional" {
  count         = var.deploy_instance ? 1 : 0
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```
---
### 6. **Multiple conditions (nested)**
```hcl
locals {
  size = var.env == "prod" ? "large" :
         var.env == "staging" ? "medium" : "small"
}
```
---
Here's a **clean and simple explanation** of **why we use Terraform functions** and how each of the listed ones works — with **easy examples**👇

---

## ✅ Why We Use Functions in Terraform?

Terraform functions help to:

* **Manipulate data** (like strings, maps, lists).
* **Write dynamic logic** (like lookups, conditional values).
* **Avoid hardcoding** and keep configuration **DRY and reusable**.

---

## 🔹 Common Terraform Functions with Examples

---

### 1. **`length()`**

👉 Returns the number of items in a list, map, or length of a string.

```hcl
length(["a", "b", "c"])      # 3
length("hello")              # 5
```

---

### 2. **`merge()`**

👉 Combines two or more maps into one.

```hcl
merge({a = 1}, {b = 2})      # {a = 1, b = 2}
```

---

### 3. **`split()`**

👉 Splits a string into a list based on a delimiter.

```hcl
split(",", "apple,banana,mango")  
# ["apple", "banana", "mango"]
```

---

### 4. **`join()`**

👉 Joins a list into a single string with a separator.

```hcl
join("-", ["a", "b", "c"])   # "a-b-c"
```

---

### 5. **`lookup()`**

👉 Returns value for a key from a map; you can also set a default.

```hcl
lookup({env = "dev", region = "us-west"}, "env", "default")  
# "dev"
```

---

### 6. **`try()`**

👉 Returns the first value that doesn’t cause an error (used for fallback logic).

```hcl
try(var.name, "default")    
# If var.name is not defined, returns "default"
```

---

### 7. **`substr()`**

👉 Extracts part of a string.

```hcl
substr("terraform", 0, 4)   # "terr"
```

---

### 8. **`element()`**

👉 Picks a value from a list by index (can loop if index is greater than list size).

```hcl
element(["a", "b", "c"], 1)  # "b"
element(["a", "b", "c"], 4)  # "b" (loops)
```
---

