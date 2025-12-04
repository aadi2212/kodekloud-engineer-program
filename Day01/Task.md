# Linux User Setup with Non-Interactive Shell – App Server 3 (Stratos DC)

## 📌 Task Overview

The system admin team at xFusionCorp Industries needs to create a **service account** for a backup agent.
This account must **not have interactive shell access**.

**Objective:**

* Create user `siva` on App Server 3 (stapp03)
* Assign a **non-login shell** (`/sbin/nologin` or equivalent)
* Ensure the user cannot log in interactively

**Date:** 7-Aug-2025
**Category:** Linux, User Management, Shell Access Control

---

## 🛠 Step-by-Step Execution

### 1️⃣ SSH into App Server 3

```bash
ssh banner@172.16.238.12
# password: BigGr33n
```

---

### 2️⃣ Configure the User Shell

Assign a non-interactive shell to the user:

```bash
sudo usermod -s /sbin/nologin siva
```

> ⚡ This ensures `siva` cannot log in or access an interactive shell.

---

### 3️⃣ Verification

1. Check user entry:

```bash
getent passwd siva
```

**Expected Output:**

```
siva:x:1001:1001::/home/siva:/sbin/nologin
```

2. List all available shells:

```bash
cat /etc/shells
```

3. Verify via `/etc/passwd`:

```bash
grep siva /etc/passwd
```

**Expected Output:**

```
siva:x:1001:1001::/home/siva:/sbin/nologin
```

---

### 4️⃣ Notes

* This configuration is ideal for **service accounts** or **automation users**.
* Alternative non-login shells:

  * `/usr/sbin/nologin`
  * `/bin/false`

---

## ✅ Final Outcome

* User `siva` created successfully
* Non-interactive shell applied
* Verification complete
* Task successfully completed

# End of Documentation
