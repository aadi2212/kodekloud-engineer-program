Jenkins Scheduled Jobs — copy-logs
📌 Task Objective

Create a Jenkins job named copy-logs to collect Apache logs from App Server 1 and transfer them to the Storage Server at /usr/src/data.

The Jenkins job must:

Collect access_log and error_log from App Server 1

Transfer them every 2 minutes

Store logs in /usr/src/data

Handle sudo and hostname issues

1️⃣ Login to Jenkins

Open Jenkins UI

Login credentials:

Username: admin

Password: Adm!n321

2️⃣ Install Required Plugin

Navigate to Manage Jenkins → Plugins → Available

Install Publish Over SSH

After installation:
✔ Restart Jenkins when installation is complete

Refresh UI if it freezes

3️⃣ Configure Storage Server for SSH

Navigate: Manage Jenkins → Configure System → Publish Over SSH

Configure server:

Field	Value
Name	StorageServer
Hostname	<Storage Server IP>
Username	natasha
Password	<server password>
Remote Directory	/usr/src/data

Test Connection → Success

Save configuration

4️⃣ Create Jenkins Job — copy-logs

New Item → Freestyle Project → copy-logs → OK

5️⃣ Build Trigger

Enable Build periodically:

*/2 * * * *

6️⃣ Build Step — Fetch Apache Logs

Command:

sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10 \
"echo 'Ir0nM@n' | sudo -S cat /var/log/httpd/access_log" > access_log

sshpass -p 'Ir0nM@n' ssh -o StrictHostKeyChecking=no tony@172.16.238.10 \
"echo 'Ir0nM@n' | sudo -S cat /var/log/httpd/error_log" > error_log


Notes:

Use correct IP instead of hostname

Use sudo -S to bypass TTY requirement

7️⃣ Post-build Action — Transfer Logs

Post-build Actions → Send build artifacts over SSH

SSH Server: StorageServer

Source Files: access_log,error_log

Remove Prefix: leave empty

Remote Directory: leave blank

8️⃣ Run Jenkins Job

Click Build Now → Job should complete successfully

9️⃣ Validate Logs on Storage Server
ssh natasha@<Storage_Server_IP>
ls -l /usr/src/data


Expected output:

access_log
error_log

✅ Final Result

✔ Jenkins job copy-logs created
✔ Apache logs collected from App Server 1
✔ Logs transferred to Storage Server
✔ Cron schedule every 2 minutes
✔ Issues resolved (sudo TTY, hostname, folder structure)
✔ Task completed successfully
