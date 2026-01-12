# Secure DynamoDB Access Using Terraform (IAM Read-Only Policy)

## 📌 Overview
In this task, we use **Terraform** to provision a **secure DynamoDB table** and enforce **fine-grained access control** using AWS IAM.  
An IAM role is created and attached to a restricted IAM policy that grants **read-only access** (`GetItem`, `Scan`, `Query`) to the DynamoDB table.

This setup follows the **principle of least privilege** and ensures secure access from trusted AWS services only.

---

## 🧩 Prerequisites
- Terraform installed
- AWS credentials configured
- Working directory:  
  ```bash
  /home/bob/terraform
🪜 Step-by-Step Implementation
1️⃣ Verify Working Directory

Ensure you are in the correct Terraform directory.

pwd


Expected output:

/home/bob/terraform

2️⃣ Create variables.tf

Define the required input variables.

variable "KKE_TABLE_NAME" {}

variable "KKE_ROLE_NAME" {}

variable "KKE_POLICY_NAME" {}

3️⃣ Create terraform.tfvars

Provide actual values for the variables.

KKE_TABLE_NAME  = "devops-table"
KKE_ROLE_NAME   = "devops-role"
KKE_POLICY_NAME = "devops-readonly-policy"

4️⃣ Create main.tf

This file provisions the DynamoDB table, IAM role, IAM policy, and attaches the policy to the role.

############################################
# DynamoDB Table
############################################
resource "aws_dynamodb_table" "devops_table" {
  name         = var.KKE_TABLE_NAME
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "id"

  attribute {
    name = "id"
    type = "S"
  }
}

############################################
# IAM Role
############################################
resource "aws_iam_role" "devops_role" {
  name = var.KKE_ROLE_NAME

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })
}

############################################
# IAM Policy (Read-Only DynamoDB Access)
############################################
resource "aws_iam_policy" "devops_readonly_policy" {
  name = var.KKE_POLICY_NAME

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "dynamodb:GetItem",
          "dynamodb:Scan",
          "dynamodb:Query"
        ]
        Resource = aws_dynamodb_table.devops_table.arn
      }
    ]
  })
}

############################################
# Attach Policy to Role
############################################
resource "aws_iam_role_policy_attachment" "devops_attach" {
  role       = aws_iam_role.devops_role.name
  policy_arn = aws_iam_policy.devops_readonly_policy.arn
}

5️⃣ Create outputs.tf

Expose useful output values after deployment.

output "kke_dynamodb_table" {
  value = aws_dynamodb_table.devops_table.name
}

output "kke_iam_role_name" {
  value = aws_iam_role.devops_role.name
}

output "kke_iam_policy_name" {
  value = aws_iam_policy.devops_readonly_policy.name
}

🚀 Terraform Execution
Initialize Terraform
terraform init

Preview Infrastructure Changes
terraform plan

Apply the Configuration
terraform apply


Confirm with:

yes

🔍 Verification
List Terraform State Resources
terraform state list

Show Detailed Resource Information
terraform show

🧠 Key Concepts

IAM Trust Policy controls who can assume the role

IAM Permission Policy controls what actions are allowed

Policy access is restricted to a specific DynamoDB table ARN

Implements least-privilege security best practices

✅ Final Result

✔ DynamoDB table created

✔ IAM role created

✔ Read-only IAM policy enforced

✔ Policy attached correctly

✔ Terraform state matches configuration

📌 Notes

This setup is suitable for corporate / production-style DevOps tasks

Easily extendable for EKS, Lambda, or ECS by modifying the IAM trust policy
