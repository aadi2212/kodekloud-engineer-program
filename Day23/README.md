# Fork a Git Repository – Jon | KodeKloud Task

## 📌 Task Overview

A new developer, Jon, needs to **fork an existing repository** to start working on a project. The repository to be forked is `sarah/story-blog`.

**Objective:**

* Fork `sarah/story-blog` under Jon's account
* Verify that the forked repository is accessible under Jon's account

**Environment:**

* Git Web UI: Gitea
* Username: jon
* Password: Jon_pass123

---

## 🏁 Steps Performed

### 1️⃣ Access Gitea Web UI

* Click on the **Gitea UI** button in the top bar of the lab environment/dashboard.
* This opens the Gitea web interface.

---

### 2️⃣ Login as Jon

```text
Username: jon
Password: Jon_pass123
```

* Click **Sign In** to access Jon’s Gitea account.

---

### 3️⃣ Locate the Repository

* Use the search bar (top-right) and search for:

```
sarah/story-blog
```

* Open the repository from the search results.

---

### 4️⃣ Fork the Repository

1. On the repository page, click the **Fork** button (top-right).
2. Select **jon** as the destination account.

✅ After completion, a new repository is created:

```
jon/story-blog
```

---

### 5️⃣ Verify the Fork

* You should be redirected to the forked repository under Jon's account.
* Confirm that the repository URL is:

```
http://<gitea-url>/jon/story-blog
```

* Ensure that all files and commit history from the original repository are present.

---

## ✅ Final Outcome

1. Repository `sarah/story-blog` successfully forked under Jon’s account as `jon/story-blog`.
2. Jon can now start working on the project using his forked repository.
3. Fork verified in Gitea UI.

---

# End of Documentation
