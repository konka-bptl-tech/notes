1. Create RDS instance
   - Create Route53 Record
   - Connect and Load Schema
```bash
CREATE DATABASE IF NOT EXISTS crud_app;
USE crud_app;

CREATE TABLE entries (
  id INT AUTO_INCREMENT PRIMARY KEY,
  amount INT NOT NULL,
  description VARCHAR(255) NOT NULL
);
CREATE USER IF NOT EXISTS 'crud'@'%' IDENTIFIED BY 'CrudApp@1';
GRANT ALL ON crud_app.* TO 'crud'@'%';
FLUSH PRIVILEGES;
```

2. Luanch Elastic Cache Redis OSS
   - Create Route53 Record
   
3. Just Launch Ec2 instance install sudo dnf install redis6
   - Checking connection telnet <dns-name> <port> to comeout from telnet  press ctrl + ] then enter quit
   - Checking the connection redis6-cli -h <dns-name> -p <port> --tls --insecure

4. Prepare AMI wihtout service file
- Go to aws shell install packer
```bash
#!/bin/bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install packer
```
- create folder backend and create backend.sh

```bash
#!/bin/bash
# Variables
USERID=$(id -u) # User id
TIMESTAMP=$(date +%F-%H-%M-%S)
SCRIPT_NAME=$(basename "$0" | cut -d "." -f1)
LOG_FILE="/tmp/${TIMESTAMP}-${SCRIPT_NAME}.log"
REPO_URL="https://github.com/sivaramakrishna-konka/3-tier-vm-backend.git"
APP_DIR="/app"
# SERVICE_FILE="/etc/systemd/system/backend.service"
# CW_CONFIG_FILE="/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json"

# Colors for terminal output
R="\e[31m"   # Red
G="\e[32m"   # Green
Y="\e[33m"   # Yellow
N="\e[0m"    # Reset

echo "Script started executing at: $TIMESTAMP"
echo "Log file: $LOG_FILE"

# Function to log command success or failure
LOG() {
    local MESSAGE="$1"
    local STATUS="$2"
    if [ "$STATUS" -eq 0 ]; then
        echo -e "$MESSAGE ..... ${G}Success${N}" | tee -a "$LOG_FILE"
    else
        echo -e "$MESSAGE ..... ${R}Failed${N}" | tee -a "$LOG_FILE"
        exit 1
    fi
}

# Check if the script is run as root
if [ $USERID -ne 0 ]; then
    echo -e "${R}Please run this script with sudo privileges${N}" | tee -a "$LOG_FILE"
    exit 1
else
    echo -e "${G}Here we go to installation${N}" | tee -a "$LOG_FILE"
fi

dnf update -y &>>"$LOG_FILE"
LOG "Updating the packages" $?

curl -fsSL https://rpm.nodesource.com/setup_20.x | bash - &>>"$LOG_FILE"
LOG "Downloading Node.js setup script" $?

# Install required packages
dnf install git telnet nodejs amazon-cloudwatch-agent -y &>>"$LOG_FILE"
LOG "Installing git, telnet, Node.js 20, and CloudWatch Agent" $?

# Add 'expense' user if not exists
if ! id -u expense &>/dev/null; then
    useradd expense &>>"$LOG_FILE"
    LOG "Adding 'expense' user" $?
fi

# Clone backend repository if directory doesn't exist
if [ ! -d "$APP_DIR" ]; then
    mkdir "$APP_DIR" &>>"$LOG_FILE"
    LOG "Creating directory $APP_DIR" $?

    git clone "$REPO_URL" "$APP_DIR" &>>"$LOG_FILE"
    LOG "Cloning expense-backend repository to $APP_DIR" $?
else
    echo "Directory $APP_DIR already exists. Skipping cloning." | tee -a "$LOG_FILE"
fi

# Install dependencies for the backend
cd "$APP_DIR" && npm install &>>"$LOG_FILE"
LOG "Installing npm dependencies for the backend" $?
echo "Script execution completed successfully." | tee -a "$LOG_FILE"
```


- create backend.pkr.hcl
```hcl
  packer {
  required_plugins {
    amazon = {
      source  = "github.com/hashicorp/amazon"
      version = "~> 1"
    }
  }
}

source "amazon-ebs" "amz3_gp3" {
  ami_name      = "sivab-{{timestamp}}"
  instance_type = "t3.micro"
  region        = "us-east-1"
  
  source_ami_filter {
    filters = {
      name                = "al2023-ami-2023*"
      architecture        = "x86_64"
      root-device-type    = "ebs"
    }
    most_recent = true
    owners      = ["amazon"]
  }

  ssh_username  = "ec2-user"

  # Adding tags to the AMI
  tags = {
    Name        = "sivab-packer-image"
    Environment = "Development"
    Owner       = "Konka"
    CreatedBy   = "Packer"
    Monitor     = "true"
  }
}

build {
  name    = "sivab"
  sources = ["source.amazon-ebs.amz3_gp3"]

  provisioner "file" {
    source      = "backend.sh"
    destination = "/tmp/backend.sh"
  }

  provisioner "shell" {
    inline = [
      "chmod +x /tmp/backend.sh",
      "sudo /tmp/backend.sh"
    ]
  }
}
```

