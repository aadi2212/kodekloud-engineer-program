# Jenkins Multistage Pipeline – Static Website Deployment

<p align="center">
  <img alt="Jenkins" src="https://img.shields.io/badge/Jenkins-Pipeline-blue?style=for-the-badge&logo=jenkins&logoColor=white">
  <img alt="Git" src="https://img.shields.io/badge/Git-Repository-orange?style=for-the-badge&logo=git&logoColor=white">
  <img alt="Gitea" src="https://img.shields.io/badge/Gitea-Code-green?style=for-the-badge&logo=gitea&logoColor=white">
  <img alt="Linux" src="https://img.shields.io/badge/Linux-Servers-yellow?style=for-the-badge&logo=linux&logoColor=black">
  <img alt="Automation" src="https://img.shields.io/badge/Automation-CI/CD-purple?style=for-the-badge&logo=githubactions&logoColor=white">
</p>

---

## 📌 Task Overview

The development team of **xFusionCorp Industries** is building a new static website and requires automated deployment using a **Jenkins multistage pipeline**.

Your responsibilities:

- Update website content in the Gitea repository  
- Create a Jenkins Pipeline job (NOT multibranch)  
- Add two stages: **Deploy** and **Test**  
- Deploy the latest code to the Storage Server  
- Verify website functionality using the Load Balancer URL  

---

## 🎯 Objectives

- Edit `index.html` under the `sarah/web` repository  
- Create a Jenkins job named **deploy-job**  
- Deploy code to `/var/www/html` on Storage Server  
- Test website access via `http://stlb01:8091`  
- Fail pipeline automatically if Deploy or Test stage fails  

---

## 🛠️ Step 1 — Update Repository Content in Gitea

### SSH into Storage Server
```bash
ssh natasha@ststor01
```

### Navigate to the repository directory
```bash
cd /var/www/html
```

### Edit the website file
```bash
vim index.html
```

Replace content with:
```
Welcome to xFusionCorp Industries
```

### Commit & Push the changes
```bash
git add index.html
git commit -m "Updated index.html"
git push origin master
```

---

## 🛠️ Step 2 — Login to Jenkins

Use Jenkins button → Login using:
- **Username:** admin  
- **Password:** Adm!n321  

---

## 🛠️ Step 3 — Install Required Plugins

Navigate to:  
**Manage Jenkins → Plugins → Available Plugins**

Install:
- Git plugin  
- Pipeline plugin  

Restart Jenkins after plugin installation.

---

## 🛠️ Step 4 — Configure Jenkins Credentials

Navigate to:  
**Manage Jenkins → Credentials → (global)**

### 1️⃣ Gitea Credentials
| Field | Value |
|--------|--------|
| Username | sarah |
| Password | Sarah_pass123 |
| ID | sarah-creds |

### 2️⃣ Storage Server SSH Credentials
| Field | Value |
|--------|--------|
| Username | natasha |
| Password | Bl@kW |
| ID | natasha-creds |

---

## 🛠️ Step 5 — Create Jenkins Pipeline Job

### Create a new job:
- **Name:** `deploy-job`  
- **Type:** Pipeline  

Scroll down to **Pipeline Script** and paste the following:

---

## 📜 Jenkins Pipeline Script

```groovy
pipeline {
    agent any

    stages {
        stage('Deploy') {
            steps {
                git branch: "master",
                    credentialsId: 'sarah-creds',
                    url: 'http://git.stratos.XfusionCorp.Com/sarah/web.git'

                withCredentials([
                    usernamePassword(
                        credentialsId: 'natasha-creds',
                        usernameVariable: 'SSH_USER',
                        passwordVariable: 'SSH_PASS'
                    )
                ]) {
                    sh """
                        sshpass -p "$SSH_PASS" scp -o StrictHostKeyChecking=no index.html $SSH_USER@ststor01:/var/www/html/
                    """
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Application …'
                sh 'curl -f http://stlb01:8091'
            }
        }
    }
}
```

---

## 🛠️ Step 6 — Build & Validate

- Click **Save**
- Click **Build Now**
- Ensure both stages complete successfully  
  - ✔ Deploy  
  - ✔ Test  

---

## 🛠️ Step 7 — Final Verification

Open the Load Balancer URL:

```
http://stlb01:8091
```

Expected Output:
```
Welcome to xFusionCorp Industries
```

---

## ✔ Final Result

If the expected message appears via the LBR URL, your Jenkins deployment pipeline is **successfully completed**.

