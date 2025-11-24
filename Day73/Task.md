# 📄 Jenkins Job Documentation — `copy-logs`

Automated Apache log collection job for **xFusionCorp Industries**.

---

## 📌 Task Objective

Set up a Jenkins job to:

- Fetch **Apache access_log** and **error_log** from **App Server 1**
- Transfer them to the **Storage Server** at:

/usr/src/data


- Run this process **every 2 minutes**
- Ensure job runs without manual intervention

---

## 🚀 1. Login to Jenkins

Open Jenkins UI and login:

- **Username:** admin  
- **Password:** Adm!n321  

---

## 🔧 2. Install Required Plugin

Navigate:

Manage Jenkins → Plugins → Available


Install:

- **Publish Over SSH**

After installation:
- ✔ Restart Jenkins
- ✔ Refresh UI if it becomes unresponsive

---

## 🖥 3. Configure Storage Server in Jenkins

Navigate:

Manage Jenkins → Configure System → Publish Over SSH


Add SSH Server:

| Field | Value |
|-------|--------|
| **Name** | StorageServer |
| **Hostname** | stbkp01 (or IP) |
| **Username** | natasha |
| **Password** | (server password) |
| **Remote Directory** | /usr/src/data |

Click **Test Configuration** → Should show **Success**  
Save.

---

## 📝 4. Create Jenkins Job — `copy-logs`

Create job:

New Item → Freestyle Project → copy-logs


Enable:

### **Build periodically**
Cron expression to run every 2 minutes:

*/2 * * * *


---

## 📂 5. Build Step — Fetch Apache Logs from App Server 1

Add step:

Add Build Step → Execute shell


Script:

```bash
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10 \
"echo 'Ir0nM@n' | sudo -S cat /var/log/httpd/access_log" > access_log

sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10 \
"echo 'Ir0nM@n' | sudo -S cat /var/log/httpd/error_log" > error_log


✔ Issues Fixed

Corrected hostname: 172.16.238.10

Resolved sudo error using: sudo -S

Avoided TTY requirement

Saved logs into Jenkins workspace

Save job.


📤 6. Post-build Action — Transfer Logs via SSH

Navigate:

Post-build Actions → Send build artifacts over SSH


Configure:

| Field                | Value                                 |
| -------------------- | ------------------------------------- |
| **SSH Server**       | StorageServer                         |
| **Source files**     | access_log,error_log                  |
| **Remove prefix**    | *(leave blank)*                       |
| **Remote directory** | *(leave blank to use server default)* |


Ensures files are stored at:
/usr/src/data/access_log
/usr/src/data/error_log


▶️ 7. Run and Verify Job

Click:
Build Now


Ensure:

✔ Build succeeds

✔ No SSH errors

✔ Logs fetched correctly


🔍 8. Validate on Storage Server

SSH into Storage Server:

ssh natasha@stbkp01


Check log files:
ls -l /usr/src/data


Expected:
access_log
error_log


To view:
cat /usr/src/data/access_log
cat /usr/src/data/error_log


✅ Final Result

✔ Jenkins job copy-logs created

✔ Apache logs successfully fetched

✔ Auto-transferred to Storage Server

✔ Runs every 2 minutes

✔ All issues (sudo, SSH, nesting) fixed

✔ Verified logs present on Storage Server
