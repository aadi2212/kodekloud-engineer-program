# Set Up Jenkins Server

## 📝 Task Overview
Install and configure Jenkins on the designated Jenkins host using **yum**, then create the required admin user via the Jenkins UI.

---

## 🔧 Prerequisites

- Access to Jenkins server through the jump host:


Username: root
Password: S3curePass


- Internet access on the Jenkins server
- Tools required: `wget`, `rpm`, `yum`

---

## 🚀 Step 1: Connect to the Jenkins Server

```bash
ssh root@<jenkins_server_ip>
# Password: S3curePass


📦 Step 2: Add Jenkins Repository
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

This allows yum to fetch Jenkins packages.


📚 Step 3: Install Dependencies (Java)
yum install -y fontconfig java-21-openjdk

Jenkins requires Java 21 to run properly.


🏗️ Step 4: Install Jenkins
yum install -y jenkins

If GPG check fails:
yum install -y jenkins --nogpgcheck


▶️ Step 5: Start & Enable Jenkins Service
systemctl start jenkins
systemctl enable jenkins
systemctl status jenkins


If Jenkins fails to start due to timeout:
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --reload
journalctl -xeu jenkins


🌐 Step 6: Access Jenkins UI
1. Open browser:
http://<jenkins_server_ip>:8080


2. Retrieve initial admin password:
cat /var/lib/jenkins/secrets/initialAdminPassword

Enter this password in Jenkins Setup Wizard.


👤 Step 7: Create Admin User
Fill the fields as follows:

| Field     | Value                                                                               |
| --------- | ----------------------------------------------------------------------------------- |
| Username  | theadmin                                                                            |
| Password  | Adm!n321                                                                            |
| Full Name | Mark                                                                                |
| Email     | [mark@jenkins.stratos.xfusioncorp.com](mailto:mark@jenkins.stratos.xfusioncorp.com) |

Complete the setup wizard.


✔️ Step 8: Verify Jenkins Setup
systemctl start jenkins
systemctl enable jenkins
systemctl status jenkins


Ensure:
1. Jenkins service is active
2. Login works using the theadmin credentials


⚠️ Common Issues & Fixes
1️⃣ Timeout when starting Jenkins

1. Check system resources (RAM/CPU)
2. Ensure port 8080 is open

Check logs:
journalctl -xeu jenkins


2️⃣ Wrong Java version installed
Remove incorrect version:
yum remove java-11-openjdk


Reinstall correct version:
yum install -y java-21-openjdk


🎯 Final Outcome:
✔ Jenkins installed using yum

✔ Jenkins service running on port 8080

✔ Admin user theadmin created successfully

✔ Server ready for CI/CD pipeline configuration


