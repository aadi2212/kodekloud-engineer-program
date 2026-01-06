# 🔐 Create IAM Policy Using Terraform

## 📌 Task Overview
When setting up infrastructure on the **AWS Cloud**, **Identity and Access Management (IAM)** is one of the most critical services.  
In this task, an **IAM policy** is created using **Terraform** to provide **read-only access** to the **Amazon EC2 console**, allowing users to view:
- EC2 Instances  
- Amazon Machine Images (AMIs)  
- Snapshots  

---

## 🎯 Requirements
- **IAM Policy Name:** `iampolicy_mariyam`
- **AWS Region:** `us-east-1`
- **Access Level:** Read-only EC2 access
- **Terraform Directory:** `/home/bob/terraform`
- **Terraform File:** `main.tf` (single file only)

---

## 📂 Project Structure
/home/bob/terraform
├── main.tf


---

## 🧾 Terraform Configuration (`main.tf`)

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_policy" "policy_mariyam" {
  name        = "iampolicy_mariyam"
  description = "Read-only access to EC2 console (instances, AMIs and snapshots)"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:Describe*"
        ]
        Resource = "*"
      }
    ]
  })
}

⚙️ Execution Steps
1️⃣ Verify Working Directory
pwd


Expected output:

/home/bob/terraform

2️⃣ Initialize Terraform
terraform init

3️⃣ Preview the Changes
terraform plan

4️⃣ Apply the Configuration
terraform apply


Type yes when prompted.

✅ Verification
Check Terraform State
terraform state list


Expected output:

aws_iam_policy.policy_mariyam

📌 Outcome

✔ IAM policy iampolicy_mariyam successfully created
✔ Read-only permissions applied to EC2 resources
✔ Policy managed entirely using Terraform (IaC)
✔ Ready to be attached to users, groups, or roles

🧠 Key Concepts Used

AWS IAM Policies

jsonencode() for IAM policy documents

Terraform Resource Management

Infrastructure as Code (IaC)

AWS Security Best Practices

📎 Notes

Policy allows only ec2:Describe* actions (read-only)

IAM policy JSON keys are case-sensitive

No additional .tf files were created



