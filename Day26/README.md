# Git Manage Remotes – Ecommerce Repo | KodeKloud Task

## 📌 Task Overview

The xFusionCorp development team updated the project repository `/opt/ecommerce.git` and cloned it locally under `/usr/src/kodekloudrepos/ecommerce`.

**Objective:**

* Add a new remote `dev_ecommerce`
* Copy and commit `/tmp/index.html` to the repository
* Push the changes to the new remote

**Repository Location (Local Clone):** `/usr/src/kodekloudrepos/ecommerce`
**New Remote:** `dev_ecommerce → /opt/xfusioncorp_ecommerce.git`
**Branch:** `master`

---

## 🏁 Steps Performed

### 1️⃣ SSH into Storage Server

```bash
ssh natasha@ststor01
```

---

### 2️⃣ Navigate to Repository

```bash
cd /usr/src/kodekloudrepos/ecommerce
```

---

### 3️⃣ Add New Remote

**Initial Error:** Typo in remote name (`dev_ecommerece`)

**Fix:**

```bash
git remote remove dev_ecommerece
git remote add dev_ecommerce /opt/xfusioncorp_ecommerce.git
git remote -v
```

**Expected Output:**

```
dev_ecommerce  /opt/xfusioncorp_ecommerce.git (fetch)
dev_ecommerce  /opt/xfusioncorp_ecommerce.git (push)
origin         /opt/ecommerce.git (fetch)
origin         /opt/ecommerce.git (push)
```

---

### 4️⃣ Copy File and Commit Changes

```bash
cp /tmp/index.html .
git add index.html
git commit -m "Add index.html file"
```

---

### 5️⃣ Resolve Safe Directory / Permission Issue

**Error Faced:**

```
fatal: detected dubious ownership in repository at '/usr/src/kodekloudrepos/ecommerce'
```

**Resolution:**

* Attempted:

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/ecommerce
```

* Final resolution: Used `sudo` for push:

```bash
sudo git push dev_ecommerce master
```

---

## ✅ Final Outcome

1. `index.html` added and committed to the `master` branch.
2. Changes successfully pushed to new remote `dev_ecommerce` (`/opt/xfusioncorp_ecommerce.git`).
3. Task completed successfully.

---

# End of Documenta
