# Managing ACLs Using Ansible

## 📌 Task Overview

The Nautilus DevOps team needed to automate the creation of specific files across multiple application servers in **Stratos DC** using **Ansible only**.  
Each file must be owned by `root`, while specific users or groups should have controlled access using **Access Control Lists (ACLs)**.

This task demonstrates the use of the **Ansible `acl` module** to manage fine-grained permissions on Linux systems.

---

## 🎯 Objectives

- Create empty files on specific application servers
- Ensure files are owned by the `root` user
- Grant permissions to specific users/groups using ACLs
- Execute the playbook using an existing inventory file without extra arguments

---

## 🖥️ Environment Details

- **Jump Host Directory:** `/home/thor/ansible`
- **Inventory File:** `inventory`
- **Playbook Name:** `playbook.yml`
- **Execution Command:**
  ```bash
  ansible-playbook -i inventory playbook.yml


📂 Server-wise Requirements

| App Server | File Name | Path         | Entity | Etype | Permissions |
| ---------- | --------- | ------------ | ------ | ----- | ----------- |
| stapp01    | blog.txt  | /opt/sysops/ | tony   | group | r           |
| stapp02    | story.txt | /opt/sysops/ | steve  | user  | rw          |
| stapp03    | media.txt | /opt/sysops/ | banner | group | rw          |


📝 Step 1: Verify Inventory File

Location: /home/thor/ansible/inventory

stapp01 ansible_host=172.16.238.10 ansible_user=tony   ansible_ssh_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve  ansible_ssh_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n

✔ Inventory is already present and configured on the Jump Server.


📝 Step 2: Create the Playbook

File: /home/thor/ansible/playbook.yml

Since each application server has different requirements, the playbook is written using three separate plays, one per server.

---
- name: Configure App Server 1
  hosts: stapp01
  become: true
  tasks:
    - name: Create empty file blog.txt
      file:
        path: /opt/sysops/blog.txt
        state: touch
        owner: root
        group: root
        mode: '0644'

    - name: Set ACL for group tony
      acl:
        path: /opt/sysops/blog.txt
        entity: tony
        etype: group
        permissions: r
        state: present


- name: Configure App Server 2
  hosts: stapp02
  become: true
  tasks:
    - name: Create empty file story.txt
      file:
        path: /opt/sysops/story.txt
        state: touch
        owner: root
        group: root
        mode: '0644'

    - name: Set ACL for user steve
      acl:
        path: /opt/sysops/story.txt
        entity: steve
        etype: user
        permissions: rw
        state: present


- name: Configure App Server 3
  hosts: stapp03
  become: true
  tasks:
    - name: Create empty file media.txt
      file:
        path: /opt/sysops/media.txt
        state: touch
        owner: root
        group: root
        mode: '0644'

    - name: Set ACL for group banner
      acl:
        path: /opt/sysops/media.txt
        entity: banner
        etype: group
        permissions: rw
        state: present


▶️ Step 3: Execute the Playbook

Run the following command from /home/thor/ansible:

ansible-playbook -i inventory playbook.yml

✔ The playbook runs successfully without passing extra arguments.



🔍 Step 4: Verification

To verify the ACL permissions on each server:
ansible all -i inventory -a "getfacl /opt/sysops/blog.txt /opt/sysops/story.txt /opt/sysops/media.txt" --become



📊 Verification Notes

Each server contains only its intended file

Errors such as "No such file or directory" for other files are expected

ACL permissions are correctly applied as per requirements


✅ Final Permissions Summary
| Entity | Server  | File      | Permissions |
| ------ | ------- | --------- | ----------- |
| tony   | stapp01 | blog.txt  | r           |
| steve  | stapp02 | story.txt | rw          |
| banner | stapp03 | media.txt | rw          |



🧠 Key Learnings

The Ansible acl module enables fine-grained permission management

Correct usage of entity and etype (user vs group) is crucial

Separate plays improve readability when server requirements differ

Validation failures often occur due to minor permission mismatches



✅ Task Status

✔ Playbook Executed Successfully
✔ ACLs Configured as Required
✔ Validation Passed
