# 🚀 Ansible Blockinfile Module – HTTPD Web Server Setup

## 📌 Task Overview
The Nautilus DevOps team required an Ansible playbook to install and configure a simple **Apache (httpd)** web server on all application servers in the Stratos Datacenter.  
Additionally, a sample web page needed to be deployed using Ansible’s **blockinfile** module.

---

## 🎯 Objectives
- Install **httpd** on all app servers
- Ensure **httpd service** is running and enabled
- Deploy a sample web page using **blockinfile**
- Set correct **ownership and permissions**
- Ensure playbook runs using:
  ```bash
  ansible-playbook -i inventory playbook.yml


🗂️ Directory Structure
ansible/
├── inventory
└── playbook.yml


🖥️ Inventory File
stapp01 ansible_host=172.16.238.10 ansible_user=tony   ansible_ssh_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve  ansible_ssh_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n



Ansible Playbook
playbook.yml

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



▶️ Execution

Run the playbook using:
ansible-playbook -i inventory playbook.yml


✅ Verification
Verify content
ansible all -i inventory -a "cat /var/www/html/index.html"


Verify ownership & permissions
ansible all -i inventory -a "ls -l /var/www/html/index.html"


Expected:
-rw-r-xr-x 1 apache apache ...


📋 Notes

Default markers are used by blockinfile

No extra arguments are required during execution

Task passes validation successfully



🏁 Conclusion

This project demonstrates automated Apache installation and web content deployment using Ansible’s blockinfile module across multiple servers.






