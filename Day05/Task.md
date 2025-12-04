# SELinux Installation & Configuration – App Server 2 (Stratos DC)

## 📌 Task Overview

Following a security audit, the security team decided to **enhance server security using SELinux**.

**Objective:**

* Install SELinux management packages
* Permanently disable SELinux for now
* Avoid reboot; final status will reflect after scheduled reboot

---

## 🛠 Step-by-Step Execution

### 1️⃣ Install SELinux Packages

```bash
sudo yum install -y policycoreutils selinux-policy selinux-policy-targeted
```

* Installs essential SELinux management tools and policies

---

### 2️⃣ Permanently Disable SELinux

Edit the SELinux configuration file:

```bash
sudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
```

Alternatively, manually edit `/etc/selinux/config` and set:

```text
SELINUX=disabled
```

---

### 3️⃣ Temporarily Disable SELinux Without Reboot

```bash
sudo setenforce 0
```

* Puts SELinux into **permissive mode** until next reboot

---

### 4️⃣ Verify SELinux Status

```bash
getenforce
```

* Output should show **Permissive** or **Disabled**

---

## ✅ Final Outcome

* SELinux packages installed on App Server 2
* SELinux permanently disabled in config file
* Temporarily set to permissive mode for immediate effect
* Final status after reboot will be **disabled**
* Server ready for further configuration/testing

# End of Documentation
