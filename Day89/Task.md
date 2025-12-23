# Ansible Manage Services – vsftpd Installation

## 📘 Task Name
**Ansible Manage Services – Install and Configure vsftpd**

---

## 🎯 Objective
The Nautilus DevOps team needs to automate the installation and management of the **vsftpd (FTP)** service on all application servers in the **Stratos Datacenter** using **Ansible**.

---

## 🖥️ Environment Details

### Jump Host
- **User:** thor  
- **Ansible Directory:**  

/home/thor/ansible


### Inventory File
- **Path:**  

/home/thor/ansible/inventory


---

## 📌 Task Requirements
- Install **vsftpd** on all application servers
- Start and enable the **vsftpd** service
- Use the existing inventory file
- Playbook must run using:
```bash
ansible-playbook -i inventory playbook.yml

User thor should be able to execute the playbook


🔍 Step 1: Verify Inventory File

Navigate to the Ansible directory:
cd /home/thor/ansible


Open the inventory file:
vi inventory


Inventory Content
stapp01 ansible_host=172.16.238.10 ansible_user=tony   ansible_ssh_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve  ansible_ssh_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n


Step 2: Create Ansible Playbook

Create the playbook file:
vi playbook.yml

📄 playbook.yml
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


▶️ Step 3: Execute the Playbook

Run the playbook using the validation command:
ansible-playbook -i inventory playbook.yml


⚠️ Observed Issue (stapp03)
During the first execution, the playbook failed on stapp03 with the following error:
rc: 137
Shared connection closed


🔁 Resolution

The playbook was re-run:
ansible-playbook -i inventory playbook.yml


Why this works

Ansible is idempotent

Hosts that already succeeded are skipped

Failed hosts are retried automatically


✅ Verification

Verify the status of the vsftpd service on all application servers:
ansible all -i inventory -a "systemctl status vsftpd"


Expected result:

Service state: active (running)

Service enabled on boot


📋 Validation Checklist

✅ vsftpd installed on all app servers

✅ vsftpd service started

✅ vsftpd service enabled

✅ Playbook runs without extra arguments

✅ Executable by user thor



🧠 Key Learnings

Automating package installation using Ansible

Managing services with the service module

Understanding Ansible idempotency

Handling transient failures in real-world environments



🏁 Conclusion

This task demonstrates how Ansible can be used to reliably automate service installation and management across multiple servers while maintaining consistency and repeatability.
