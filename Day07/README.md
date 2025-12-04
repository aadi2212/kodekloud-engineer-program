# Password-less SSH Setup – Jump Host to App Servers (Stratos DC)

## 📌 Task Overview

The system admins of xFusionCorp Industries need **password-less SSH access** for the `thor` user on the Jump Host to all App Servers.

**Objective:**

* Allow `thor` user on Jump Host to SSH into all app servers via respective sudo users (e.g., `tony` on App Server 1) without a password
* Enable automated scripts to run without manual intervention

---

## 🛠 Step-by-Step Execution

### 1️⃣ Log in to Jump Host as thor

```bash
ssh thor@jump_host.stratos.xfusioncorp.com
# password: mjolnir123
```

---

### 2️⃣ Check for Existing SSH Keys

```bash
ls -l ~/.ssh/id_rsa.pub
```

---

### 3️⃣ Generate SSH Key Pair (if not present)

```bash
ssh-keygen -t rsa
```

---

### 4️⃣ Prepare Sudo User Account on App Server

**Example: App Server 1 (sudo user: tony)**

```bash
ssh tony@172.16.238.10
# password: Ir0nM@n
```

* Check if `.ssh` directory exists:

```bash
ls -ld ~/.ssh
```

* If not, create and set permissions:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

---

### 5️⃣ Create the `authorized_keys` File

```bash
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

---

### 6️⃣ Add thor’s Public Key

Append thor’s public key from Jump Host to the sudo user’s `authorized_keys`:

```bash
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDvUR7FRlIPsuaPbSgo14bqJEv1TbmVUi12hL17TEgO97gsPd7GFDQdl5P/1K3p5pwf8J2F7iD0LGFTiO/NsSWqal6APF+nnEFttWLM6+LVWM4DqYo7Z+SLpuNMjqSNzckQ1WJCIGuDnMG9FIrhYjDLgZfiH+dFuA6UszwcHBVjfvjf4NwXJNpFBseEAZPrhK/68GchL+BdN2FB4qVyaJKpMiU7+AK9uo1TI62DMRAYTnWHUNN8lUWh0s1kizpsp3WgQJpX90JZ12h/67vySBSq+t5wMBlksoub/gK7GqdwU1Wjrs+I4oEqK6d5PE3Gm4QJ4yrJVBiXv4pGARddlbYdqMq3PoXc4Hz54SiveRlt0sDH2+EdWC384+WZHVtAGI6yjs6/c2TIRxpye5Eb9fmRDJ3bSDa7OtV1b1f4TXr+EoIDG3uORsBMEZqhqoSRM4488PevUPOjzdmYyf/RQZM321FFTtYSJgu5i5PwwMgObFhnJySnG98ssCgWdKW+W30= thor@jumphost.stratos.xfusioncorp.com" >> ~/.ssh/authorized_keys
```

---

### 7️⃣ Test Password-less SSH

```bash
ssh tony@172.16.238.10
ssh steve@172.16.238.11
ssh banner@172.16.238.12
```

* Login should succeed **without prompting for a password**

---

## ✅ Final Outcome

* `thor` user on Jump Host can SSH into all App Servers via their sudo users without a password
* Automated scripts can now run seamlessly across all servers ✅

# End of Documentation
