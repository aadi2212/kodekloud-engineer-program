# Create Security Group Using Terraform (AWS)

## 📌 Overview

The **Nautilus DevOps Team** is migrating a portion of their infrastructure to the **AWS Cloud** using an incremental approach.  
Instead of performing a large-scale migration at once, the team breaks tasks into smaller, manageable units to ensure smoother execution, reduced risk, and better control.

In this task, **Terraform** is used to create an **AWS Security Group** in the **default VPC** with required inbound rules.

---

## 🎯 Task Requirements

Create an AWS Security Group using Terraform with the following specifications:

- **Security Group Name:** `devops-sg`
- **Description:** `Security group for Nautilus App Servers`
- **VPC:** Default VPC
- **Region:** `us-east-1`
- **Inbound Rules:**
  - HTTP → Port `80` → `0.0.0.0/0`
  - SSH → Port `22` → `0.0.0.0/0`
- **Terraform Directory:** `/home/bob/terraform`
- **Terraform File:** `main.tf` only

---

## 🔐 What is a Security Group?

An **AWS Security Group** acts as a virtual firewall that controls inbound and outbound traffic for EC2 instances.

Examples:
- Port **80 (HTTP)** allows web traffic
- Port **22 (SSH)** allows secure administrative access

---

## 📂 Step 1: Verify Working Directory

```bash
pwd

Expected output:

/home/bob/terraform

🧱 Step 2: Create main.tf

Create a file named main.tf inside /home/bob/terraform and add the following configuration:

provider "aws" {
  region = "us-east-1"
}

data "aws_vpc" "default_vpc" {
  default = true
}

resource "aws_security_group" "devops_sg" {
  name        = "devops-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default_vpc.id

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

⚙️ Step 3: Initialize Terraform

Initialize Terraform and download required provider plugins:

terraform init

🔍 Step 4: Preview Changes

Check what Terraform will create before applying changes:

terraform plan

🚀 Step 5: Apply Configuration

Create the security group in AWS:

terraform apply


Type yes when prompted.

📌 Step 6: Verify Terraform State

Verify that the resources are tracked by Terraform:

terraform state list


Expected output:

data.aws_vpc.default_vpc
aws_security_group.devops_sg

🔎 Step 7: Verify Using AWS CLI

Retrieve detailed information about the created security group:

aws ec2 describe-security-groups \
  --group-names devops-sg \
  --region us-east-1

✅ Outcome

✔ Security Group devops-sg successfully created

✔ Deployed in the default VPC

✔ HTTP and SSH access enabled

✔ Infrastructure managed using Terraform (IaC)

🧠 Key Learnings

Terraform enables Infrastructure as Code

Security Groups act as stateful firewalls

terraform plan helps avoid unintended changes

Modular and incremental cloud migration reduces risk

📎 Tools Used

Terraform

AWS EC2

AWS CLI
```bash
pwd
