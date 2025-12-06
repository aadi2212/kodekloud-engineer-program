# Ansible Installation – Jump Host (Stratos DC)

## 📌 Task Overview

The Nautilus DevOps team has decided to implement **Ansible** for automation and configuration management.

**Objective:**

* Install Ansible version 4.8.0 using `pip3`
* Ensure Ansible is globally accessible for all users on the Jump Host

---

## 🛠 Step-by-Step Execution

### 1️⃣ Install pip3 (if not already installed)

**For RHEL/CentOS:**

```bash
sudo yum install python3-pip -y
```

**For Ubuntu/Debian:**

```bash
sudo apt install python3-pip -y
```

---

### 2️⃣ Install Ansible 4.8.0 System-wide

```bash
sudo pip3 install ansible==4.8.0
```

> ⚡ Note: Do not use `--user` mode, as Ansible needs to be available globally for all users.

---

### 3️⃣ Verify Ansible Installation

```bash
ansible --version
```

**Expected Output:**

```
ansible [core 2.11.x]
```

---

### 4️⃣ Ensure Ansible Binary is Globally Accessible

Global bin paths for pip installs:

* RHEL/CentOS: `/usr/local/bin`
* Ubuntu/Debian: `/usr/local/bin`

Check binary location:

```bash
which ansible
```

* Should return the path where Ansible is installed (e.g., `/usr/local/bin/ansible`)

---

## ✅ Final Outcome

* Ansible 4.8.0 installed successfully on Jump Host
* Binary is accessible globally for all users
* Ready to execute automation tasks across servers ✅

# End of Documentation
