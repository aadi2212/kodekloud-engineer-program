# Tomcat Server Setup & Deployment – App Server 2 (Stratos DC)

## 📌 Task Overview

The Nautilus application team plans to deploy a Java-based application using **Tomcat** on App Server 2.

**Objective:**

* Install Tomcat server
* Configure it to run on port 8088
* Deploy ROOT.war from Jump Host
* Verify the application is accessible at base URL

---

## 🛠 Step-by-Step Execution

### 1️⃣ SSH into App Server 2

```bash
ssh steve@172.16.238.11
```

---

### 2️⃣ Install Tomcat

```bash
sudo yum install -y tomcat
sudo yum install -y tomcat tomcat-webapps tomcat-admin-webapps tomcat-docs-webapp
```

---

### 3️⃣ Configure Tomcat Port

Edit Tomcat configuration file:

```bash
sudo vi /etc/tomcat/server.xml
```

Locate the connector:

```xml
<Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

Change port to **8088**:

```xml
<Connector port="8088" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

---

### 4️⃣ Start & Enable Tomcat

```bash
sudo systemctl start tomcat
sudo systemctl enable tomcat
sudo systemctl status tomcat
```

---

### 5️⃣ Deploy ROOT.war from Jump Host

**Copy ROOT.war to App Server 2:**

```bash
scp /tmp/ROOT.war steve@172.16.238.11:/tmp/
```

**Move WAR file into Tomcat webapps directory:**

```bash
sudo mv /tmp/ROOT.war /usr/share/tomcat/webapps/
```

> ⚡ Note: Tomcat auto-deploys WAR files placed in `/usr/share/tomcat/webapps/`

---

### 6️⃣ Verify Deployment

Wait a few seconds for deployment, then test:

```bash
curl http://172.16.238.11:8088
```

**Expected Output (HTML snippet):**

```html
<!DOCTYPE html>
<html>
    <head>
        <title>SampleWebApp</title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
    </head>
    <body>
        <h2>Welcome to xFusionCorp Industries!</h2>
        <br>
    </body>
</html>
```

---

## ✅ Final Outcome

* Tomcat installed and running on port 8088
* ROOT.war successfully deployed
* Application homepage accessible via `curl http://172.16.238.11:8088`
* Task completed successfully ✅

# End of Documentation
