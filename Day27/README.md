# Git Revert – Revert Latest Commit | KodeKloud Task

## 📌 Task Overview

The Nautilus application development team reported an issue with recent commits in the repository `/usr/src/kodekloudrepos/demo`.

**Objective:**

* Revert the latest commit (`HEAD`) to the previous commit
* Create a new revert commit with the message: `revert demo message`
* Preserve commit history while undoing the changes

---

## 🏁 Steps Performed

### 1️⃣ SSH into Storage Server

```bash
ssh natasha@ststor01
```

---

### 2️⃣ Navigate to Repository

```bash
cd /usr/src/kodekloudrepos/demo
```

---

### 3️⃣ Check Commit History

```bash
sudo git log --oneline
```

Example output:

```
1393c4e (HEAD -> master, origin/master) add data.txt file
13be74c initial commit
```

> Latest commit to revert: `1393c4e` (`add data.txt file`)

---

### 4️⃣ Revert the Latest Commit

#### Option A – Interactive (editor opens)

```bash
git revert HEAD
```

* Replace the default commit message with:

```
revert demo message
```

* Save and exit editor

#### Option B – Non-Interactive (one-shot)

```bash
git revert -n HEAD
git commit -m "revert demo message"
```

> If repository permissions require it, prefix commands with `sudo`.

---

### 5️⃣ Verify New Revert Commit

```bash
git log --oneline -n 3
```

Expected output:

```
66a9c61 (HEAD -> master) revert demo message
1393c4e (origin/master) add data.txt file
13be74c initial commit
```

> The top commit should now be `revert demo message`.

---

## ✅ End Result

1. Latest commit is reverted successfully.
2. New commit `revert demo message` is created to undo the previous changes.
3. Repository history is maintained, and the repo points back to the state of the previous commit.

---

# End of Documentation
