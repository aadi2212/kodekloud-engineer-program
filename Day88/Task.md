# 🚀 Ansible Blockinfile Module – HTTPD Web Server Setup

## 📌 Task Description
The Nautilus DevOps team wants to install and configure a simple **Apache (httpd)** web server on all application servers in the Stratos Datacenter.  
Additionally, a sample web page must be deployed using **Ansible only**, leveraging the **blockinfile** module.

---

## 🎯 Task Requirements
1. Install **httpd** on all app servers.
2. Ensure the **httpd service** is running and enabled.
3. Add content to `/var/www/html/index.html` using **blockinfile**.
4. Set **owner** and **group** of the file to `apache`.
5. Set file **permissions** to `0655`.
6. Do **not** use custom or empty markers in `blockinfile`.
7. Playbook must run using:
   ```bash
   ansible-playbook -i inventory playbook.yml


🗂️ Directory Structure
/home/thor/ansible
├── inventory
└── playbook.yml

🖥️ Inventory File

Path: /home/thor/ansible/inventory

stapp01 ansible_host=172.16.238.10 ansible_user=tony   ansible_ssh_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve  ansible_ssh_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n

🛠️ Ansible Playbook

Path: /home/thor/ansible/playbook.yml

---
- name: Install and configure httpd web server
  hosts: all
  become: yes

  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: present

    - name: Ensure httpd service is running and enabled
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Add sample content to index.html
      blockinfile:
        path: /var/www/html/index.html
        create: yes
        block: |
          Welcome to XfusionCorp!
          This is Nautilus sample file, created using Ansible!
          Please do not modify this file manually!

    - name: Set ownership and permissions on index.html
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache
        mode: '0655'

▶️ Execution Steps
Step 1: Navigate to Ansible directory
cd /home/thor/ansible

Step 2: Run the playbook
ansible-playbook -i inventory playbook.yml

✅ Verification
Verify web page content
ansible all -i inventory -a "cat /var/www/html/index.html"


Expected output:

Welcome to XfusionCorp!
This is Nautilus sample file, created using Ansible!
Please do not modify this file manually!


Note: The blockinfile module automatically adds BEGIN and END markers.

Verify file ownership and permissions
ansible all -i inventory -a "ls -l /var/www/html/index.html"


Expected result:

-rw-r-xr-x 1 apache apache ...


✔ Owner: apache
✔ Group: apache
✔ Permissions: 0655

📋 Validation Checklist

 httpd installed on all app servers

 httpd service running and enabled

 index.html created using blockinfile

 Correct content deployed

 Correct ownership and permissions set

 Playbook runs without extra arguments

🧠 Key Learnings

Automating package installation using Ansible

Managing services with the service module

Using blockinfile to safely manage file content

Applying file ownership and permissions declaratively

🏁 Conclusion

This task demonstrates a complete Ansible automation workflow for configuring a web server and deploying content consistently across multiple servers.
