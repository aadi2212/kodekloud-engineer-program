# Ansible Ping Module Usage

## Task Objective

The Nautilus DevOps team needs to verify Ansible connectivity to application servers before executing playbooks. This task focuses on setting up **passwordless SSH authentication** between the Ansible controller (Jump Host) and managed nodes, followed by validating connectivity using the **Ansible Ping module**.

---

## Environment Details

* **Ansible Controller**: Jump Host
* **Controller User**: `thor`
* **Inventory File**: `/home/thor/ansible/inventory`
* **Managed Nodes**:

  * App Server 1 → `stapp01`
  * App Server 2 → `stapp02`
  * App Server 3 → `stapp03`

---

## Step 1: Generate SSH Key Pair (on Jump Host)

Login as user `thor` on the jump host and generate an SSH key pair.

```bash
ssh-keygen -t rsa -b 4096
```

* Press **Enter** to accept the default file location
* Leave the **passphrase empty** to enable passwordless login

---

## Step 2: Copy Public Key to App Servers

Copy the generated public key to each app server using `ssh-copy-id`.

```bash
ssh-copy-id tony@stapp01
# Password: Ir0nM@n

ssh-copy-id steve@stapp02
# Password: Am3ric@

ssh-copy-id banner@stapp03
# Password: BigGr33n
```

After this step, SSH access should not prompt for a password.

### Verification Example

```bash
ssh banner@stapp03
```

You should be logged in without entering a password.

---

## Step 3: Update Ansible Inventory File

Navigate to the Ansible directory and edit the inventory file.

```bash
cd /home/thor/ansible
vi inventory
```

Update the inventory to use **SSH key-based authentication** by specifying `ansible_user` and removing any `ansible_ssh_pass` entries.

```ini
stapp01 ansible_host=172.16.238.10 ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_user=banner
```

---

## Step 4: Test Connectivity Using Ansible Ping Module

### Ping All App Servers

```bash
ansible all -i inventory -m ping
```

Expected result:

```text
SUCCESS => {"ping": "pong"}
```

### Ping Only App Server 1

```bash
ansible stapp01 -i inventory -m ping
```

---

## Final Outcome

* Passwordless SSH connectivity successfully configured
* Inventory file updated for key-based authentication
* Ansible Ping module confirms reachability of app servers

---

## Conclusion

This task ensures that the Ansible controller can securely and efficiently communicate with managed nodes. Successful ping results confirm the environment is ready for executing Ansible playbooks.
