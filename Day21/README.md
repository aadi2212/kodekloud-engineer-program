# Set Up Git Repository on Storage Server – Nautilus Task

## 📌 Task Overview

The Nautilus development team requires a **new Git repository** for an application project on the Storage Server in Stratos DC.

**Objective:**

* Install Git on the Storage Server
* Create a **bare repository** at `/opt/beta.git`

**Environment:**

* Storage Server (Stratos DC)
* User: natasha
* Repository path: `/opt/beta.git`

---

## 🏁 Steps Performed

### 1️⃣ SSH into the Storage Server

```bash
ssh natasha@ststor01
```

---

### 2️⃣ Install Git

```bash
sudo yum install -y git
```

> Verify installation:

```bash
git --version
```

---

### 3️⃣ Create Bare Repository

```bash
sudo mkdir -p /opt/beta.git
sudo git init --bare /opt/beta.git
```

> Expected output:

```
Initialized empty Git repository in /opt/beta.git/
```

---

### 4️⃣ Verify Repository Structure

```bash
ls /opt/beta.git
```

> You should see directories like: `branches`, `hooks`, `info`, `objects`, `refs`

---

## ✅ Final Outcome

1. Git installed on the Storage Server.
2. Bare Git repository created at `/opt/beta.git`.
3. Repository ready for the development team to **clone and push code**.

---

⚡ **Note:** Bare repositories are central repositories shared by teams. They **do not contain a working directory**, only Git history.

---

# End of Documentation
