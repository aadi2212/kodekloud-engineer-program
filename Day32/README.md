# Git Rebase – Feature Branch with Master | KodeKloud Task

## 📌 Task Overview

The Nautilus application development team needed to **rebase a feature branch onto the master branch** without losing any data and **without creating merge commits**.

**Repository Location:** `/opt/official.git`
**Local Clone:** `/usr/src/kodekloudrepos/official`

**Objective:**

* Rebase the feature branch with master
* Preserve all commits from the feature branch
* Avoid merge commits
* Push the updated feature branch to remote

---

## 🏁 Steps Performed

### 1️⃣ Navigate to the Repository

```bash
cd /usr/src/kodekloudrepos/official
```

### 2️⃣ Check Existing Branches

```bash
git branch -a
```

> Confirmed both `master` and `feature` branches exist.

---

### 3️⃣ Update Local Master Branch

```bash
git checkout master
git pull origin master
```

> Ensures local master is up-to-date with remote.

---

### 4️⃣ Switch to Feature Branch

```bash
git checkout feature
```

---

### 5️⃣ Rebase Feature onto Master

```bash
git rebase master
```

> Re-applies commits from the feature branch on top of the updated master branch.

**Handling Conflicts (if any):**

1. Resolve conflicts manually in the affected files
2. Stage resolved files:

```bash
git add <file>
```

3. Continue the rebase:

```bash
git rebase --continue
```

---

### 6️⃣ Verify Commit History

```bash
git log --oneline --graph --all
```

Example output:

```
* 5f0b0f1 (HEAD -> feature) Add new feature
* 481a103 (origin/master, master) Update info.txt
| * e5b1e26 (origin/feature) Add new feature
|/
* c435c9a initial commit
```

> Confirmed that feature commits are now on top of master with **no merge commits** introduced.

---

### 7️⃣ Push Rebases Feature Branch to Remote

```bash
git push origin feature --force
```

> Force push is required because rebasing rewrites commit history.

---

## ✅ Final State

1. Feature branch is fully rebased on top of the latest master.
2. Developer’s changes are intact.
3. Git history is clean with no merge commits.
4. Remote repository updated successfully.

---

# End of Documentation