5. Create secrets in secrets manager and non sensitive data in parameter store
   - aws secrets manager --> store a new secret --> other type of secret[enter key value pairs] --> secretname --> review and enter
   - go to parameter store enter all non-sensitive data in secure string type
6. Create IAM policy to get secrets and paramter strore

```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "ssm:GetParameter",
          "ssm:GetParameters"
        ],
        "Resource": [
          "arn:aws:ssm:REGION:ACCOUNT_ID:parameter/crud/config/*" # Replace with your actual arn 
        ]
      },
      {
        "Effect": "Allow",
        "Action": [
          "secretsmanager:GetSecretValue"
        ],
        "Resource": [
          "arn:aws:secretsmanager:REGION:ACCOUNT_ID:secret:crud/db/credentials-??????" # Replace with actual arn
        ]
      }
    ]
  }
```

- create iam role backend crednetials

7. Create Luanch template with backend-ami choose key and in assign instance profile above create role and enter user data

```bash
#!/bin/bash
set -e

APP_DIR="/app"
SERVICE_FILE="/etc/systemd/system/backend.service"
SECRET_NAME="crud/db/credentials"

# Install necessary tools
dnf install -y awscli jq

# Fetch Secrets from Secrets Manager
SECRETS=$(aws secretsmanager get-secret-value --secret-id $SECRET_NAME --query SecretString --output text)
DB_USER=$(echo "$SECRETS" | jq -r .DB_USER)
DB_PASSWORD=$(echo "$SECRETS" | jq -r .DB_PASSWORD)

# Fetch Configuration from SSM Parameter Store
DB_HOST=$(aws ssm get-parameter --name "/crud/config/DB_HOST" --with-decryption --query "Parameter.Value" --output text)
DB_NAME=$(aws ssm get-parameter --name "/crud/config/DB_NAME" --with-decryption --query "Parameter.Value" --output text)
REDIS_HOST=$(aws ssm get-parameter --name "/crud/config/REDIS_HOST" --with-decryption --query "Parameter.Value" --output text)
REDIS_PORT=$(aws ssm get-parameter --name "/crud/config/REDIS_PORT" --with-decryption --query "Parameter.Value" --output text)

# Generate systemd service file
cat <<EOF > $SERVICE_FILE
[Unit]
Description=Backend Service

[Service]
User=expense
Environment="DB_HOST=${DB_HOST}"
Environment="DB_USER=${DB_USER}"
Environment="DB_PASSWORD=${DB_PASSWORD}"
Environment="DB_NAME=${DB_NAME}"
Environment="REDIS_HOST=${REDIS_HOST}"
Environment="REDIS_PORT=${REDIS_PORT}"
ExecStart=/usr/bin/node ${APP_DIR}/server.js
SyslogIdentifier=backend
Restart=always

[Install]
WantedBy=multi-user.target
EOF

# Start service
systemctl daemon-reload
systemctl enable backend
systemctl start backend
```

8. Luanch ASG with above template and also ALB

9. Create IAM role with below policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowReadAccessToNginxConf",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::your-bucket-name/path/to/nginx.conf"
    }
  ]
}
```

10. Create Luanch template with frontend-ami choose key and in assign instance profile above create role and enter user data
```bash
#!/bin/bash
aws s3 cp s3://vm-s3-nginx-conf/nginx.conf /etc/nginx/nginx.conf
systemctl restart nginx
```

11. Check the entries in redis

```markdown    
redis6-cli -h test-redis.konkas.tech -p 6379 --tls --insecure GET "all_entries" | jq .
```
12. create certificate frontend-vm.konkas.tech same as upload to Route53

13. create cloud front 
```markdown
create distribution --> choose origin frontend-vm.konkas.tech --> Alternative Name frontend-vm.konkas.tech --> certificate choose frontend-vm.konkas.tech
```


```markdown
Our developers make sure api response should be 15 ms latency application architectural response we enable elastic cache it ensure less latency some times it returns cache connnect we enable cloud front for edge locations
- most of the errors are coming by misconfiguration
```