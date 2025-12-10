# 📂 Copy Data to Application Servers Using Ansible

## 📌 Task Objective
Use Ansible to copy a file from the jump host to all application servers in Stratos DC.  

- Source file: `/usr/src/itadmin/index.html`  
- Destination: `/opt/itadmin/index.html` on all application servers  
- Inventory and playbook must work without extra arguments  

Validation command:

```bash
ansible-playbook -i inventory playbook.yml


🗂️ 1. Navigate to the Ansible Directory
cd /home/thor/ansible


🛠️ 2. Create the Inventory File
vim inventory


Add the following:
[app]
stapp01 ansible_user=tony ansible_ssh_password=Ir0nM@n
stapp02 ansible_user=steve ansible_ssh_password=Am3ric@
stapp03 ansible_user=banner ansible_ssh_password=BigGr33n

[all:vars]
ansible_ssh_common_args='-o StrictHostKeyChecking=no'

This defines all application servers and disables strict host key checking.



📄 3. Create the Playbook
vim playbook.yml


Add the following content (make sure no tabs, only spaces):
- name: Copy data to application servers
  hosts: app
  become: yes
  tasks:
    - name: Copy index.html to /opt/itadmin
      copy:
        src: /usr/src/itadmin/index.html
        dest: /opt/itadmin/index.html
        mode: '0644'



✔️ 4. Validate the Playbook Syntax
ansible-playbook -i inventory playbook.yml --syntax-check


▶️ 5. Run and Verify the Playbook
ansible-playbook -i inventory playbook.yml

✅ Result: The file /opt/itadmin/index.html should now exist on all application servers.


