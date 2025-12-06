# LAMP Server Setup for WordPress Hosting – xFusionCorp Task

## 📌 Task Overview

xFusionCorp Industries planned to host a **WordPress website** on their infra in **Stratos Datacenter**.

**Objective:**

* Install Apache, PHP, and dependencies on all app servers
* Configure Apache to listen on port **8086**
* Install and configure MariaDB on DB server
* Create database `kodekloud_db5` and user `kodekloud_tim` with full privileges
* Deploy WordPress on shared storage `/var/www/html`
* Configure Nginx on Load Balancer to route traffic
* Verify website connectivity via LBR URL

---

## 🛠 Step-by-Step Execution

### 1️⃣ App Servers (stapp01, stapp02, stapp03)

**Install Apache and PHP dependencies:**

```bash
sudo yum install -y httpd php php-mysqlnd
```

**Start and enable Apache:**

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

**Configure Apache to listen on port 8086:**

```bash
sudo vi /etc/httpd/conf/httpd.conf
# Change Listen 80 → Listen 8086
sudo systemctl restart httpd
```

**Test Apache locally:**

```bash
curl http://<app-ip>:8086
```

---

### 2️⃣ MariaDB Setup on DB Server (stdb01)

**Install MariaDB:**

```bash
sudo yum install -y mariadb-server
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

**Create database and user:**

```sql
mysql -u root
CREATE DATABASE kodekloud_db5;
CREATE USER 'kodekloud_tim'@'%' IDENTIFIED BY 'YchZHRcLkL';
GRANT ALL PRIVILEGES ON kodekloud_db5.* TO 'kodekloud_tim'@'%';
FLUSH PRIVILEGES;
EXIT;
```

---

### 3️⃣ WordPress Setup on Shared Storage

**On Storage Server (ststor01):**

```bash
sudo yum install -y wget
cd /var/www/html
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
mv wordpress/* .
rm -rf wordpress latest.tar.gz
```

**Configure WordPress to connect to DB:**

```bash
mv wp-config-sample.php wp-config.php
vi wp-config.php
# Update the following:
define( 'DB_NAME', 'kodekloud_db5' );
define( 'DB_USER', 'kodekloud_tim' );
define( 'DB_PASSWORD', 'YchZHRcLkL' );
define( 'DB_HOST', 'stdb01' );  # or DB server IP
```

---

### 4️⃣ Nginx Setup on Load Balancer

**Install Nginx:**

```bash
sudo yum install -y nginx
```

**Configure upstream servers:**

```nginx
http {
    upstream backend {
        server 172.16.238.10:8086;
        server 172.16.238.11:8086;
        server 172.16.238.12:8086;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://backend;
        }
    }
}
```

**Handle port conflicts (if Nginx fails to start):**

```bash
sudo ss -tulnp | grep :80
# Stop conflicting service (e.g., haproxy)
sudo systemctl stop haproxy
sudo systemctl disable haproxy
```

**Start Nginx:**

```bash
sudo nginx -t
sudo systemctl restart nginx
```

**Test connectivity via LBR:**

```bash
curl http://172.16.238.14:8086
# Expected output: App is able to connect to the database using user kodekloud_tim
```

---

## ✅ Final Outcome

* Apache running on **port 8086** on all app servers
* MariaDB installed and configured with database `kodekloud_db5` and user `kodekloud_tim`
* WordPress deployed on shared storage `/var/www/html`
* Nginx load balancer routing traffic to app servers
* Verified connectivity from LBR URL, confirming DB connection

---

# End of Documentation
