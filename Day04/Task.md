# Script Execution Permissions – App Server 1 (Stratos DC)

## 📌 Task Overview

The xFusionCorp Industries sysadmin team developed a new bash script `xfusioncorp.sh` for backup automation. On App Server 1, the script exists but **lacks executable permissions**.

**Objective:**

* Grant executable permissions to `/tmp/xfusioncorp.sh`
* Ensure all users can read and execute the script

---

## 🛠 Step-by-Step Execution

### 1️⃣ SSH into App Server 1

```bash
ssh tony@172.16.238.10
```

---

### 2️⃣ Verify Current Script Permissions

```bash
ls -l /tmp/xfusioncorp.sh
```

**Sample Output:**

```
---------- 1 root root 40 Aug 11 06:57 /tmp/xfusioncorp.sh
```

* `x` missing indicates script is **not executable**

---

### 3️⃣ Grant Execute Permission to All Users

```bash
chmod a+x /tmp/xfusioncorp.sh
```

* `a+x` → Adds **execute** permission for **owner, group, others**

---

### 4️⃣ Verify Permissions After Change

```bash
ls -l /tmp/xfusioncorp.sh
```

**Sample Output:**

```
---x--x--x 1 root root 40 Aug 11 06:57 /tmp/xfusioncorp.sh
```

---

### 5️⃣ Correct Permissions: Add Read & Execute for All

```bash
chmod a+rx /tmp/xfusioncorp.sh
```

* `r` → read permission
* `x` → execute permission

---

### 6️⃣ Verify Final Permissions

```bash
ls -l /tmp/xfusioncorp.sh
```

**Expected Output:**

```
-r-xr-xr-x 1 root root 40 Aug 11 06:57 /tmp/xfusioncorp.sh
```

---

## ✅ Final Outcome

* Script `/tmp/xfusioncorp.sh` is **readable and executable by all users**
* Ready for automation tasks without permission issues

# End of Documentation
