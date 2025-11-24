# **OneNote Documentation: Jenkins Slave Node Configuration**

---

## **📌 Task Objective**

Configure all three application servers as SSH build agents in Jenkins. Ensure each node has correct credentials, labels, remote directories, and Java installed so Jenkins agents connect successfully.

---

## **1️⃣ Install Required Plugin**

### **Steps:**

1. Go to **Manage Jenkins → Plugins**
2. Select **Available Plugins**
3. Search and install: **SSH Build Agents Plugin**
4. After installation, click **Restart Jenkins when installation is complete**

---

## **2️⃣ Add SSH Credentials for App Servers**

### Navigation:

**Manage Jenkins → Credentials → System → Global Credentials → Add Credentials**

### **Credentials Added:**

#### 🔹 **App Server 1**

* Username: **tony**
* Password: *(from server details)*
* ID: **stapp01**

#### 🔹 **App Server 2**

* Username: **steve**
* Password: *(from server details)*
* ID: **stapp02**

#### 🔹 **App Server 3**

* Username: **banner**
* Password: *(from server details)*
* ID: **stapp03**

---

## **3️⃣ Add Jenkins Nodes (SSH Build Agents)**

### Navigation:

**Manage Jenkins → Nodes → New Node**

---

### 🔹 **Node: App_server_1**

* Node Name: **App_server_1**
* Type: **Permanent Agent**
* Remote Root Directory: **/home/tony/jenkins**
* Labels: **stapp01**
* Launch Method: **Launch agents via SSH**
* Host: **stapp01**
* Credentials: **tony**
* Host Key Verification Strategy: **Non verifying verification strategy**

### **❗ Launch Error Encountered**

*Launch failed – SSH connection closed*

### **Reason:** Java was not installed on **App Server 1**.

### **Fix: Install Java 17 on App Server 1**

```
ssh tony@172.16.238.10
yum search java*
yum install java-17-openjdk-devel.x86_64 -y
java --version
```

After installation → Relaunch Agent →

### ✅ **Agent successfully connected**

---

## **4️⃣ Configure App_server_2 and App_server_3**

Follow the same steps as App_server_1.

---

### 🔹 **Node: App_server_2**

* Remote Root Directory: **/home/steve/jenkins**
* Labels: **stapp02**
* Host: **stapp02**
* Credentials: **steve**
* Host Key Verification: **Non verifying verification strategy**

### Install Java:

```
ssh steve@<server_ip>
yum install java-17-openjdk-devel.x86_64 -y
java --version
```

### ✅ Connected

---

### 🔹 **Node: App_server_3**

* Remote Root Directory: **/home/banner/jenkins**
* Labels: **stapp03**
* Host: **stapp03**
* Credentials: **banner**
* Host Key Verification: **Non verifying verification strategy**

### Install Java:

```
ssh banner@<server_ip>
yum install java-17-openjdk-devel.x86_64 -y
java --version
```

### ✅ Connected

---

## **5️⃣ Verify All Three Nodes Are Running**

Go to **Manage Nodes**, all agents should show:

### **🟢 Connected & Online**

* **App_server_1 – stapp01**
* **App_server_2 – stapp02**
* **App_server_3 – stapp03**

---

## **6️⃣ Test Each Node with a Job**

### **Create Test Job**

1. New Item → **test-job** → Freestyle Project
2. Under **Restrict where this project can run** → Label Expression: **App_server_1**
3. Build Steps → Execute Shell:

```
echo "Hello"
```

4. Save → Build Now

### **Console Output:**

```
Building remotely on App_server_1 (: stapp01) in workspace /home/tony/jenkins/workspace/test-job
+ echo Hello
Hello
Finished: Success
```

Repeat for:

* **App_server_2**
* **App_server_3**

All test jobs run successfully.

---

# **✅ Final Result**

✔ All three application servers added as Jenkins SSH agents
✔ Correct labels, directories, and credentials configured
✔ Java installed on all nodes for agent connectivity
✔ All agents online and verified with test jobs

### **🎉 Task fully completed successfully!**
