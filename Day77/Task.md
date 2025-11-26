# **📘 OneNote Documentation — Jenkins Deploy Pipeline**

---

## **📌 Task Name:** Jenkins Deploy Pipeline

## **🏢 Project:** xFusionCorp Industries — Static Website Deployment

---

## **🎯 Objective**

Set up a Jenkins Pipeline job to deploy the **web_app** static website from Gitea onto the **Storage Server**, which is mounted to App Servers running Apache on port **8080**.

---

# **🧩 1. Preliminary Setup**

## **1.1 Login to Jenkins**

* **URL:** Click Jenkins button
* **Username:** `admin`
* **Password:** `Adm!n321`

## **1.2 Login to Gitea**

* **URL:** Click Gitea button
* **Username:** `sarah`
* **Password:** `Sarah_pass123`
* **Repository:** `web_app` (already cloned under `/var/www/html` on Storage Server)

---

# **🔧 2. Install Required Jenkins Plugins**

Navigate to:

```
Manage Jenkins → Plugins → Available Plugins
```

Install:

* SSH Build Agents
* Git Plugin
* Pipeline
* Credentials Plugin

After installation:
✔️ Click **Restart Jenkins when installation is complete and no jobs are running**.

---

# **📡 3. Prepare the Storage Server (Slave Node)**

## **3.1 SSH into the Storage Server**

```bash
ssh natasha@ststor01
sudo su -
```

(Use password from server details)

## **3.2 Install Java (Required for Jenkins Agent)**

```bash
java -version
yum install java-17-openjdk -y
```

Java installation ensures the Jenkins agent can run correctly.

---

# **🔐 4. Add Storage Server Credentials**

Navigate to:

```
Manage Jenkins → Credentials → System → Global credentials → Add Credentials
```

Enter:

* **Username:** `natasha`
* **Password:** (from server details)
* **ID:** `natasha`
* Click **Save**

---

# **🖥️ 5. Add Jenkins Node (Slave)**

Navigate to:

```
Manage Jenkins → Nodes → New Node
```

Configure:

* **Node Name:** Storage Server
* **Type:** Permanent Agent
* **Remote Root Directory:** `/var/www/html`
* **Labels:** `ststor01`
* **Usage:** Only build jobs with matching label
* **Launch Method:** Launch agent via SSH

  * **Host:** `ststor01`
  * **Credentials:** natasha
  * **Host Key Verification:** Manually trusted key verification strategy

Save configuration.

---

# **⚠️ 6. Fix Permission Error (remoting.jar Copy Failure)**

If the Jenkins agent fails with error:

```
Permission denied: cannot copy remoting.jar to /var/www/html
```

Fix by setting proper ownership:

```bash
cd /var/www
ls -l
chown -R natasha html/
ls -l
```

After this, **relaunch the agent** → ✔️ It should connect successfully.

---

# **🌐 7. Validate Apache on App Servers**

Apache runs on port **8080**.

Check default page:

```
Welcome to XfusionCorp Industries!
```

If this page appears, remove the default file from Storage Server:

```bash
cd /var/www/html
rm index.html
```

The page should now be blank → storage directory is ready for deployment.

---

# **🚀 8. Create Jenkins Pipeline Job**

Navigate to:

```
New Item → Pipeline → Name: nautilus-webapp-job
```

## **Pipeline Script:**

```groovy
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
```

Save the job.

---

# **🏁 9. Build & Verify Deployment**

## **9.1 Run the Build**

* Click **Build Now**
* Job should complete successfully (Green)

## **9.2 Validate Deployment Output**

Click the **App** (Load Balancer URL):

```
Welcome to XfusionCorp Industries!
```

✔ Deployment successful
✔ No subdirectory should appear (must load directly from `/var/www/html`)

---

# **📎 Notes**

* Always restart Jenkins after plugin installation.
* Refresh the browser if UI freezes.
* Keep screenshots or recordings for task verification.

---

📘 **Jenkins Deploy Pipeline Documentation Completed**

