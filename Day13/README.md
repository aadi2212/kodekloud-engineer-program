# Iptables Installation & Apache Port Security – Stratos DC

## 📌 Task Overview

The security team has raised concerns that **Apache port 5002** is currently open to all traffic. To comply with security standards, a firewall must be implemented.

**Objective:**

* Install iptables on all app servers
* Restrict access to Apache port 5002 only to the Load Balancer (LBR) host
* Ensure firewall rules persist across reboots

---

## 🛠 Step-by-Step Execution

### 1️⃣ SSH into App Servers

```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

---

### 2️⃣ Install iptables

```bash
sudo yum install -y iptables iptables-services
sudo systemctl enable iptables
sudo systemctl start iptables
```

---

### 3️⃣ Configure Firewall Rules

Flush old rules (optional but recommended):

```bash
sudo iptables -F
```

Allow Apache port 5002 only from LBR host (172.16.238.14):

```bash
sudo iptables -A INPUT -p tcp -s 172.16.238.14 --dport 5002 -j ACCEPT
```

Block port 5002 from all other sources:

```bash
sudo iptables -A INPUT -p tcp --dport 5002 -j DROP
```

Allow existing and related connections:

```bash
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

Allow loopback interface:

```bash
sudo iptables -A INPUT -i lo -j ACCEPT
```

Allow SSH access:

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

Set default policy to drop all other traffic:

```bash
sudo iptables -P INPUT DROP
```

---

### 4️⃣ Persist Rules Across Reboot

```bash
sudo service iptables save
sudo cat /etc/sysconfig/iptables
```

---

### 5️⃣ Verify Firewall Rules

```bash
sudo iptables -L -n
```

**Expected Output (relevant lines):**

```
ACCEPT     tcp  --  172.16.238.14       0.0.0.0/0            tcp dpt:5002
DROP       tcp  --  0.0.0.0/0           0.0.0.0/0            tcp dpt:5002
```

---

### 6️⃣ Test Access

From App Server:

```bash
curl http://172.16.238.10:5002
```

From Jump Host (should fail):

```bash
curl http://172.16.238.10:5002
```

---

## ✅ Final Outcome

* Iptables installed and active on all app servers
* Apache port 5002 accessible only from LBR host
* Firewall rules persist after system reboot
* Security compliance achieved ✅

# End of Documentation
