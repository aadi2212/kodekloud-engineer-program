# 🚀 Create VPC Using Terraform

## 📌 Task Overview

The **Nautilus DevOps Team** is planning a phased migration of their infrastructure to the **AWS Cloud**.  
Instead of executing a large, one-time migration, the team has divided the process into smaller, manageable tasks.  
This approach helps reduce risk, improves control, and ensures smooth cloud adoption.

As part of this migration, the task is to create an **AWS VPC** using **Terraform**.

---

## 🎯 Objective

Provision an AWS **Virtual Private Cloud (VPC)** using Terraform with the following requirements:

- **VPC Name:** `datacenter-vpc`
- **Region:** `us-east-1`
- **IPv4 CIDR Block:** `10.0.0.0/16`
- **Terraform Directory:** `/home/bob/terraform`
- **Terraform File:** `main.tf` (no additional `.tf` files allowed)

---

## 📘 What is Terraform?

**Terraform** is an **Infrastructure as Code (IaC)** tool that allows you to define and manage infrastructure using code instead of manual configurations.

### ✅ Benefits of Terraform
- Infrastructure versioning
- Reusable configurations
- Automation and consistency
- Team collaboration via Git

---

## 🌐 What is a VPC?

A **Virtual Private Cloud (VPC)** is a private and isolated network within a public cloud like AWS.

### 🏢 Simple Analogy
- AWS → Large business park  
- VPC → Your own private, secured office building  

Each VPC is completely isolated from others, even though they exist within the same cloud.

---

## 🛠️ Implementation Steps

---

### 🔹 Step 1: Verify Working Directory

Open the terminal and ensure you are in the correct directory:
```bash
pwd

Expected output:

/home/bob/terraform

🔹 Step 2: Create main.tf

Create and open the Terraform configuration file:

vi main.tf


Add the following content:

provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "datacenter-vpc"
  }
}

🔹 Step 3: Initialize Terraform

Initialize the Terraform project:

terraform init


⚠️ Issue Encountered:
Terraform initialization fails due to a duplicate AWS provider configuration.

🔹 Step 4: Fix Provider Conflict

A provider.tf file already exists containing the AWS provider block.

📌 Terraform automatically loads all .tf files in the directory, so duplicate provider blocks cause conflicts.

Edit main.tf and remove the provider block:

vi main.tf


Updated content:

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "datacenter-vpc"
  }
}

🔹 Step 5: Re-initialize Terraform
terraform init


✅ Initialization completes successfully.

🔹 Step 6: Apply Terraform Configuration

Create the VPC resource:

terraform apply


When prompted, type:

yes


✔️ VPC is successfully created in AWS.

🔹 Step 7: Verify Infrastructure State

Display the current Terraform-managed infrastructure:

terraform show


This command provides:

VPC ID

CIDR block

Tags

Region details

✅ Final Outcome

✔️ AWS VPC datacenter-vpc created

✔️ Region: us-east-1

✔️ Managed using Terraform

✔️ Infrastructure as Code best practices followed

✔️ Ready for further cloud expansion

🧠 Key Takeaways

Terraform loads all .tf files automatically

Duplicate provider blocks cause initialization errors

Always verify directory structure before execution

IaC enables safe, repeatable, and scalable infrastructure deployments


