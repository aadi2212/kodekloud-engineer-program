# 🚀 Ansible Manage Services – Install and Configure vsftpd

## 📌 Task Description
The Nautilus DevOps team needs to install and manage the **vsftpd (FTP) service** on all application servers in the Stratos Datacenter using Ansible.  
This task focuses on automating package installation and service management.

---

## 🎯 Requirements
- Install **vsftpd** on all app servers
- Start and enable **vsftpd** service
- Use existing inventory file
- Playbook must run using:
  ```bash
  ansible-playbook -i inventory playbook.yml

User thor must be able to run the playbook on the jump host


🗂️ Directory Structure
/home/thor/ansible
├── inventory
└── playbook.yml

🖥️ Inventory File

Location: /home/thor/ansible/inventory

stapp01 ansible_host=172.16.238.10 ansible_user=tony   ansible_ssh_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve  ansible_ssh_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n

🛠️ Ansible Playbook

Location: /home/thor/ansible/playbook.yml

---
- name: Install and Configure vsftpd on App Servers
  hosts: all
  become: true

  tasks:
    - name: Install vsftpd package
      yum:
        name: vsftpd
        state: present

    - name: Start and enable vsftpd service
      service:
        name: vsftpd
        state: started
        enabled: yes

▶️ Execution Steps
Step 1: Navigate to Ansible directory
cd /home/thor/ansible

Step 2: Run the playbook
ansible-playbook -i inventory playbook.yml

⚠️ Observed Issue

During the first execution, the playbook failed on stapp03 with the following error:

rc: 137
Shared connection closed

🔁 Resolution

The playbook was re-run:

ansible-playbook -i inventory playbook.yml

Why this works

Ansible is idempotent

Servers that already succeeded are skipped

Failed hosts are retried automatically

✅ Verification
Check vsftpd service status on all servers
ansible all -i inventory -a "systemctl status vsftpd"


Expected result:

Service state: active (running)

Service enabled on boot

📋 Validation Checklist

 vsftpd installed on all app servers

 vsftpd service started

 vsftpd service enabled

 Playbook runs without extra arguments

 Executable by user thor

🧠 Key Learnings

Package installation using Ansible

Service management using the service module

Importance of Ansible idempotency

Handling transient failures in lab environments

🏁 Conclusion

This task demonstrates how Ansible can be used to automate service installation and management across multiple servers in a reliable and repeatable way.
