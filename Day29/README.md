# Git Pull Request Workflow – Max’s Story | KodeKloud Task

## 📌 Task Overview

The goal of this task was to ensure that **Max’s story** is merged into the `master` branch **only after review**, following a proper Pull Request (PR) workflow. Direct pushes to `master` are restricted to maintain code quality.

**Repository Path:** `~/<repo-name>` (already cloned)
**Remote Git host:** Gitea
**Branch containing new story:** `story/fox-and-grapes`
**Target branch:** `master`

**Users Involved:**

* **Author:** Max (`Max_pass123`)
* **Reviewer:** Tom (`Tom_pass123`)

---

## 🏁 Steps Performed

### 1️⃣ SSH and Verify Repository

```bash
ssh max@<storage-server-ip>
# Password: Max_pass123
```

Navigate to the cloned repository:

```bash
cd ~/<repo-name>
```

Check current branch:

```bash
git branch
# Expected: * story/fox-and-grapes
```

Verify commits in the branch:

```bash
git log --oneline --decorate
```

**Observation:**

```
b61085f (HEAD -> story/fox-and-grapes, origin/story/fox-and-grapes) Added fox-and-grapes story
3d8d862 (origin/master, origin/HEAD, master) Merge branch 'story/frogs-and-ox'
40e4b4b Fix typo in story title
18ce805 Completed frogs-and-ox story
08d9cd1 Added the lion and mouse story
2ab2ecd Add incomplete frogs-and-ox story
```

* Max’s story commit: `b61085f` on `story/fox-and-grapes`.
* Master branch contains previously approved stories.

---

### 2️⃣ Create Pull Request (PR) via Gitea UI

1. Open the repository in Gitea UI.

2. Login as Max:

   * Username: `max`
   * Password: `Max_pass123`

3. Click **New Pull Request**.

4. Configure PR:

   * **Title:** Added fox-and-grapes story
   * **Source branch:** `story/fox-and-grapes`
   * **Destination branch:** `master`

5. Click **Create PR**.

---

### 3️⃣ Assign Reviewer

1. On the PR page, locate **Reviewers** on the right.
2. Add **Tom** as a reviewer.

---

### 4️⃣ Review and Merge PR as Tom

1. Log out as Max.

2. Login as Tom:

   * Username: `tom`
   * Password: `Tom_pass123`

3. Open the PR **Added fox-and-grapes story**.

4. Review changes and commit history.

5. Click **Approve** → Merge the PR into `master`.

---

## ✅ Outcome

1. Max’s story is safely merged into `master`.
2. Master branch contains only reviewed and approved content.
3. Proper Pull Request workflow enforced, preventing direct pushes to master.

---

# End of Documentation
