# 🚀 Launch EC2 in Private VPC Subnet Using Terraform

## 📘 Overview
The Nautilus DevOps team is expanding its AWS infrastructure by provisioning a **private Virtual Private Cloud (VPC)** and launching an **EC2 instance inside a private subnet** using Terraform.

This setup ensures:
- Complete isolation from external networks
- Secure internal-only communication within the VPC
- Infrastructure as Code (IaC) best practices

---

## 🎯 Requirements
- Create a **private VPC**
- Create a **private subnet** (no public IP assignment)
- Launch an **EC2 instance** inside the private subnet
- Restrict EC2 access to **VPC CIDR only**
- Use Terraform with:
  - `main.tf`
  - `variables.tf`
  - `outputs.tf`

---

## 📂 Project Structure
```text
terraform/
├── main.tf
├── variables.tf
└── outputs.tf
📌 Configuration Details
Resource	Name	CIDR / Type
VPC	nautilus-priv-vpc	10.0.0.0/16
Subnet	nautilus-priv-subnet	10.0.1.0/24
EC2	nautilus-priv-ec2	t2.micro
🔧 variables.tf
variable "KKE_VPC_CIDR" {
  description = "CIDR block for the private VPC"
  type        = string
  default     = "10.0.0.0/16"
}

variable "KKE_SUBNET_CIDR" {
  description = "CIDR block for the private subnet"
  type        = string
  default     = "10.0.1.0/24"
}

🏗️ main.tf
provider "aws" {
  region = "us-east-1"
}

# VPC
resource "aws_vpc" "nautilus_priv_vpc" {
  cidr_block           = var.KKE_VPC_CIDR
  enable_dns_support   = true
  enable_dns_hostnames = true

  tags = {
    Name = "nautilus-priv-vpc"
  }
}

# Private Subnet
resource "aws_subnet" "nautilus_priv_subnet" {
  vpc_id                  = aws_vpc.nautilus_priv_vpc.id
  cidr_block              = var.KKE_SUBNET_CIDR
  map_public_ip_on_launch = false

  tags = {
    Name = "nautilus-priv-subnet"
  }
}

# Security Group (VPC-only access)
resource "aws_security_group" "nautilus_priv_sg" {
  name        = "nautilus-priv-sg"
  description = "Allow traffic only within VPC"
  vpc_id      = aws_vpc.nautilus_priv_vpc.id

  ingress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = [var.KKE_VPC_CIDR]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# EC2 Instance
resource "aws_instance" "nautilus_priv_ec2" {
  ami                    = "ami-0c02fb55956c7d316"
  instance_type          = "t2.micro"
  subnet_id              = aws_subnet.nautilus_priv_subnet.id
  vpc_security_group_ids = [aws_security_group.nautilus_priv_sg.id]

  tags = {
    Name = "nautilus-priv-ec2"
  }
}

📤 outputs.tf
output "KKE_vpc_name" {
  value = aws_vpc.nautilus_priv_vpc.tags["Name"]
}

output "KKE_subnet_name" {
  value = aws_subnet.nautilus_priv_subnet.tags["Name"]
}

output "KKE_ec2_private" {
  value = aws_instance.nautilus_priv_ec2.tags["Name"]
}

▶️ Usage
1️⃣ Initialize Terraform
terraform init

2️⃣ Preview Changes
terraform plan

3️⃣ Apply Configuration
terraform apply


Type yes when prompted.

✅ Verification
Ensure No Configuration Drift
terraform plan


Expected Output:

No changes. Your infrastructure matches the configuration.

Check Managed Resources
terraform state list

🏁 Result

✔ Private VPC created
✔ Private subnet without public IPs
✔ EC2 instance isolated inside VPC
✔ Security group restricted to internal traffic only
✔ Terraform best practices followed
