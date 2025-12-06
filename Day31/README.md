# Git Stash – Restore stash@{1} | KodeKloud Task

## 📌 Task Overview

The Nautilus application development team had **stashed some in-progress changes** in the repository `/usr/src/kodekloudrepos/beta`.

**Objective:**

* Restore the stash identified as `stash@{1}`
* Commit the restored changes
* Push the changes to the remote repository

**Repository Location:** `/usr/src/kodekloudrepos/beta`

---

## 🏁 Steps Performed

### 1️⃣ Navigate to the Repository

```bash
cd /usr/src/kodekloudrepos/beta
```

---

### 2️⃣ List Available Stashes

```bash
git stash list
```

**Example Output:**

```
stash@{0}: WIP on master: 98c5590 initial commit
stash@{1}: WIP on master: 98c5590 initial commit
```

> Confirmed that `stash@{1}` exists.

---

### 3️⃣ Apply the Stash

```bash
git stash apply stash@{1}
```

> Restored the changes into the working directory.
> Note: The stash remains in the list because `apply` was used instead of `pop`.

---

### 4️⃣ Verify Changes

```bash
git status
git diff
```

> Confirmed that files from `stash@{1}` appeared as modified or added.

---

### 5️⃣ Stage and Commit Changes

```bash
git add .
git commit -m "Restored changes from stash@{1}"
```

---

### 6️⃣ Push Changes to Remote

```bash
git push origin master
```

---

## ✅ Final State

1. The stash `stash@{1}` was successfully restored.
2. Changes were committed with the message: `"Restored changes from stash@{1}"`.
3. Remote repository is fully updated with the restored changes.

---

# End of Documentation
