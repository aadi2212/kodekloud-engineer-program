# **Jenkins Conditional Pipeline – OneNote Style Documentation**

---

## **📌 Task Name: Jenkins Conditional Pipeline Deployment**

This task involves configuring a Jenkins pipeline that conditionally deploys code based on a branch parameter. The deployment targets the Storage Server, which is mounted to all App Servers for serving a static website.

---

## **🧾 Task Overview**

The development team of xFusionCorp Industries is building a static website and wants to deploy it using a Jenkins conditional pipeline. The pipeline must:

* Deploy from **master** branch when BRANCH parameter = master
* Deploy from **feature** branch when BRANCH parameter = feature
* Use a slave node labeled **ststor01**
* Deploy into `/var/www/html` on Storage Server

The repo is available under **sarah/web_app** in Gitea.

---

## **🖥️ Server Details**

* **Jenkins Login:** admin / Adm!n321
* **Gitea Login:** sarah / Sarah_pass123
* **Storage Server SSH User:** natasha
* **Repository:** `/var/www/html` (already cloned)
* **App Servers:** Apache running on port 8080

---

## **📝 Step-by-Step Implementation**

### **1️⃣ Login to Storage Server**

```bash
ssh natasha@ststor01
sudo su
```

Check server passwords from provided server details.

---

### **2️⃣ Install Java (Required for Jenkins Agent)**

```bash
yum install java-17-openjdk -y
```

---

### **3️⃣ Install Required Jenkins Plugins**

Go to:

```
Manage Jenkins → Plugins → Available Plugins
```

Install:

* SSH Build Agents
* Git Plugin
* Credentials Plugin
* Pipeline Plugin

Restart Jenkins after installation.

---

### **4️⃣ Fix Directory Ownership**

Move to repo directory:

```bash
cd /var/www/
ls -l
```

Change ownership to Jenkins user (`natasha`):

```bash
chown -R natasha html/
```

---

### **5️⃣ Create Credentials for Storage Server**

```
Credentials → System → Global Credentials → Add Credentials
```

**Values:**

* Username: `natasha`
* Password: `Bl@kW`
* ID: `natasha`

---

### **6️⃣ Create Jenkins Slave Node (Storage Server)**

```
Manage Jenkins → Nodes → New Node
```

**Node Settings:**

* Node Name: **Storage Server**
* Type: **Permanent Agent**
* Remote Root Dir: `/var/www/html`
* Labels: `ststor01`
* Usage: Only build jobs with matching label
* Launch Method: SSH
* Host: `ststor01`
* Credentials: `natasha`
* Host Key Verification: Manually trusted

Save and **Launch Agent** → Node becomes **online**.

---

## **7️⃣ Create Pipeline Job – datacenter-webapp-job**

```
New Item → datacenter-webapp-job → Pipeline
```

Add Parameter:

* **String Parameter – BRANCH** (default: master)

### **📄 Pipeline Script**

```groovy
pipeline {
    agent {
        label 'ststor01'
    }

    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch to deploy (master or feature)')
    }

    stages {
        stage('Deploy') {
            when {
                expression {
                    params.BRANCH == 'master' || params.BRANCH == 'feature'
                }
            }
            steps {
                script {
                    if (params.BRANCH == 'master') {
                        git branch: 'master', url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git'
                    } else if (params.BRANCH == 'feature') {
                        git branch: 'feature', url: 'http://git.stratos.xfusioncorp.com/sarah/web_app.git'
                    }

                    sh "cp -r /var/www/html/workspace/datacenter-webapp-job/* /var/www/html/"
                }
            }
        }
    }
}
```

---

### **8️⃣ Build the Job (Master Branch Test)**

* Click **Build with Parameters** → BRANCH = master
* Job builds successfully
* Click **App** button → Should display:

**“Welcome to XfusionCorp Industries!”**

---

### **9️⃣ Build the Job (Feature Branch Test)**

* Click **Build with Parameters** → BRANCH = feature
* Click App button again → Shows updated content

---

### **🔟 Verify Branch Contents in Gitea**

Login to Gitea → `sarah/web_app` Repo

* **master branch:** Contains text *“Welcome to XfusionCorp Industries”*
* **feature branch:** Contains text *“updated”*

---

## **📌 Final Output Verification**

Check main URL from LB:

```
https://<LBR-URL>
```

✔ Must show latest deployed content
✔ Should **NOT** show sub-directory like `/web_app`

---

## **✅ Task Completed Successfully**

This completes the Jenkins conditional pipeline setup for automated static website deployment using Storage Server as the shared mount point.

---
