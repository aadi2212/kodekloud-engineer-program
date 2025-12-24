# Managing Jinja2 Templates Using Ansible (HTTPD Role)

This project demonstrates how to manage **Jinja2 templates** using **Ansible roles** to dynamically configure an Apache (`httpd`) web server.  
The setup installs `httpd`, deploys a dynamic `index.html` file using a Jinja2 template, and applies correct permissions and ownership on **App Server 3 (stapp03)**.

---

## 🚀 Features

- Uses **Ansible roles** for reusable automation
- Dynamically generates `index.html` using **Jinja2 templates**
- Avoids hardcoding hostnames using `inventory_hostname`
- Sets proper file permissions and ownership
- Validated using `ansible-playbook` command

---

## 📁 Project Structure

ansible/
├── inventory
├── playbook.yml
└── role/
└── httpd/
├── tasks/
│ └── main.yml
└── templates/
└── index.html.j2


---

## 📌 Prerequisites

- Ansible installed on the control node
- Inventory file already configured (`~/ansible/inventory`)
- SSH access to managed nodes
- Sudo privileges on target servers

---

## 🧩 Jinja2 Template

**File:** `role/httpd/templates/index.html.j2`

```jinja2
This file was created using Ansible on {{ inventory_hostname }}


This template dynamically displays the hostname of the target server.

🧩 HTTPD Role Tasks

File: role/httpd/tasks/main.yml

---
# Tasks for httpd role

- name: Install latest version of httpd
  yum:
    name: httpd
    state: latest

- name: Start and enable httpd service
  service:
    name: httpd
    state: started
    enabled: yes

- name: Deploy index.html using Jinja2 template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
    mode: '0755'
    owner: "{{ ansible_user }}"
    group: "{{ ansible_user }}"

🧩 Playbook Configuration

File: playbook.yml

---
- hosts: stapp03
  become: yes
  roles:
    - role/httpd


This playbook applies the httpd role only to App Server 3.

▶️ Execution

Run the playbook using:

ansible-playbook -i inventory playbook.yml

✅ Verification
Check index.html content
ansible -i inventory stapp03 -a "cat /var/www/html/index.html"


Expected output:

This file was created using Ansible on stapp03

Check permissions and ownership
ansible -i inventory stapp03 -a "ls -l /var/www/html/index.html"


Expected:

Permission: 0755

Owner & Group: respective sudo user of the server

🧠 Key Concepts Used

Ansible Roles

Jinja2 Templates

Inventory Variables (inventory_hostname)

Idempotent Configuration Management

Linux File Permissions & Ownership

📌 Use Case

This setup is useful for:

Automating web server deployments

Managing dynamic configuration files

Learning real-world Ansible role structure

DevOps interview preparation
