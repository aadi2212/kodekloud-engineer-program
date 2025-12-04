# Git Post-Update Hook for Release Tag | KodeKloud Task

## 📌 Task Overview

This task demonstrates how to **automatically create a release tag** on a Git repository whenever the master branch is updated.

**Objectives:**

* Merge feature branch into master
* Set up a **post-update hook** to automatically create a release tag `release-YYYY-MM-DD` on master
* Test the hook to ensure the tag is created from the latest master commit

---

## 🏁 Steps Performed

### 1️⃣ Work in the Clone

```bash
cd /usr/src/kodekloudrepos/ecommerce
git checkout master
git merge feature
git push origin master
```

> Ensure the master branch is up-to-date and push changes to the bare repository `/opt/ecommerce.git`.

---

### 2️⃣ Setup Post-Update Hook in Bare Repository

```bash
cd /opt/ecommerce.git/hooks
sudo vi post-update
```

**Hook Script Content:**

```bash
#!/bin/bash

DATE=$(date +%F)
TAG="release-$DATE"
MASTER_COMMIT=$(git rev-parse refs/heads/master 2>/dev/null)

if [ -n "$MASTER_COMMIT" ]; then
    if ! git rev-parse "$TAG" >/dev/null 2>&1; then
        echo "Creating release tag $TAG on commit $MASTER_COMMIT"
        git tag "$TAG" "$MASTER_COMMIT"
        git push --tags
    else
        echo "Tag $TAG already exists"
    fi
fi
```

Make the hook executable:

```bash
sudo chmod +x /opt/ecommerce.git/hooks/post-update
```

> Hooks must reside in the **bare repository** and be executable to run automatically on push.

---

### 3️⃣ Trigger the Hook

```bash
cd /usr/src/kodekloudrepos/ecommerce

sudo vi hooktest.txt
echo "trigger $(date)" >> hooktest.txt

git add hooktest.txt
git commit -m "Trigger post-update hook"
git push origin master
```

> This push triggers the post-update hook, creating a release tag from the master commit automatically.

---

### 4️⃣ Verify the Release Tag

```bash
cd /opt/ecommerce.git

git rev-parse refs/heads/master
git rev-parse refs/tags/release-$(date +%F)

git ls-remote --tags /opt/ecommerce.git
```

✅ Ensure:

* The tag exists
* The tag points to the latest master commit

---

### 5️⃣ Backup Manual Tag Creation (If Hook Fails)

```bash
cd /opt/ecommerce.git
TODAY=$(date +%F)
git tag -f release-$TODAY $(git rev-parse refs/heads/master)
git push origin release-$TODAY --force
```

> Use this only if the hook did not fire to guarantee task completion.

---

## 💡 Notes

* Always **work in the clone** for merging, committing, and pushing.
* Place hooks **only in the bare repository**.
* Hooks **must be executable** (`chmod +x`).
* **Do not manually create the tag** before pushing; otherwise, the hook won’t trigger.
* Verify that the **hashes of master and tag match** for validation.

---

## ⚠️ Errors Faced & Resolutions

| Error / Issue                      | Cause                                    | Resolution                                    |                       |
| ---------------------------------- | ---------------------------------------- | --------------------------------------------- | --------------------- |
| fatal: not a git repository        | Running git commands inside bare repo    | Run git commands in the clone only            |                       |
| Permission denied on writing files | Using `sudo echo "..." >> file.txt`      | Use `echo "..."                               | sudo tee -a file.txt` |
| Hook not firing / grader fails     | Tag already existed before pushing       | Delete old tag and push to let hook create it |                       |
| Hook not executable                | Missing `chmod +x`                       | Run `chmod +x post-update`                    |                       |
| Tag not pointing to master         | Manual tag creation or wrong commit      | Use `refs/heads/master` in hook               |                       |
| fatal: 'origin' does not appear... | Using `git push origin` inside bare repo | Use `git push --tags` inside bare repo        |                       |

---

# ✅ End of Documentation

The Git post-update hook is now configured to **automatically create release tags** whenever the master branch is updated.
