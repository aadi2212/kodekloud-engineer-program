# Nginx SSL Setup – App Server 2 (Stratos DC)

## 📌 Task Overview

Prepare App Server 2 for hosting a new application with **SSL enabled** using a self-signed certificate.

**Objective:**

* Install Nginx
* Configure SSL with provided certificate and key
* Serve a basic index.html page
* Verify SSL connectivity from Jump Host

---

## 🛠 Step-by-Step Execution

### 1️⃣ SSH into App Server 2

```bash
ssh 172.16.238.11
```

---

### 2️⃣ Install Nginx

```bash
sudo yum install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

### 3️⃣ Configure SSL

Create directory for SSL certificates:

```bash
sudo mkdir -p /etc/nginx/ssl
sudo mv /tmp/nautilus.crt /etc/nginx/ssl/
sudo mv /tmp/nautilus.key /etc/nginx/ssl/
```

Edit Nginx configuration:

```bash
sudo vi /etc/nginx/nginx.conf
```

Add/modify the server block:

```nginx
server {
    listen       443 ssl http2 default_server;
    listen       [::]:443 ssl http2 default_server;
    server_name  _;
    root         /usr/share/nginx/html;

    ssl_certificate "/etc/nginx/ssl/nautilus.crt";
    ssl_certificate_key "/etc/nginx/ssl/nautilus.key";
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  10m;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

    include /etc/nginx/default.d/*.conf;

    error_page 404 /404.html;
        location = /40x.html { }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html { }
}
```

---

### 4️⃣ Test & Restart Nginx

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

### 5️⃣ Create index.html

```bash
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```

---

### 6️⃣ Verify SSL Setup

From Jump Host:

```bash
curl -Ik https://<App-Server-2-IP-or-hostname>
```

**Expected Output:**

* HTTP/1.1 200 OK
* Server: nginx/...
* Confirms SSL connection is successful

---

## ✅ Final Outcome

* Nginx installed and running with SSL
* Self-signed certificate configured correctly
* index.html served under HTTPS
* Verified connectivity using curl from Jump Host

# End of Documentation
