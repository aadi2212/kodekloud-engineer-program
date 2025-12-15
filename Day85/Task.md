# Create Files on App Servers Using Ansible

## 📌 Task: Create Files on App Servers Using Ansible

The Nautilus DevOps team is testing various **Ansible modules** on servers in **Stratos DC**. This task focuses on using the Ansible `file` module to create a blank file on multiple remote hosts while assigning **different ownership per server**.

---

## 🎯 Requirements

* Create an inventory file on the jump host
* Create a playbook to:

  * Create a blank file `/tmp/webapp.txt` on all app servers
  * Set file permissions to **0744**
  * Assign ownership as follows:

| App Server | Owner  | Group  |
| ---------- | ------ | ------ |
| stapp01    | tony   | tony   |
| stapp02    | steve  | steve  |
| stapp03    | banner | banner |

> ⚠️ Validation Command:

```bash
ansible-playbook -i inventory playbook.yml
```

---

## 📂 Project Structure

```bash
playbook/
├── inventory
└── playbook.yml
```

---

## 🧾 Inventory Configuration

**File:** `inventory`

```ini
[app]
stapp01 ansible_user=tony ansible_ssh_password=Ir0nM@n
stapp02 ansible_user=steve ansible_ssh_password=Am3ric@
stapp03 ansible_user=banner ansible_ssh_password=BigGr33n

[all:vars]
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

### 🔹 Notes

* Each server uses a **different SSH user**
* The SSH user is reused as the **file owner**
* Host key checking is disabled to avoid interactive prompts

---

## 📄 Ansible Playbook

**File:** `playbook.yml`

```yaml
- name: Create webapp file with correct permissions and ownership
  hosts: app
  become: true

  tasks:
    - name: Create a blank file /tmp/webapp.txt
      file:
        path: /tmp/webapp.txt
        state: touch
        mode: '0744'
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
```

---

## ▶️ Execution Steps

```bash
cd ~/playbook
ansible-playbook -i inventory playbook.yml
```

---

## 🔍 Verification

Verify the file, permissions, and ownership on all app servers:

```bash
ansible -i inventory app -a "ls -l /tmp/webapp.txt"
```

### ✅ Expected Output

| Server  | File            | Permission | Owner  | Group  |
| ------- | --------------- | ---------- | ------ | ------ |
| stapp01 | /tmp/webapp.txt | 0744       | tony   | tony   |
| stapp02 | /tmp/webapp.txt | 0744       | steve  | steve  |
| stapp03 | /tmp/webapp.txt | 0744       | banner | banner |

---

## 🧠 Key Learnings

* The Ansible `file` module can manage file creation, permissions, and ownership
* Inventory variables allow **host-specific configuration**
* `ansible_user` can be reused when SSH user and file owner are the same
* Always use **spaces instead of tabs** in YAML files

---

## ✅ Status

✔ Task completed successfully

---

### 🔖 Tags

`ansible` `devops` `automation` `infrastructure-as-code` `linux`
