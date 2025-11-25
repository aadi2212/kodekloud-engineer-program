# 📄 Jenkins Job Documentation — `install-packages`

Automated remote package installation using a Jenkins parameterized Freestyle Job.

---

## 📌 Task Objective

Create a Jenkins job that:

- Accepts a package name as a parameter  
- Connects to the storage server via SSH  
- Installs the specified package using `yum`  
- Executes successfully for multiple package names  

---

## 🚀 1. Login to Jenkins

Open Jenkins UI and login:

- **Username:** admin  
- **Password:** Adm!n321  

---

## 🧱 2. Create a New Jenkins Job

Navigate:

New Item → Freestyle Project


Enter job name:

install-packages


Click **OK**.

---

## ⚙️ 3. Configure Job Parameters

Enable:


This project is parameterized

### 📝 String Parameter

| Field | Value |
|-------|--------|
| **Name** | PACKAGE |
| **Default Value** | git |
| **Description** | Enter the package name to install on the storage server |

---

## 🖥 4. Add Build Step — Execute Shell

Navigate:


Build → Add build step → Execute shell

Add script:

```bash
#!/bin/bash
set -euo pipefail

if [ -z "${PACKAGE:-}" ]; then
  echo "ERROR: PACKAGE parameter is empty."
  exit 1
fi

SSH_USER="natasha"
SSH_HOST="ststor01"
SSH_PASS="Bl@kW"

sshpass -p "$SSH_PASS" ssh -o StrictHostKeyChecking=no ${SSH_USER}@${SSH_HOST} bash -s -- "$PACKAGE" <<'REMOTE'
pkg="$1"
echo "Installing package on remote host: $pkg"
echo 'Bl@kW' | sudo -S yum install -y "$pkg"
REMOTE

Click Save.


▶️ 5. Build the Job

Navigate:
Build with Parameters

Enter:
PACKAGE: git (or wget, vim, etc.)

Run the build.

Expected Console Output:
Installing package on remote host: git
Complete!
Finished: SUCCESS


❗ Errors Encountered & Fixes
1️⃣ Sudo TTY Error
Error:
sudo: a terminal is required to read the password

Fix: Added sudo -S to pass password through stdin.

2️⃣ PACKAGE Not Passed to Remote Host
Error:
yum install: error: the following arguments are required: PACKAGE

Cause: $PACKAGE expanded locally in Jenkins instead of on the remote machine.
Fix: Used a here-document so $PACKAGE expands on the remote host.

✅ Final Result

✔ Jenkins job created successfully
✔ PACKAGE parameter functional
✔ Remote installation working
✔ Tested with multiple package names
✔ Console output verified


📌 Notes

sshpass is acceptable in training labs but insecure for production

Use SSH keys + Jenkins Credentials Store for secure automation

Ensure the remote user has sudo privileges

Use screenshots or screen recordings for documentation if required


🎉 Task Completed Successfully



