# Clone Git Repository on Storage Server – Nautilus Task

## 📌 Task Overview

The Nautilus development team requires a **local copy of an existing Git repository** on the Storage Server in Stratos DC.

**Objective:**

* Clone `/opt/official.git` repository to `/usr/src/kodekloudrepos`
* Ensure no modifications are made to repository or directories
* Perform task using `natasha` user

**Environment:**

* Storage Server (Stratos DC)
* User: natasha
* Source repository: `/opt/official.git`
* Target directory: `/usr/src/kodekloudrepos`

---

## 🏁 Steps Performed

### 1️⃣ Log in to the Storage Server

```bash
ssh natasha@172.16.238.15
```

---

### 2️⃣ Navigate to the Target Directory

```bash
cd /usr/src/kodekloudrepos
```

> Ensure you are in the correct directory before cloning.

---

### 3️⃣ Clone the Repository

```bash
git clone /opt/official.git
```

> This will create a directory named `official` inside `/usr/src/kodekloudrepos`.

---

### 4️⃣ Verify the Cloned Repository

```bash
ls -ld /usr/src/kodekloudrepos/official
cd /usr/src/kodekloudrepos/official
git status
```

* Expected output: Clean working directory, no modifications.

---

## ✅ Important Notes

1. Always run clone as **natasha**, not root.
2. Do not change ownership or permissions of `/usr/src/kodekloudrepos` or `/opt/official.git`.
3. Only clone the repository; no other operations are required.

---

# End of Documentation
