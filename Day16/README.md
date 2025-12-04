# Nginx Load Balancer Setup – Nautilus Stratos DC

## 📌 Task Overview

Due to increasing website traffic, the Nautilus team decided to deploy their application on a **high-availability stack**. The LBR (Load Balancer) configuration is required to complete the setup.

**Objective:**

* Install Nginx on LBR server
* Load balance across **3 app servers**
* Use correct Apache ports (do not change existing Apache configuration)
* Verify access from Jump Host / StaticApp

---

## 🛠 Step-by-Step Execution

### 1️⃣ Install & Start Nginx

```bash
sudo yum install -y nginx
sudo systemctl status nginx

# If stopped
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

### 2️⃣ Verify Apache on App Servers

```bash
ssh tony@172.16.238.10
ssh steve@172.16.238.11
ssh banner@172.16.238.12
```

Check Apache service:

```bash
sudo systemctl status httpd
```

Check Apache listening port:

```bash
sudo grep -i Listen /etc/httpd/conf/httpd.conf
# Found: Listen 3003
```

---

### 3️⃣ Configure Nginx for Load Balancing

Edit the main configuration file:

```bash
sudo vi /etc/nginx/nginx.conf
```

**nginx.conf Example:**

```nginx
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    upstream backend {
        server stapp01:3003;
        server stapp02:3003;
        server stapp03:3003;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://backend;
        }
    }

    include /etc/nginx/conf.d/*.conf;
}
```

> ⚠️ Ensure default configuration is deleted to avoid conflicts.

---

### 4️⃣ Test & Restart Nginx

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

### 5️⃣ Verify Load Balancer

Test each app server individually:

```bash
curl http://172.16.238.10:3003
curl http://172.16.238.11:3003
curl http://172.16.238.12:3003
# Output: Welcome to xFusionCorp Industries!
```

Test via Load Balancer from Jump Host:

```bash
curl http://172.16.238.14
```

---

## 🐞 Troubleshooting Notes

* ❌ **Error:** "server" directive not allowed → caused by missing braces `{}` in nginx.conf
* ❌ **Error:** Failed to connect to port 8080 → Apache runs on 3003, not 8080
* ✅ Resolution: Correct upstream ports and ensure braces are properly set
* ✅ Validate Nginx config and remove default configs to avoid conflicts

---

## ✅ Final Outcome

* Nginx installed and running on LBR server
* Load balancing across all app servers working correctly
* Apache services on all app servers remain unchanged
* Website accessible via LBR IP and individual app servers

# End of Documentation
