# Apache Service Troubleshooting – Port 5000 Configuration

## 📌 Task Overview

The monitoring system reported **Apache service unavailability** on one of the app servers in the Stratos Datacenter.

**Objective:**

* Identify the faulty app host
* Ensure Apache service is running on all app servers
* Verify Apache listens on **port 5000**
* Resolve any conflicts preventing Apache from starting

---

## 🛠 Step-by-Step Execution

### 1️⃣ Check Apache Status on All App Servers

```bash
sudo systemctl status httpd
```

**Findings:**

* App Server 1 → Failed ❌
* App Server 2 → Running ✅
* App Server 3 → Running ✅

---

### 2️⃣ Investigate Logs for Failure

```bash
sudo journalctl -u httpd --no-pager | tail -20
```

**Error Found:**

```
(98)Address already in use: AH00072: make_sock: could not bind to address [::]:5000
```

---

### 3️⃣ Identify Conflicting Process

```bash
sudo netstat -tulnp | grep 5000
```

* Another process is using port 5000 on App Server 1

---

### 4️⃣ Resolve Port Conflict

Stop and disable conflicting service (example: `sendmail`):

```bash
sudo systemctl stop sendmail
sudo systemctl disable sendmail
```

---

### 5️⃣ Ensure Apache Listens on Port 5000

Edit Apache configuration:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Update:

```
Listen 5000
```

---

### 6️⃣ Restart and Enable Apache

```bash
sudo systemctl daemon-reload
sudo systemctl start httpd
sudo systemctl enable httpd
```

---

### 7️⃣ Verify Listening Port

```bash
sudo netstat -tulnp | grep httpd
```

**Expected Output:**

```
tcp   0   0 0.0.0.0:5000   0.0.0.0:*   LISTEN   <pid>/httpd
```

---

### 8️⃣ Test Apache Locally on Each Server

```bash
curl http://localhost:5000
```

* Apache test page should load ✅

---

### 9️⃣ Test from Jump Host

```bash
curl http://stapp01:5000
curl http://stapp02:5000
curl http://stapp03:5000
```

* All responses successful ✅

---

## ✅ Final Outcome

* Apache service is up and running on all app servers
* Apache configured and verified on port 5000
* Monitoring alerts resolved successfully

# End of Documentation
