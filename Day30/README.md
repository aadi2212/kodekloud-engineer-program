# Git Hard Reset – Clean Repository History | KodeKloud Task

## 📌 Task Overview

The Nautilus application development team was testing in the Git repository `/usr/src/kodekloudrepos/demo`. A few unwanted commits were pushed, and the requirement was to **reset the repository** so that only the following two commits remain in the history:

1. Initial commit
2. add data.txt file

The cleaned history also needed to be **pushed to the remote repository**.

---

## 🏁 Steps Performed

### 1️⃣ Navigate to the Repository

```bash
cd /usr/src/kodekloudrepos/demo
```

---

### 2️⃣ Check Commit History

```bash
git log --oneline
```

> Identified the commit hash corresponding to the commit message `add data.txt file`.
> Example: `4998c7d add data.txt file`

---

### 3️⃣ Interactive Rebase to Keep Only Two Commits

```bash
git rebase -i --root
```

> Opens an editor listing all commits starting from the very first one.

**Actions in Editor:**

* Keep only:

```
pick <hash_init> initial commit
pick 4998c7d add data.txt file
```

* Delete (or comment out) all other commits
* Save and exit

> Git rewrites history so only these two commits remain.

---

### 4️⃣ Verify Commit History

```bash
git log --oneline
```

**Expected Output:**

```
4998c7d add data.txt file
<hash_init> initial commit
```

---

### 5️⃣ Force Push to Remote

```bash
git push origin master --force
```

> Force push is required to rewrite remote history and clean the repository completely.

---

## ✅ Final State

1. Only two commits remain:

   * initial commit
   * add data.txt file
2. Remote repository history is rewritten and cleaned.
3. Work tree points to the state from `add data.txt file`.

---

# End of Documentation
