# Managing Jinja2 Templates Using Ansible

## 📌 Task Overview
One of the Nautilus DevOps team members is developing an Ansible role for **httpd installation and configuration**.  
The requirement is to extend the role by adding a **Jinja2 template** for the `index.html` file and update the playbook to run this role on **App Server 3**.

The implementation must:
- Use a dynamic template (no hardcoded server names)
- Apply correct file permissions and ownership
- Run successfully using:
  ```bash
  ansible-playbook -i inventory playbook.yml

🎯 Objectives

Update the Ansible playbook to run the httpd role on stapp03

Create a Jinja2 template using inventory_hostname

Copy the rendered template to /var/www/html/index.html

Set permissions to 0755

Set owner and group to the respective sudo user of the server

📂 Directory Structure
/home/thor/ansible/
├── inventory
├── playbook.yml
└── role/
    └── httpd/
        ├── tasks/
        │   └── main.yml
        └── templates/
            └── index.html.j2

📝 Step 1: Create Jinja2 Template
Navigate to templates directory
cd /home/thor/ansible/role/httpd/templates

Create template file
vi index.html.j2

Template content
This file was created using Ansible on {{ inventory_hostname }}

Explanation

inventory_hostname dynamically resolves the server name

Avoids hardcoding hostnames

Makes the role reusable across multiple servers

🛠 Step 2: Update httpd Role Tasks
Navigate to tasks directory
cd /home/thor/ansible/role/httpd/tasks

Edit main.yml
vi main.yml

Final main.yml
---
# tasks file for httpd role

- name: Install the latest version of HTTPD
  yum:
    name: httpd
    state: latest

- name: Start httpd service
  service:
    name: httpd
    state: started

- name: Copy index.html template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
    mode: '0755'
    owner: "{{ ansible_user }}"
    group: "{{ ansible_user }}"

Key Points

Uses template module to render Jinja2 file

ansible_user sets correct owner and group dynamically

File permissions set as required

▶️ Step 3: Update Playbook
Navigate to ansible directory
cd /home/thor/ansible

Edit playbook.yml
vi playbook.yml

Playbook content
---
- hosts: stapp03
  become: yes
  roles:
    - role/httpd

🚀 Step 4: Execute the Playbook
ansible-playbook -i inventory playbook.yml

🔎 Step 5: Verification
Verify file content
ansible -i inventory stapp03 -a "cat /var/www/html/index.html"

Verify ownership and permissions
ansible -i inventory stapp03 -a "ls -l /var/www/html/index.html"

✅ Expected Result

File content:

This file was created using Ansible on stapp03


Permissions:

-rwxr-xr-x


Owner and group:

sudo user of stapp03

🧠 Key Takeaways

Jinja2 templates enable dynamic and reusable configurations

Ansible roles help standardize and scale automation

Avoid hardcoding values by using built-in Ansible variables

Proper permissions and ownership are critical for web services
