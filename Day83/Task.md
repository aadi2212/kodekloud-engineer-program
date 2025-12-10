# 🧩 Troubleshoot and Create Ansible Playbook

## 📌 Task Objective
Fix the Ansible inventory and create a playbook that runs on **App Server 2** in Stratos DC, ensuring it successfully creates an empty file at:

/tmp/file.txt


Validation will be performed using:

```bash
ansible-playbook -i inventory playbook.yml

Your setup must work without extra arguments.


🗂️ 1. Navigate to the Ansible Directory
cd /home/thor/ansible


2. Update the Inventory File
Open the inventory file:
vim inventory

Update it to target App Server 2:
stapp02 ansible_host=stapp02 ansible_user=steve ansible_ssh_password=Am3ric@ ansible_ssh_common_args='-o StrictHostKeyChecking=no'

This ensures Ansible can connect to App Server 2 correctly.


📄 3. Create the Playbook File
Create playbook.yml:
vim playbook.yml


Add the following playbook:
- name: Configure App Server 2                 # Human-readable title for the play
  hosts: stapp02                               # Target specific host
  tasks:                                       # List of tasks to run
    - name: Create an empty file at /tmp/file.txt   # Task description
      file:                                    # Using Ansible's file module
        path: /tmp/file.txt                    # Destination path
        state: touch                           # Ensures file exists (empty file)



▶️ 4. Run and Verify the Playbook
Execute:
ansible-playbook -i inventory playbook.yml

If everything is correct, it will connect to App Server 2 and create the empty file.



✅ Result

✔ Inventory corrected

✔ Playbook created

✔ /tmp/file.txt successfully created on App Server 2



