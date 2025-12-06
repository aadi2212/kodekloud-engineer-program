# Website Backup Automation – App Server 3 (Stratos DC)

## 📌 Task Overview

The production support team needs to **automate backup of the static website** on App Server 3.

**Objective:**

* Create a bash script `news_backup.sh`
* Backup `/var/www/html/news` as a zip archive
* Save locally and copy to Nautilus Backup Server
* Ensure passwordless SSH for automation

---

## 🛠 Step-by-Step Execution

### 1️⃣ SSH into App Server 3

```bash
ssh banner@172.16.238.10
# password: BigGr33n
```

---

### 2️⃣ Prepare Directories

```bash
sudo mkdir -p /scripts /backup
sudo chown banner:banner /scripts
```

---

### 3️⃣ Create Backup Script

```bash
vi /scripts/news_backup.sh
```

**Add the following content:**

```bash
#!/bin/bash

# Variables
SRC_DIR="/var/www/html/news"
TMP_BACKUP="/backup/xfusioncorp_news.zip"
REMOTE_USER="clint"
REMOTE_HOST="stbkp01.stratos.xfusioncorp.com"
REMOTE_BACKUP="/backup/"

# Step 1: Create zip archive
zip -r $TMP_BACKUP $SRC_DIR

# Step 2: Copy archive to backup server
scp $TMP_BACKUP ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_BACKUP}
```

---

### 4️⃣ Make Script Executable

```bash
chmod +x /scripts/news_backup.sh
```

---

### 5️⃣ Enable Passwordless SSH

**Generate SSH key (if not already):**

```bash
ssh-keygen -t rsa
```

**Copy public key to Backup Server:**

```bash
ssh-copy-id clint@stbkp01.stratos.xfusioncorp.com
```

**Alternative method (if errors occur):**

```bash
mkdir -p ~/.ssh
touch ~/.ssh/authorized_keys
# Paste public key into authorized_keys
echo "your-public-key banner@stapp03.stratos.xfusioncorp.com=clint@stbkp01.stratos.xfusioncorp.com" >> ~/.ssh/authorized_keys
```

**Test passwordless SSH:**

```bash
ssh clint@stbkp01.stratos.xfusioncorp.com
exit
```

---

### 6️⃣ Test Backup Script

```bash
/scripts/news_backup.sh
```

---

### 7️⃣ Verify Backup

**On App Server 3:**

```bash
ls -l /backup/xfusioncorp_news.zip
```

**On Backup Server:**

```bash
ssh clint@stbkp01.stratos.xfusioncorp.com "ls -l /backup/xfusioncorp_news.zip"
```

---

## ✅ Final Outcome

* Backup script created and executable at `/scripts/news_backup.sh`
* Archive saved locally in `/backup/` and copied to Backup Server
* Passwordless SSH configured for automation
* Task completed successfully ✅

# End of Documentation
