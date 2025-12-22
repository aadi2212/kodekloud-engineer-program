# 🚀 Ansible Manage Services – Install and Configure vsftpd

## 📌 Task Overview
The Nautilus DevOps team required an Ansible playbook to automate the installation and management of the **vsftpd (FTP) service** on all application servers in the Stratos Datacenter.

This task demonstrates how Ansible can be used to **install packages**, **manage services**, and ensure **idempotent execution** across multiple servers.

---

## 🎯 Objectives
- Install **vsftpd** on all app servers
- Start and enable the **vsftpd service**
- Use an existing Ansible inventory
- Ensure the playbook runs with:
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


Ansible Playbook
Path: /home/thor/ansible/playbook.yml

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



⚠️ Observed Issue and Resolution
Issue
During the initial execution, the playbook failed on stapp03 with the following error:
rc: 137
Shared connection closed


Explanation

Exit code 137 typically indicates a resource or memory constraint on the target server.

This behavior is common in lab environments.


Resolution
Re-running the playbook resolved the issue:
ansible-playbook -i inventory playbook.yml

✔ Ansible’s idempotency ensures already configured servers are skipped and failed hosts are retried.


✅ Verification
Verify vsftpd service status
ansible all -i inventory -a "systemctl status vsftpd"

Expected result:

Service state: active (running)

Service enabled at boot


📋 Validation Checklist

 vsftpd installed on all app servers

 vsftpd service started

 vsftpd service enabled

 Playbook runs without extra arguments

 Executable by user thor



 🧠 Key Learnings

Automating package installation with Ansible

Managing services using the service module

Leveraging Ansible’s idempotent behavior

Handling transient failures in distributed environments



🏁 Conclusion

This project showcases a practical example of using Ansible to manage services across multiple servers, ensuring consistency, reliability, and automation best practices.






