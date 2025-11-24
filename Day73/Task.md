OneNote Documentation: Jenkins Scheduled Job – copy-logs
📌 Task Objective

Configure a Jenkins scheduled job to automatically collect Apache logs from App Server 1 every 2 minutes and store them on the Storage Server at:

/usr/src/data


This temporary setup helps the DevOps team analyze Apache issues until centralized logging is fully implemented.

1️⃣ Login to Jenkins

Username: admin

Password: Adm!n321

Go to the Jenkins dashboard using the top navigation bar.

2️⃣ Install Required Plugin

Jenkins needs SSH-based file transfer support.

🔌 Plugin Required: Publish Over SSH
Steps:

Manage Jenkins → Plugins → Available

Search: Publish Over SSH

Install plugin

Click Restart Jenkins when installation is complete

Refresh UI (UI may freeze during restart)

3️⃣ Configure Storage Server (Publish Over SSH)
Navigation:

Manage Jenkins → Configure System → Publish Over SSH

SSH Server Settings
Field	Value
Name	StorageServer
Hostname	(Storage Server IP / hostname)
Username	natasha
Password	(password)
Remote Directory	/usr/src/data

Click Test Configuration → Success

4️⃣ Create Jenkins Job
Navigation:

New Item → Freestyle Project → Name: copy-logs

5️⃣ Build Triggers

Configure a periodic schedule:

*/2 * * * *


✔ Job runs every 2 minutes

6️⃣ Build Step – Fetch Apache Logs from App Server 1

Apache log location on App Server 1:

/var/log/httpd/access_log
/var/log/httpd/error_log

❗ Issue 1: Hostname not resolved

Error:

ssh: Could not resolve hostname app01


Fix: Used the correct IP
172.16.238.10

❗ Issue 2: sudo requires TTY

Error:

sudo: a terminal is required to read the password


Fix: Pipe password using sudo -S

Working Script:
sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10 \
"echo 'Ir0nM@n' | sudo -S cat /var/log/httpd/access_log" > access_log

sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10 \
"echo 'Ir0nM@n' | sudo -S cat /var/log/httpd/error_log" > error_log


✔ Logs successfully downloaded into Jenkins workspace.

7️⃣ Post-build Action – Transfer Logs to Storage Server
Navigation:

Post-build Actions → Send build artifacts over SSH

❗ Initial Issue: Nested Folder Creation

Wrong output:

/usr/src/data/usr/src/data/access_log
/usr/src/data/usr/src/data/error_log

Cause:

Wrong “Source files”

Misconfigured “Remote Directory”

Incorrect prefix logic

✔ Correct Configuration:
Field	Value
Source files	access_log,error_log
Remove prefix	(Leave empty)
Remote directory	(Leave empty — use default)

Final output stored correctly in:

/usr/src/data/access_log
/usr/src/data/error_log

8️⃣ Final Verification

On Storage Server:

ls -l /usr/src/data


Output:

access_log
error_log


✔ Logs transferred
✔ File names correct
✔ No duplicate folders
✔ Job runs every 2 minutes
✔ Fully functional

✅ Final Status: SUCCESS

All errors resolved:

✔ Hostname resolution fixed
✔ sudo TTY issue fixed
✔ Folder duplication fixed
✔ Logs successfully collected & transferred
