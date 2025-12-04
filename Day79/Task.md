# Jenkins Deployment Job — Auto Deployment Setup

## 📌 Task Objective
Automate deployment of the **web application** from the Gitea repository to the Storage server’s shared directory (`/var/www/html`) using a Jenkins job. The job should auto-trigger on every push to the **master branch**. Apache (httpd) must serve on **port 8080**, and deployment must run with proper permissions.

---

## 1️⃣ Install & Configure Apache (httpd) on All App Servers

**Goal:** Serve content on port 8080.

**Commands (run on all 3 App Servers):**
```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

sudo su
yum install -y httpd
# Change Apache port from 80 to 8080
sed -i 's/Listen 80/Listen 8080/g' /etc/httpd/conf/httpd.conf
systemctl restart httpd


Notes:
Ensure port 8080 is reachable through the load balancer.
Refresh the browser; initially, you’ll see nothing because Apache was not running on 8080.
Repeat for all App Servers.


2️⃣ Install Required Jenkins Plugins & Create Job

Step 2.1 — Install Plugins
Manage Jenkins → Manage Plugins → Available

Install:
Git plugin

SSH plugin

SSH Credentials plugin

SSH Build Agents plugin

Restart Jenkins after installation.


Step 2.2 — Create Jenkins Job
Job Name: nautilus-app-deployment

Type: Freestyle Project

Source Code Management:
Git repository URL: http://git.stratos.XfusionCorp.com/sarah/web.git

Branch specifier: */master (default)

Credentials: None (public repo)

Build Trigger: Poll SCM → Schedule: * * * * * (every minute)

This ensures the job runs automatically whenever new changes are pushed to master.


3️⃣ Configure Passwordless SSH from Jenkins to Storage Server

Step 3.1 — Generate SSH Keys on Jenkins Server

ssh jenkins@jenkins
ssh-keygen -t rsa
cd ~/.ssh/
cat id_rsa.pub
# Copy the public key


Step 3.2 — Configure Storage Server

ssh natasha@ststor01   # password: Bl@kW
mkdir -p ~/.ssh
cat > ~/.ssh/authorized_keys   # paste Jenkins public key
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

# Test passwordless login
ssh natasha@ststor01


4️⃣ Configure Jenkins Job to Deploy Code

Step 4.1 — Add Build Step

Build → Execute Shell

scp -r * natasha@ststor01:/var/www/html


Step 4.2 — Handle Permission Issues
ssh natasha@ststor01
sudo chown -R natasha /var/www/html

Re-run Jenkins job → should succeed.


5️⃣ Update index.html and Trigger Deployment
Step 5.1 — SSH into Storage Server as sarah

ssh sarah@ststor01   # password: Bl@kW
cd ~/web


Step 5.2 — Update index.html
cat > index.html
# Paste the following:
Welcome to the xFusionCorp Industries
# Press Ctrl+D to save


Step 5.3 — Commit & Push Changes
git add index.html
git commit -am "Updated index.html"
git push origin master

Jenkins job will automatically detect changes and deploy the updated repository to /var/www/html.


6️⃣ Verify Deployment
Open browser → https://<LBR-URL>
Expected output:
Welcome to the xFusionCorp Industries


Notes:
Make sure content is served from root, not /web.
If content doesn’t appear, check /var/www/html and Poll SCM settings.


7️⃣ Troubleshooting
Permission denied during SCP: Verify /var/www/html ownership and permissions.

Job does not auto-run: Check Poll SCM schedule or webhook.

Partial deployment (only index.html): Use scp * or **/* for full repo.

Apache port issue: Verify Apache running on 8080 on all App Servers.


✅ Task Checklist
Apache installed & running on port 8080 on all App Servers

 Jenkins plugins installed (Git, SSH, SSH Credentials, SSH Build Agents)

 Jenkins job nautilus-app-deployment created & configured

 Passwordless SSH configured between Jenkins → Storage Server

 /var/www/html ownership set to user natasha

 Repository deployed via Jenkins job successfully

 index.html updated and pushed → triggers job

 Verify updated site on https://<LBR-URL>










Repeat for all App Servers.
