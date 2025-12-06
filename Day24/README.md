# Git Create Branch – News Repo | KodeKloud Task

## 📌 Task Overview

The Nautilus development team required a **new branch** for implementing upcoming features while keeping the `master` branch intact.

**Repository Location:** `/usr/src/kodekloudrepos/news`

**Objective:**

* Create a new branch `xfusioncorp_news` from `master`
* Do **not** modify any files
* Ensure repository is trusted to avoid ownership-related errors

---

## 🏁 Steps Performed

### 1️⃣ Log in to Storage Server

```bash
ssh natasha@172.16.238.15
```

---

### 2️⃣ Switch to Natasha User (if needed)

```bash
sudo su - natasha
```

---

### 3️⃣ Navigate to Repository

```bash
cd /usr/src/kodekloudrepos/news
```

---

### 4️⃣ Check Git Status and Branch

```bash
git status
git branch
```

> Error faced:

```
fatal: detected dubious ownership in repository at '/usr/src/kodekloudrepos/news'
```

✅ **Fix:** Mark the repository as safe

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/news
```

> Re-run `git status` and `git branch` to confirm no errors.

---

### 5️⃣ Fetch Latest Updates

```bash
git fetch --all
```

---

### 6️⃣ Create and Switch to New Branch

```bash
git checkout -b xfusioncorp_news master
```

---

### 7️⃣ Verify New Branch

```bash
git branch
```

Expected output:

```
* xfusioncorp_news
  master
```

---

## ✅ Key Notes

* **Do not edit any files.**
* The task only requires creating a branch, **no push is needed** unless explicitly instructed.
* Repository trust must be configured to avoid ownership errors.

---

# End of Documentation
