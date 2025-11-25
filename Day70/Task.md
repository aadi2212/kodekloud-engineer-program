# 📄 Jenkins User Access Configuration — `jim`

Configuration of Jenkins security using Project-based Matrix Authorization Strategy, assigning restricted read-only access to the user **jim**, while keeping full admin access for **admin**.

---

## 📌 Task Objective

Configure Jenkins security so that:

- `admin` retains full Administrator permissions  
- `jim` has *only* read access  
- `anonymous` users have **no access**  
- Existing Jenkins job grants read-only permissions to `jim`

---

## 🚀 1. Access Jenkins

Open Jenkins UI and login:

- **Username:** admin  
- **Password:** Adm!n321  

---

## 🧱 2. Create User `jim`

Navigate:

Manage Jenkins → Users → Create User


Enter the following details:

| Field | Value |
|-------|--------|
| Username | jim |
| Password | Rc5C9EyvbU |
| Confirm Password | Rc5C9EyvbU |
| Full Name | Jim |

Click **Create User**.

You should now see both users: **admin** and **jim**.

---

## 🔌 3. Install Matrix Authorization Strategy Plugin

If "Project-based Matrix Authorization Strategy" is not visible:

1. Navigate:


Manage Jenkins → Plugins → Available Plugins


2. Search for:

Matrix Authorization Strategy


3. Install the plugin using:


Install without restart


4. After installation:


Restart Jenkins when installation is complete and no jobs are running


Wait until the Jenkins login screen reappears before continuing.

---

## 🔐 4. Configure Global Security

Navigate:


Manage Jenkins → Configure Global Security

Under **Authorization**, select:


Project-based Matrix Authorization Strategy


Add users:

- admin  
- jim  

Configure permissions:

### Matrix Authorization Table

| User | Overall | Job | Other Permissions |
|------|---------|------|-------------------|
| admin | Administer ✅ | All ✅ | All ✅ |
| jim | Read ✅ | (none) | (none) |
| anonymous | ❌ None | ❌ None | ❌ None |

Click **Save**.

---

## 🛠 5. Configure Job-Level Permissions

1. Open the existing job:


Dashboard → <Your Job Name> → Configure


2. Scroll to the "Project-based Security" section.

3. Enable:


Enable project-based security

4. Add user:

jim

5. Assign only:


Job → Read

Leave all other permissions unchecked.

Click **Save**.

---

## 🧪 6. Verify Access

Log out and log in as:

- Username: **jim**
- Password: **Rc5C9EyvbU**

Verify:

- jim can **view** jobs  
- jim **cannot** build, configure, delete, or modify anything  
- anonymous users cannot access Jenkins  

---

## ✅ Final Access Summary

| User | Permission | Access Level |
|------|------------|--------------|
| admin | Overall → Administer | Full access |
| jim | Overall → Read | Read-only |
| anonymous | None | No access |

---

## 📷 7. Documentation

Recommended screenshots:

1. User creation page (jim)  
2. Plugin installation (Matrix Authorization Strategy)  
3. Global Security → Matrix table  
4. Job-level project-based security  
5. Login screen showing jim's restricted access  

---

# 🎉 Task Completed Successfully



