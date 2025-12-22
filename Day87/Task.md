# Ansible Install Package

## Task Overview

The Nautilus Application Development team requested the installation of required packages on application servers in the Stratos Datacenter. Since Ansible is already used for automation, this task focuses on installing the **zip** package across all app servers using an Ansible playbook.

---

## Objectives

* Create an Ansible inventory file with all application servers
* Create an Ansible playbook to install the `zip` package using the `yum` module
* Ensure the playbook can be executed directly by user `thor` without extra arguments

---

## Environment Details

* **Ansible Controller**: Jump Host
* **User**: thor
* **Playbook Directory**: `/home/thor/playbook`
* **Inventory File**: `/home/thor/playbook/inventory`
* **Playbook File**: `/home/thor/playbook/playbook.yml`

### Application Servers

* stapp01
* stapp02
* stapp03

---

## Step 1: Navigate to Playbook Directory

```bash
cd /home/thor/playbook
```

---

## Step 2: Create Inventory File

Create and edit the inventory file:

```bash
vi inventory
```

Add the following content:

```ini
[app]
stapp01 ansible_host=stapp01 ansible_user=tony ansible_ssh_password=Ir0nM@n
stapp02 ansible_host=stapp02 ansible_user=steve ansible_ssh_password=Am3ric@
stapp03 ansible_host=stapp03 ansible_user=banner ansible_ssh_password=BigGr33n

[all:vars]
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

---

## Step 3: Create Ansible Playbook

Create the playbook file:

```bash
vi playbook.yml
```

Playbook content:

```yaml
- name: Install packages on App Servers
  hosts: all
  become: true
  tasks:
    - name: Install zip using yum
      yum:
        name: zip
        state: present
```

> ⚠️ Note: Ensure indentation uses **spaces only**, not tabs.

---

## Step 4: Execute the Playbook

Run the playbook using the inventory file:

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Execution Result

* **stapp01**: `zip` package already installed (no change)
* **stapp02**: `zip` package already installed (no change)
* **stapp03**: `zip` package was missing and successfully installed by Ansible

---

## Final Outcome

* Inventory file correctly configured
* Ansible playbook executed successfully
* Required package installed across all application servers
* System is now ready for application testing

---

## Conclusion

This task demonstrates how Ansible can be used to maintain package consistency across multiple servers efficiently. By using the `yum` module and a centralized inventory, repetitive administrative tasks can be automated reliably in a production-like environment.
