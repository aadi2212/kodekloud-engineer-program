# Install Jenkins Plugins (Git & GitLab)

## 📝 Task Overview
Install Git and GitLab plugins on the Jenkins server to enable source-code integration for CI/CD pipelines.

---

## 🔐 1. Access Jenkins UI

1. Click on the **Jenkins** button from the top navigation bar.
2. Log in using the following credentials:

Username: admin
Password: Adm!n321


---

## 🔧 2. Install Git & GitLab Plugins

Navigate to:


Dashboard → Manage Jenkins → Manage Plugins → Available



Search and select:

- ✅ **Git Plugin**
- ✅ **GitLab Plugin**

Click:



Install without restart


---

## 🔄 3. Restart Jenkins (If Prompted)

After installation, if Jenkins requests a restart:

Choose:


Restart Jenkins when installation is complete and no jobs are running


Wait until the **Jenkins login page** reappears before continuing.

---

## ✔️ 4. Verification

To confirm plugin installation:

Navigate to:


Dashboard → Manage Jenkins → Manage Plugins → Installed


You should now see:

- ✅ **Git**
- ✅ **GitLab**

---

## 🎯 Outcome

- Jenkins successfully updated with Git & GitLab plugins.
- Ready for CI/CD workflows and SCM integrations.
- Login verified after restart.

---


Install without restart

