# Jenkins Database Backup Automation

## 📌 Task Objective

Automate the backup of the **kodekloud_db01** database on the Database Server and store the backup on the Backup Server.

The Jenkins job must:

1. Generate MySQL dump using user **kodekloud_roy** (password: `asdfgdsd`)
2. Filename format: `db_$(date +%F).sql`
3. Copy dump to Backup Server → `/home/clint/db_backups/`
4. Schedule job to run every 10 minutes: `*/10 * * * *`

---

## 1️⃣ Login to Jenkins

* Open Jenkins UI
* Login credentials:

  * **Username:** admin
  * **Password:** Adm!n321

---

## 2️⃣ Install Required Plugins

Navigate to:
**Manage Jenkins → Plugins**

Install the following:

* SSH
* SSH Credentials
* SSH Build Agents

After installation:
✔ **Restart Jenkins when installation is complete**

---

## 3️⃣ Add SSH Credentials for Database Server (stdb01)

Navigate to:
**Manage Jenkins → Credentials → System → Global credentials → Add Credentials**

Enter:

* **Username:** peter
* **Password:** (from server details)
* **ID:** db_creds
* Save

---

## 4️⃣ Configure SSH Remote Host (DB Server)

Navigate to:
**Manage Jenkins → Configure System**

Under **SSH remote hosts / SSH Sites:**

* Click **Add**
* **Hostname:** stdb01
* **Port:** 22
* **Credentials:** peter (db_creds)
* Click **Test Connection** → It should display **Successful connection**

Save.

---

## 5️⃣ Create Jenkins Job — database-backup

Navigate:
**New Item → Freestyle Project → database-backup → OK**

Enable Build Trigger:

* **Build periodically**

Schedule:

```
*/10 * * * *
```

---

## 6️⃣ Build Step Part 1 — Create DB Dump

Navigate to:
**Build Steps → Execute shell script on remote host using SSH**

Command:

```
mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > db_$(date +%F).sql
```

Save job.

---

## 7️⃣ Configure Passwordless SSH (DB → Backup Server)

SSH into Jump Host → DB Server:

```
ssh peter@stdb01
```

Generate SSH key:

```
ssh-keygen -t rsa
```

(Press Enter to all prompts)

Copy key to Backup Server:

```
ssh-copy-id clint@stbkp01
```

* Type **yes**
* Enter password

Test passwordless login:

```
ssh clint@stbkp01
```

Should log in without password.

---

## 8️⃣ Build Step Part 2 — Add SCP Command

Edit Jenkins job → Configure → Build Step:

```
mysqldump -u kodekloud_roy -pasdfgdsd kodekloud_db01 > db_$(date +%F).sql
scp -o StrictHostKeyChecking=no db_$(date +%F).sql clint@stbkp01:/home/clint/db_backups/
```

Save.

---

## 9️⃣ Run Jenkins Job

Click **Build Now** → Ensure job succeeds.

---

## 🔟 Validate Backup on Backup Server

From DB Server:

```
ssh clint@stbkp01
ls
cd db_backups
ls
```

You should see:

```
db_2025-11-14.sql
```

View file:

```
cat db_2025-11-14.sql
```

This confirms the backup is correct.

---

## ✅ Final Result

✔ Automated Jenkins job created
✔ MySQL backup generated
✔ Backup transferred to Backup Server via SCP
✔ Cron scheduling every 10 minutes configured
✔ Verified working backup

Task completed successfully.
