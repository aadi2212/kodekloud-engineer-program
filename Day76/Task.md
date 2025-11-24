# **📘 OneNote Documentation — Jenkins Job Permission Setup**

---

## **📌 Task Name:** Jenkins Job Permission Setup

## **🏢 Project:** xFusionCorp Industries — Developer Access Control

---

# **🎯 Objective**

Grant controlled permissions to two new developers (**sam** and **rohan**) on the existing Jenkins job **Packages**, ensuring the system remains secure and error-free.

---

# **🧩 Prerequisites**

### **Login Credentials**

* **Jenkins Admin:**

  * Username: `admin`
  * Password: `Adm!n321`
* **Developer Accounts:**

  * sam → `sam@pass12345`
  * rohan → `rohan@pass12345`

### **Existing Setup**

* Jenkins Job: **Packages**
* Required Plugins:

  * Role-Based Authorization Strategy **OR**
  * Matrix Authorization Strategy

---

# **1️⃣ Step 1: Login to Jenkins**

1. Open Jenkins UI.
2. Login using:

   * Username: `admin`
   * Password: `Adm!n321`

---

# **2️⃣ Step 2: Install Required Plugins**

Navigate to:

```
Manage Jenkins → Manage Plugins → Available
```

Search and install:

* **Role-based Authorization Strategy** (preferred)
  **or**
* **Matrix Authorization Strategy**

After installation:
✔️ Click **Restart Jenkins when installation is complete and no jobs are running**

---

# **3️⃣ Step 3: Configure Global Security**

Navigate to:

```
Manage Jenkins → Configure Global Security
```

### **Settings:**

1. Under **Authorization**, select:
   **Project-based Matrix Authorization Strategy**
2. Add the following users:

   * sam
   * rohan
3. Grant **Overall → Read** permission to both users.
4. Click **Save**.

⚠️ **Important:** Without *global read permission*, Jenkins will throw errors when applying job-level permissions.

---

# **4️⃣ Step 4: Assign Job-Specific Permissions**

Navigate to:

```
Jenkins Dashboard → Packages (job) → Configure
```

### **Enable Project-Level Security:**

* Check: **Enable project-based security**
* Check: **Inherit permissions from parent ACL**

### **Assign Permissions:**

| **User**  | **Permissions**                             |
| --------- | ------------------------------------------- |
| **sam**   | Build, Configure, Read                      |
| **rohan** | Build, Cancel, Configure, Read, Update, Tag |

Save the job configuration.

---

# **5️⃣ Step 5: Verification**

### **Login as sam:**

✔ Verify access to:

* Read job
* Build job
* Configure job

### **Login as rohan:**

✔ Verify access to:

* Read job
* Build job
* Cancel builds
* Configure
* Update
* Tag

---

# **6️⃣ Step 6: Documentation / Evidence**

Take screenshots of:

1. **Global Security** page (showing sam & rohan with Overall → Read).
2. **Project Security** section on the Packages job.

Optional:
🎥 Record configuration steps using Loom for future reference.

---

# **📎 Key Notes / Best Practices**

* Grant only required permissions—follow the principle of least privilege.
* Always enable **Inherit permissions from parent ACL**.
* Global Read permission is **mandatory** for job-level perm
