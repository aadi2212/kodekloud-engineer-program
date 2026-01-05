# 🚀 Create EC2 Instance Using Terraform

## 📌 Task Overview
The **Nautilus DevOps team** is migrating parts of their infrastructure to **AWS Cloud** using an incremental approach.  
As part of this phase, an **EC2 instance** is provisioned using **Terraform**, including automatic SSH key generation and attachment of the default security group.

---

## 🎯 Requirements
- **EC2 Name Tag:** `datacenter-ec2`
- **AMI:** `ami-0c101f26f147fa7fd` (Amazon Linux)
- **Instance Type:** `t2.micro`
- **SSH Key Pair:** `datacenter-kp` (RSA)
- **Security Group:** Default AWS security group
- **Terraform Directory:** `/home/bob/terraform`
- **Terraform File:** `main.tf` (single file only)

---

## 📂 Project Structure
/home/bob/terraform
├── main.tf


---

## 🧾 Terraform Configuration (`main.tf`)

```hcl
resource "tls_private_key" "my_key" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "aws_key_pair" "deployer" {
  key_name   = "datacenter-kp"
  public_key = tls_private_key.my_key.public_key_openssh
}

data "aws_security_group" "default_sg" {
  name = "default"
}

resource "aws_instance" "web" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"

  key_name = aws_key_pair.deployer.key_name

  vpc_security_group_ids = [data.aws_security_group.default_sg.id]

  tags = {
    Name = "datacenter-ec2"
  }
}

⚙️ Execution Steps
1️⃣ Verify Working Directory
pwd


Expected output:

/home/bob/terraform

2️⃣ Initialize Terraform
terraform init

3️⃣ Review the Execution Plan
terraform plan

4️⃣ Apply the Configuration
terraform apply


Type yes when prompted.

✅ Verification
Check Terraform State
terraform state list


Expected output:

data.aws_security_group.default_sg
aws_instance.web
aws_key_pair.deployer
tls_private_key.my_key

📌 Outcome

✔ Amazon Linux EC2 instance created
✔ RSA SSH key pair generated automatically
✔ Default security group attached
✔ Instance tagged as datacenter-ec2
✔ Infrastructure fully managed using Terraform

🧠 Key Concepts Used

tls_private_key for SSH key generation

aws_key_pair for AWS key management

data blocks to fetch existing AWS resources

Infrastructure as Code (IaC) using Terraform

📎 Notes

No additional .tf files were created

Default AWS security group was reused

Suitable for DevOps labs and interview preparation


