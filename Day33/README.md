# Git Troubleshooting Task – Resolve Merge Conflicts | Story Blog Repo

## 📌 Task Overview

The Nautilus application development team needed to resolve **merge conflicts** in the `story-blog` repository.

**Objectives:**

1. Fix push issues faced by user `max`.
2. Ensure `story-index.txt` contains titles of all 4 stories.
3. Correct the typo: `"The Lion and the Mooose"` → `"The Lion and the Mouse"`.

**Repository Location:** `/home/max/story-blog`
**Remote Repository:** `http://git.stratos.xfusioncorp.com/sarah/story-blog.git`

---

## 🏁 Steps Performed

### 1️⃣ Login and Navigate to Repository

```bash
ssh max@<storage_server>
cd /home/max/story-blog
```

Check repository status:

```bash
git status
git log --oneline --decorate
```

---

### 2️⃣ Attempt to Push Changes

```bash
git push origin master
```

**Error received:**

```
! [rejected] master -> master (non-fast-forward)
error: failed to push some refs
hint: Updates were rejected because a pushed branch tip is behind its remote
```

**Cause:** Local branch was behind remote master (Sarah had pushed new commits).
**Solution:** Rebase local changes on top of updated remote branch.

---

### 3️⃣ Pull Remote Changes with Rebase

```bash
git pull origin master --rebase
```

**Conflict in `story-index.txt`:**

```text
<<<<<<< HEAD
1. The Lion and the Mooose
...
=======
1. The Lion and the Mouse
...
>>>>>>>
```

---

### 4️⃣ Resolve Conflict Manually

Update `story-index.txt` to include all 4 story titles and fix typo:

```text
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog
```

---

### 5️⃣ Continue Rebase

```bash
git add story-index.txt
git rebase --continue
```

Commit typo fix (if needed):

```bash
git commit -m "Fixed story-index titles and corrected typo in The Lion and the Mouse"
```

---

### 6️⃣ Push Changes Successfully

```bash
git push origin master
# Username: max
# Password: Max_pass123
```

---

### 7️⃣ Verify in Gitea UI

1. Open **Gitea UI** (top bar button).
2. Login as `sarah/Sarah_pass123` or `max/Max_pass123`.
3. Confirm latest commit appears with correct message.
4. Check file contents:

   * `story-index.txt` → lists all 4 stories
   * Story content → shows `"Mouse"` instead of `"Mooose"`

---

## ⚠️ Issues Faced & Resolution

| Issue                               | Cause                                           | Resolution                                           |
| ----------------------------------- | ----------------------------------------------- | ---------------------------------------------------- |
| Push failed due to non-fast-forward | Local branch behind remote                      | `git pull origin master --rebase`                    |
| Merge conflict in `story-index.txt` | Rebase applied changes on top of remote commits | Manual conflict resolution, staged, continued rebase |

---

## ✅ Final Outcome

1. `story-index.txt` contains all 4 story titles.
2. Typo `"Mooose"` corrected to `"Mouse"`.
3. Changes successfully pushed as user `Max`.
4. Verified in Gitea UI.

---

# End of Documentation
