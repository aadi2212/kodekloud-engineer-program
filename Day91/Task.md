# Ansible Lineinfile Module – HTTPD Web Server Setup

## 📌 Task Overview

The Nautilus DevOps team wants to install and configure a simple **httpd web server** on all application servers in **Stratos Datacenter** using **Ansible**.  
A sample web page must be deployed and updated using the **lineinfile module**, ensuring correct content order, ownership, and permissions.

---

## 🖥️ Environment Details

- **Jump Host Path:** `/home/thor/ansible`
- **Inventory File:** `inventory` (already available)
- **Playbook Name:** `playbook.yml`
- **Target Hosts:** All App Servers
- **Privilege Escalation:** Enabled (`become: yes`)

---

## 📋 Task Requirements

1. Install `httpd` web server on all app servers
2. Ensure `httpd` service is started and enabled
3. Create `/var/www/html/index.html` with the following content:

This is a Nautilus sample file, created using Ansible!


4. Add the below line **at the top of the file** using `lineinfile`:



Welcome to Nautilus Group!


5. Set file ownership:
- User: `apache`
- Group: `apache`

6. Set file permissions:
- Mode: `0755`

---

## 📁 Repository Structure



ansible/
├── inventory
└── playbook.yml


---

## ⚙️ Playbook Implementation

Create the playbook file:

```bash
cd /home/thor/ansible
vi playbook.yml

playbook.yml
---
- name: Deploy and Configure Web Server
  hosts: all
  become: yes

  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: present

    - name: Start and Enable httpd Service
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Create index.html with initial content
      copy:
        dest: /var/www/html/index.html
        content: |
          This is a Nautilus sample file, created using Ansible!
        owner: apache
        group: apache
        mode: "0755"

    - name: Add Welcome message to the top of index.html
      lineinfile:
        path: /var/www/html/index.html
        line: "Welcome to Nautilus Group!"
        insertbefore: BOF

▶️ Playbook Execution

Run the playbook using the existing inventory file:

ansible-playbook -i inventory playbook.yml

🔍 Verification

Verify the file content on all app servers:

ansible -i inventory all -m shell -a "cat /var/www/html/index.html"

Expected Output
Welcome to Nautilus Group!
This is a Nautilus sample file, created using Ansible!

✅ Final Result

httpd installed on all app servers

httpd service running and enabled

Web page deployed successfully

Content added in correct order

Ownership set to apache:apache

Permissions set to 0755

Playbook runs successfully using:

ansible-playbook -i inventory playbook.yml

🧠 Key Concepts Used

yum module for package installation

service module for service management

copy module for file creation with permissions

lineinfile module for inserting content at a specific position

Idempotent Ansible playbook design

📌 Notes

insertbefore: BOF ensures the welcome message is always at the top

Ownership and permissions are handled in the copy module to avoid validation errors

No extra arguments are required during playbook execution

🚀 Task Completed Successfully
