# Cron Job Setup – App Servers (Stratos DC)

## 📌 Task Overview

The Nautilus system admins want to **test and deploy scheduled scripts** on all App Servers using cron jobs.

**Objective:**

* Install `cronie` package
* Enable and start cron daemon
* Create a sample cron job to run every 5 minutes as root

---

## 🛠 Step-by-Step Execution

### 1️⃣ Install cronie

SSH into each App Server (App Server 1, 2, 3) and run:

```bash
sudo yum install cronie -y
```

---

### 2️⃣ Start & Enable crond

```bash
sudo systemctl start crond
sudo systemctl enable crond
sudo systemctl status crond
```

* Ensure the service is **active (running)**

---

### 3️⃣ Add Cron Job for Root

Edit root’s crontab:

```bash
sudo crontab -e
```

Add the following line at the end:

```cron
*/5 * * * * echo hello > /tmp/cron_text
```

> ⚡ Note: This will write "hello" to `/tmp/cron_text` every 5 minutes.

---

### 4️⃣ Verify Cron Job

1. List root’s cron jobs:

```bash
sudo crontab -l
```

2. Wait at least 5 minutes, then check the file:

```bash
cat /tmp/cron_text
```

**Expected Output:**

```
hello
```

---

## ✅ Final Outcome

* `cronie` installed and cron daemon running on all App Servers
* Cron job executes every 5 minutes and writes "hello" to `/tmp/cron_text`
* Task successfully validated ✅

# End of Documentation
