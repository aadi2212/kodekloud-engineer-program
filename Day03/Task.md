# Secure Root SSH Access – App Servers (Stratos DC)

## 📌 Task Overview

Following security audits, xFusionCorp Industries implemented stricter access control policies. One key measure is **disabling direct SSH root login** on all application servers to prevent unauthorized privileged access.

**Objective:**

* Disable direct SSH root login on all App Servers:

  * App Server 1
  * App Server 2
  * App Server 3

**Project Date:** 10-Aug-2025
**Category:** Linux Server Security / SSH Configuration

---

## 🛠 Step-by-Step Implementation

### 1️⃣ SSH into Each App Server

```bash
ssh tony@172.16.238.10
# Repeat for App Server 2 and 3 using respective sudo users
```

---

### 2️⃣ Switch to Root

```bash
sudo su -
```

---

### 3️⃣ Edit SSH Configuration

```bash
vi /etc/ssh/sshd_config
```

Locate the line:

```
PermitRootLogin yes
```

* Change it to:

```
PermitRootLogin no
```

> ⚡ If the line is commented (`#`), remove the `#` and set it to `no`.

---

### 4️⃣ Restart SSH Service

```bash
systemctl restart sshd
```

---

### 5️⃣ Verify Configuration

```bash
grep PermitRootLogin /etc/ssh/sshd_config
```

**Expected Output:**

```
PermitRootLogin no
```

---

## ✅ Validation

* Confirm that **root SSH login is blocked**.
* Only non-root accounts with **sudo privileges** can access via SSH.

---

## 🔒 Security Impact

* Reduces the risk of **brute-force attacks** on the root user
