# Managing ACLs Using Ansible

## 🛠️ Step-by-Step Execution

---

### 1️⃣ Log in to Jump Host as `thor`

```bash
ssh thor@jump_host.stratos.xfusioncorp.com
# password: ********

2️⃣ Navigate to Ansible Directory
cd /home/thor/ansible

3️⃣ Verify Inventory File

Check that the inventory file already exists and contains all app servers.

vi inventory

stapp01 ansible_host=172.16.238.10 ansible_user=tony   ansible_ssh_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve  ansible_ssh_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n


✔ Inventory file verified.

4️⃣ Create Ansible Playbook

Create the playbook file as per task requirements.

vi playbook.yml

5️⃣ Add Playbook Configuration

Since each application server has unique requirements, three separate plays are used.

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

6️⃣ Execute the Playbook

Run the playbook using the existing inventory file.

ansible-playbook -i inventory playbook.yml


✔ Playbook executed successfully.

7️⃣ Verify ACL Permissions

Verify ACL permissions using getfacl.

ansible all -i inventory -a "getfacl /opt/sysops/blog.txt /opt/sysops/story.txt /opt/sysops/media.txt" --become

8️⃣ Verification Notes

Each server contains only its respective file

Errors like "No such file or directory" for other files are expected

ACL permissions are correctly applied as per requirements

✅ Final Permission Summary
Server	File	Entity	Type	Permissions
stapp01	blog.txt	tony	group	r
stapp02	story.txt	steve	user	rw
stapp03	media.txt	banner	group	rw
🎯 Task Outcome

✔ Files created successfully
✔ Ownership set to root
✔ ACL permissions applied correctly
✔ Task validated successfully
