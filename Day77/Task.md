📘 OneNote Documentation — Jenkins Deploy Pipeline

📌 Task Name: Jenkins Deploy Pipeline

🏢 Project: xFusionCorp Industries — Static Website Deployment

🎯 Objective:
Create a Jenkins Pipeline job to deploy the web_app static website from Gitea onto the Storage Server, which is mounted to App Servers running Apache on port 8080.


🧩 1. Preliminary Setup
1.1 Login to Jenkins
	• URL: Click Jenkins button
	• Username: admin
	• Password: Adm!n321

1.2 Login to Gitea
	• URL: Click Gitea button
	• Username: sarah
	• Password: Sarah_pass123
	• Repository: web_app (already cloned on Storage Server under /var/www/html)
	
	
🔧 2. Install Required Jenkins Plugins
Navigate to:
Manage Jenkins → Plugins → Available Plugins
Install the following:
	• SSH Build Agents
	• Git plugin
	• Pipeline
	• Credentials Plugin
After installation:
✔️ Click Restart Jenkins when installation is complete and no jobs are running.



📡 3. Prepare the Storage Server (Slave Node)
3.1 SSH into Storage Server

ssh natasha@ststor01
# enter password from server details
sudo su -



3.2 Install Java (required for Jenkins agent)

java -version
yum install java-17-openjdk -y



🔐 4. Add Storage Server Credentials
Navigate to:
Manage Jenkins → Credentials → System → Global credentials → Add Credentials
Enter:
	• Username: natasha
	• Password: (from server details)
	• ID: natasha
	• Save.



🖥️ 5. Add Jenkins Node (Slave)
Navigate to:
Manage Jenkins → Nodes → New Node
Enter:
	• Node name: Storage Server
	• Type: Permanent Agent
Then configure:
	• Remote Root Directory: /var/www/html
	• Labels: ststor01
	• Usage: Only build jobs with label
	• Launch Method: Launch agents via SSH
		○ Host: ststor01
		○ Credentials: natasha
		○ Host Key Verification: Manually trusted key verification strategy
		○ Save.



⚠️ 6. Fix Permission Error (remoting.jar Copy Failure)
If the agent fails with:
	Permission denied: cannot copy remoting.jar to /var/www/html
Fix Ownership

cd /var/www
ls -l
chown -R natasha html/
ls -l
Now Launch Agent again → ✔️ It should connect successfully.




🌐 7. Validate Apache on App Servers
Check page running on port 8080.
If default page shows:
	Welcome to XfusionCorp Industries!
then remove the default index.html from Storage Server:

cd /var/www/html
rm index.html
Re-check Apache: page should be blank → ready for deployment.



🚀 8. Create Jenkins Pipeline Job
Navigate to:
New Item → Pipeline → Name: nautilus-webapp-job
Pipeline Script:

pipeline {
    agent {
        label 'ststor01'
    }
    stages {
        stage ('Deploy') {
            steps {
                git branch: "master",
                    url: "http://git.stratos.xfusioncorp.com/sarah/web_app.git"
sh "cp -r /var/www/html/workspace/nautilus-webapp-job/* /var/www/html/"
            }
        }
    }
}
Save the job.



🏁 9. Build & Verify Deployment
9.1 Build the Pipeline
	• Click Build Now
	• Build completes successfully (green)
9.2 Validate Apache Output
	• Visit the App link (Load Balancer URL)
	• You should now see:
	Welcome to XfusionCorp Industries!
✔️ Deployment successful
✔️ No subdirectory in URL (must load from /var/www/html)



📎 Notes
	• Always restart Jenkins after plugin installation.
	• Keep screenshots for verification (task reviewers require them).
	• If Jenkins UI freezes after restart → refresh the browser.
![Uploading image.png…]()
