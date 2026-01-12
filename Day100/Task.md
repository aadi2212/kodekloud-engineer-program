# Create and Configure CloudWatch Alarm Using Terraform

## 📌 Overview
This project demonstrates how to use **Terraform** to launch an **EC2 instance** and configure an **Amazon CloudWatch alarm** to monitor CPU utilization.  
The alarm triggers when CPU usage exceeds **90% for a consecutive 5-minute period** and sends a notification via an existing **SNS topic**.

---

## 🎯 Requirements
- Terraform installed
- AWS credentials configured
- Existing SNS topic: **datacenter-sns-topic**
- Working directory: `/home/bob/terraform`

---

## 🏗️ Infrastructure Components
- **EC2 Instance**
  - Name: `datacenter-ec2`
  - AMI: `ami-0c02fb55956c7d316`
  - Instance Type: `t2.micro`

- **CloudWatch Alarm**
  - Name: `datacenter-alarm`
  - Metric: `CPUUtilization`
  - Statistic: `Average`
  - Threshold: `>= 90%`
  - Period: `5 minutes`
  - Evaluation Periods: `1`

- **SNS Topic**
  - Name: `datacenter-sns-topic`

---

## 📂 Project Structure
.
├── main.tf
└── outputs.tf


---

## 🛠️ main.tf

```hcl
resource "aws_sns_topic" "sns_topic" {
  name = "datacenter-sns-topic"
}

# Launch an EC2 instance
resource "aws_instance" "nautilus_node" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"

  tags = {
    Name = "datacenter-ec2"
  }
}

# Create a CloudWatch alarm for CPU utilization
resource "aws_cloudwatch_metric_alarm" "cpu_alert" {
  alarm_name          = "datacenter-alarm"
  comparison_operator = "GreaterThanOrEqualToThreshold"
  evaluation_periods  = 1
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 90
  alarm_description   = "Alarm when CPU exceeds 90%"

  alarm_actions = [
    aws_sns_topic.sns_topic.arn
  ]

  dimensions = {
    InstanceId = aws_instance.nautilus_node.id
  }
}

📤 outputs.tf
output "KKE_instance_name" {
  value = aws_instance.nautilus_node.tags.Name
}

output "KKE_alarm_name" {
  value = aws_cloudwatch_metric_alarm.cpu_alert.alarm_name
}

🚀 Deployment Steps
1️⃣ Verify working directory
pwd
/home/bob/terraform

2️⃣ Initialize Terraform
terraform init

3️⃣ Validate configuration
terraform validate

4️⃣ Preview changes
terraform plan


✅ Ensure the output shows:

No changes. Your infrastructure matches the configuration.

5️⃣ Apply configuration
terraform apply


Type yes when prompted.

✅ Verification

List managed resources:

terraform state list


View deployed infrastructure:

terraform show

📌 Outputs

After successful deployment, Terraform outputs:

KKE_instance_name → EC2 instance name

KKE_alarm_name → CloudWatch alarm name

🧠 Key Learnings

Creating EC2 resources using Terraform

Monitoring EC2 CPU utilization with CloudWatch

Using SNS for alarm notifications

Correct usage of dimensions in CloudWatch alarms

Managing and verifying Terraform state

✅ Status

✔ EC2 instance created
✔ CloudWatch alarm configured
✔ SNS notifications integrated
✔ Terraform state verified
