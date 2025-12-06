# Install and Configure Two Static Websites on App Server 1 – xFusionCorp Task

## 📌 Task Overview

xFusionCorp Industries planned to host **two static websites** (`blog` & `demo`) on App Server 1 in Stratos Datacenter.

**Objective:**

* Install Apache (`httpd`) and dependencies
* Configure Apache to listen on **port 5003**
* Deploy `blog` and `demo` websites with the following URLs:

  * Blog → `http://localhost:5003/blog/`
  * Demo → `http://localhost:5003/demo/`
* Validate deployment using `curl`

> Website backups are present on the jump host at `/home/thor/blog` and `/home/thor/demo`.

---

## 🏁 Steps Performed

### 1️⃣ Login to App Server 1

```bash
ssh tony@stapp01
```

---

### 2️⃣ Install Apache and Dependencies

```bash
sudo yum install -y httpd
```

---

### 3️⃣ Enable & Start Apache

```bash
sudo systemctl enable --now httpd
sudo systemctl start httpd
```

---

### 4️⃣ Change Apache Port to 5003

Edit the Apache config file:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Change:

```apache
Listen 80
```

to:

```apache
Listen 5003
```

Restart Apache:

```bash
sudo systemctl restart httpd
```

Verify Apache is listening on port 5003:

```bash
sudo ss -tulnp | grep 5003
```

---

### 5️⃣ Copy Websites from Jump Host to App Server

From **jump host**:

```bash
scp -r /home/thor/blog tony@stapp01:/tmp/
scp -r /home/thor/demo tony@stapp01:/tmp/
```

On **App Server 1**, move websites to Apache root:

```bash
sudo mv /tmp/blog /var/www/html/
sudo mv /tmp/demo /var/www/html/
```

---

### 6️⃣ Configure Virtual Aliases in Apache

Edit Apache config:

```bash
sudo vi /etc/httpd/conf/httpd.conf
```

Add at the bottom:

```apache
Alias /blog "/var/www/html/blog"
<Directory "/var/www/html/blog">
    AllowOverride All
    Require all granted
</Directory>

Alias /demo "/var/www/html/demo"
<Directory "/var/www/html/demo">
    AllowOverride All
    Require all granted
</Directory>
```

Restart Apache to apply changes:

```bash
sudo systemctl restart httpd
```

---

### 7️⃣ Verify Websites

```bash
curl http://localhost:5003/blog/
curl http://localhost:5003/demo/
```

> Both commands should return their respective website content.

---

## ✅ Final Outcome

1. Apache installed and running on port **5003**
2. `blog` and `demo` websites deployed under `/var/www/html`
3. Websites accessible via `http://localhost:5003/blog/` and `http://localhost:5003/demo/`
4. Verified functionality using `curl`

---

# End of Documentation
