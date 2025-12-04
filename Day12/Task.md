# Apache Service Troubleshooting – Port 8086 (Stratos DC)

## 📌 Task Overview

The monitoring system reported that **Apache service on App Server 1 (stapp01)** was not reachable on **port 8086**.

**Objective:**

* Identify the cause of Apache unavailability
* Ensure Apache service is running and accessible on port 8086
* Maintain security compliance while allowing access from Jump Host

---

## 🛠 Step-by-Step Execution

### 1️⃣ Check Apache Service Status

```bash
sudo systemctl status httpd
```

* Found service in **failed state**
* Logs showed: `(98)Address already in use: AH00072: make_sock: could not bind to address 0.0.0.0:8086`

---

### 2️⃣ Identify Process Using Port 8086

```bash
sudo netstat -tulnp | grep 8086
```

* Output: `tcp 0 0 127.0.0.1:8086 0.0.0.0:* LISTEN 429/sendmail:accep`
* **Sendmail service** was occupying port 8086

---

### 3️⃣ Stop Conflicting Service

```bash
sudo systemctl stop sendmail
sudo systemctl disable sendmail
```

---

### 4️⃣ Start Apache Service

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

Verify listening port:

```bash
sudo netstat -tulnp | grep 8086
```

* Output: `tcp 0 0 0.0.0.0:8086 0.0.0.0:* LISTEN <pid>/httpd`

---

### 5️⃣ Verify External Access

From Jump Host:

```bash
curl http://172.16.238.10:8086
```

* Issue: `curl: (7) Failed to connect … No route to host`
* Root cause: **iptables firewall blocking port 8086**

---

### 6️⃣ Configure Firewall to Allow Port 8086

```bash
sudo iptables -I INPUT -p tcp --dport 8086 -j ACCEPT
sudo service iptables save
```

Verify rule:

```bash
sudo iptables -L -n | grep 8086
```

* Output: `ACCEPT tcp -- 0.0.0.0/0 0.0.0.0/0 tcp dpt:8086`

---

### 7️⃣ Verification

**On App Server 1:**

```bash
curl http://localhost:8086
```

**From Jump Host:**

```bash
curl http://172.16.238.10:8086
```

* Apache Test Page returned successfully ✅

---

## ✅ Root Causes & Resolution

**Root Causes:**

1. Sendmail service was using port 8086 → prevented Apache from starting
2. iptables firewall blocked external access → Jump Host unable to connect

**Resolution Steps:**

* Stopped and disabled Sendmail service
* Configured Apache to listen on `0.0.0.0:8086`
* Added iptables rule to allow TCP traffic on port 8086
* Verified connectivity locally and remotely

---

## ✅ Final Outcome

* Apache is fully operational on port 8086
* Accessible from Jump Host without compromising security
* Task completed successfully ✅

# End of Documentation
