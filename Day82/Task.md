# 🧪 Ansible Inventory Setup for App Server Testing

## 📌 Task Objective
The Nautilus DevOps team is testing Ansible playbooks stored under `/home/thor/playbook/` on the jump host.  
To run these playbooks on **App Server 2**, an Ansible **INI-style inventory** must be created.

This inventory will allow Ansible to connect to **stapp02** using predefined connection variables.

---

## 📁 Step 1 — Navigate to the Playbook Directory

```bash
cd /home/thor/playbook/
```

---

## 📄 Step 2 — Create the Inventory File

Create and edit the file named `inventory`:

```bash
vim inventory
```

Add the following content:

```ini
[app]
stapp02

[app:vars]
ansible_user=steve
ansible_ssh_pass=Am3ric@
```

### 🔍 Explanation
- **[app]** → group name to organize servers  
- **stapp02** → App Server 2 hostname (as per Stratos DC wiki)  
- **ansible_user / ansible_ssh_pass** → connection variables used by Ansible during execution  

---

## 🧪 Step 3 — Test Connectivity with Ping Module

```bash
ansible all -i inventory -m ping
```

Successful output will confirm proper connectivity to **stapp02**.

---

## ▶️ Step 4 — Execute the Playbook

```bash
ansible-playbook -i inventory playbook.yml
```

The playbook should run successfully **without any extra arguments**, since all details are already defined in the inventory.

---

## ✅ Final Result
You have successfully:

- Created an INI-style Ansible inventory  
- Added App Server 2 with required authentication variables  
- Validated server connectivity  
- Executed the playbook successfully  

This setup ensures smooth and automated interaction with App Server 2 using Ansible.

---
