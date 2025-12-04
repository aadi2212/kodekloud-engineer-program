# Git Merge Branches – News Repo | KodeKloud Task

## 📌 Task Overview

The Nautilus application development team required the following operations on the repository `/opt/news.git` (cloned at `/usr/src/kodekloudrepos/news`):

**Objective:**

* Create a new branch `xfusion` from `master`
* Copy `/tmp/index.html` into the new branch and commit it
* Merge the `xfusion` branch back into `master`
* Push changes for both branches to the remote

---

## 🏁 Steps Performed

### 1️⃣ Log in to Storage Server

```bash
ssh natasha@172.16.238.15
```

---

### 2️⃣ Navigate to Repository

```bash
sudo cd /usr/src/kodekloudrepos/news
```

---

### 3️⃣ Ensure Repository is Trusted

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/news
```

---

### 4️⃣ Switch to Master Branch and Update

```bash
sudo git checkout master
sudo git pull origin master
```

---

### 5️⃣ Create and Switch to New Branch `xfusion`

```bash
git checkout -b xfusion
```

---

### 6️⃣ Copy File into Repository

```bash
sudo cp /tmp/index.html .
ls -l index.html
```

---

### 7️⃣ Stage and Commit Changes

```bash
sudo git add index.html
sudo git commit -m "Added index.html file in xfusion branch"
```

---

### 8️⃣ Push New Branch to Remote

```bash
git push origin xfusion
```

---

### 9️⃣ Switch Back to Master Branch

```bash
git checkout master
```

---

### 10️⃣ Merge `xfusion` Branch into Master

```bash
git merge xfusion
```

---

### 11️⃣ Push Updated Master to Remote

```bash
git push origin master
```

---

## ✅ Final Verification

Check branches:

```bash
git branch -a
```

Expected output:

```
* master
  xfusion
  remotes/origin/master
  remotes/origin/xfusion
```

---

# End of Documentation
