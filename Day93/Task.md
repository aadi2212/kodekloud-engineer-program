# Using Ansible Conditionals

## 📌 Task Overview
The Nautilus DevOps team wanted to demonstrate how **Ansible conditionals (`when`)** can be used to perform **different actions on different servers** using a **single playbook**.

In this task, different files are copied to different application servers based on the host on which the playbook is running.

---

## 🎯 Objectives
- Use Ansible `when` conditionals
- Run the play on **all hosts**
- Copy specific files to specific servers
- Apply correct ownership and permissions
- Use `ansible_nodename` from gathered facts

---

## 📂 Inventory Details

### Inventory File Location

/home/thor/ansible/inventory


### Inventory Content


stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_ssh_pass=Ir0nM@n
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_ssh_pass=Am3ric@
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_ssh_pass=BigGr33n


---

## 📋 Task Requirements

| Server  | Source File              | Destination Path       | Owner  | Permissions |
|--------|--------------------------|------------------------|--------|-------------|
| stapp01 | `/usr/src/data/blog.txt`  | `/opt/data/blog.txt`   | tony   | `0744`      |
| stapp02 | `/usr/src/data/story.txt` | `/opt/data/story.txt` | steve  | `0744`      |
| stapp03 | `/usr/src/data/media.txt` | `/opt/data/media.txt` | banner | `0744`      |

---

## 📝 Playbook Implementation

### Playbook Location


/home/thor/ansible/playbook.yml


### `playbook.yml`
```yaml
---
- name: Copy files using conditionals
  hosts: all
  become: yes

  tasks:
    - name: Show node name for debugging
      debug:
        msg: "Nodename is: {{ ansible_nodename }}"

    - name: Copy blog.txt to App Server 1
      copy:
        src: /usr/src/data/blog.txt
        dest: /opt/data/blog.txt
        owner: tony
        group: tony
        mode: '0744'
      when: ansible_nodename == "stapp01.stratos.xfusioncorp.com"

    - name: Copy story.txt to App Server 2
      copy:
        src: /usr/src/data/story.txt
        dest: /opt/data/story.txt
        owner: steve
        group: steve
        mode: '0744'
      when: ansible_nodename == "stapp02.stratos.xfusioncorp.com"

    - name: Copy media.txt to App Server 3
      copy:
        src: /usr/src/data/media.txt
        dest: /opt/data/media.txt
        owner: banner
        group: banner
        mode: '0744'
      when: ansible_nodename == "stapp03.stratos.xfusioncorp.com"

▶️ Execute the Playbook
ansible-playbook -i inventory playbook.yml

🔍 Expected Behavior

Play runs on all servers

Only the task matching the when condition executes

Non-matching tasks are skipped

Correct file is copied to the correct server

✅ Verification
Verify files on all servers
ansible -i inventory all -a "ls -l /opt/data"

📌 Expected Results

blog.txt exists only on stapp01

story.txt exists only on stapp02

media.txt exists only on stapp03

Permissions set to 0744

Correct owner and group assigned

🧠 Key Concepts Used

Ansible when conditionals

ansible_nodename

copy module

Fact-based decision making

Single playbook, multiple conditions

🏁 Conclusion

This task demonstrates how Ansible conditionals can be effectively used to manage host-specific automation while keeping the playbook clean, reusable, and scalable.

✨ Task Completed Successfully
