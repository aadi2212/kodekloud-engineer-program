# Git Cherry-Pick – Merge Specific Commit from Feature to Master | KodeKloud Task

## 📌 Task Overview

The Nautilus application development team wanted to **merge a specific commit** from the `feature` branch into `master` without merging the entire branch.

**Repository Location:** `/opt/games.git`
**Local Clone:** `/usr/src/kodekloudrepos/games`
**Objective Commit:** `Update info.txt`

**Goal:**

* Cherry-pick the commit `Update info.txt` from `feature` into `master`
* Keep other work-in-progress commits in `feature` untouched
* Push the updated `master` branch to remote

---

## 🏁 Steps Performed

### 1️⃣ SSH into Storage Server

```bash
ssh natasha@ststor01
```

---

### 2️⃣ Navigate to Repository

```bash
cd /usr/src/kodekloudrepos/games
```

---

### 3️⃣ Check Branches

```bash
git branch
# If permission issues, use sudo
```

> Ensure both `master` and `feature` branches are present.

---

### 4️⃣ Find Commit Hash on Feature Branch

```bash
git log feature --oneline
```

Example output:

```
827195a (HEAD -> feature, origin/feature) Update welcome.txt
1bb2733 Update info.txt
9e5fe67 (origin/master, master) Add welcome.txt
f641130 initial commit
```

> Commit to cherry-pick: `1bb2733` (`Update info.txt`)

---

### 5️⃣ Switch to Master Branch

```bash
sudo git checkout master
```

---

### 6️⃣ Cherry-Pick the Commit

```bash
git cherry-pick 1bb2733
```

> **If conflicts occur:**

1. Resolve conflicts manually
2. Stage resolved files:

```bash
git add <file>
```

3. Continue cherry-pick:

```bash
git cherry-pick --continue
```

---

### 7️⃣ Push Master Branch to Remote

```bash
git push origin master
```

---

## ✅ End Result

1. Only the commit `Update info.txt` from `feature` branch is merged into `master`.
2. Other work-in-progress commits in `feature` remain untouched.
3. `master` branch is updated on the remote repository.

---

# End of Documentation
