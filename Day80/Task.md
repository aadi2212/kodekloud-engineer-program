# Day 80 — Jenkins Chained Builds | KodeKloud 100 Days of DevOps

## 📌 Task Overview

This task demonstrates **Jenkins Chained Builds**, connecting multiple jobs to run sequentially:

* **Upstream Job** → `Nautilus-app-deployment` (pulls code from Git repository)
* **Downstream Job** → `manage-services` (restarts Apache on all App Servers)

The goal is to set up a **Continuous Deployment (CD) pipeline** that:

1. Updates code on a shared storage server
2. Restarts Apache on all App Servers **only if the upstream job is successful**

---

## 🔄 Concepts

* **Chained Builds:** Multiple Jenkins jobs connected so they execute **in sequence**.
  Example: `JobA → JobB → JobC`

  * JobA finishes → triggers JobB
  * JobB finishes → triggers JobC

* **Upstream Job:** Runs first and triggers downstream jobs when it completes.

* **Downstream Job:** Runs after the upstream job completes successfully.

---

## 🏁 Step 1 — Access Jenkins UI

* URL: Jenkins button / local instance
* Username: `admin`
* Password: `Adm!n321`

---

## ⚙️ Step 2 — Install Required Plugin

1. Go to **Manage Jenkins → Plugins → Available Plugins**
2. Search for **Publish over SSH**
3. Install the plugin and **restart Jenkins** after installation

---

## 🔧 Step 3 — Configure SSH Servers

1. Navigate to **Manage Jenkins → System → SSH Servers**
2. Click **Add** and configure:

   * **Storage Server** (shared code directory)
   * **App Servers:** `stapp01`, `stapp02`, `stapp03`
3. Click **Test Configuration** for each server → ensure **success message** appears
4. Click **Save**

---

## 🚀 Step 4 — Create Upstream Job

### Job Name: `Nautilus-app-deployment`

1. Jenkins → **New Item → Freestyle Project** → Name: `Nautilus-app-deployment`
2. **Build Step:** Add **Send files or execute commands over SSH**

   * Server Name: `ststor01` (Storage Server)
   * Exec Commands:

     ```bash
     cd /var/www/html
     git pull origin master
     ```
3. **Post-build Action:**

   * Select **Build other projects**
   * Project to build: `manage-services`
   * Check **Trigger only if build is stable**
4. Click **Save**

---

## 🔧 Step 5 — Create Downstream Job

### Job Name: `manage-services`

1. Jenkins → **New Item → Freestyle Project** → Name: `manage-services`
2. Enable **The project is parameterized**

   * Add **Password Parameter** for each App Server:

| Variable Name  | Default Value         |
| -------------- | --------------------- |
| `STAPP01_PASS` | App Server 1 password |
| `STAPP02_PASS` | App Server 2 password |
| `STAPP03_PASS` | App Server 3 password |

---

## 🛠️ Step 6 — Configure Build Steps in Downstream Job

1. Add **Send files or execute commands over SSH** for each App Server:

* **stapp01**

```bash
echo $STAPP01_PASS | sudo -S systemctl restart httpd
```

* **stapp02**

```bash
echo $STAPP02_PASS | sudo -S systemctl restart httpd
```

* **stapp03**

```bash
echo $STAPP03_PASS | sudo -S systemctl restart httpd
```

2. Click **Save**

---

## ✅ Step 7 — Validate the Pipeline

1. Trigger `Nautilus-app-deployment` manually
2. Confirm downstream job `manage-services` runs automatically **only if upstream job is stable**
3. Verify Apache restarted on all App Servers:

```bash
systemctl status httpd
```

4. Verify website loads via **main URL**, not subdirectory

---

## 💡 Tips & Notes

* **Chained Builds** ensure sequential execution and reduce manual intervention
* Use **parameters** instead of hardcoding passwords for security
* Always **test SSH connections** before adding commands to Jenkins jobs
* Post-build actions ensure downstream jobs run **only on successful builds**

---

# End of Documentation

Your Jenkins Chained Builds setup is complete and ready for Continuous Deployment.
