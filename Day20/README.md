# Configure Nginx + PHP-FPM Using Unix Socket – Nautilus Task

## 📌 Task Overview

The Nautilus application development team planned to deploy a **PHP-based application** on App Server 1 (stapp01) in Stratos DC.

**Objective:**

* Install **nginx** and configure it on port 8094 with document root `/var/www/html`
* Install **PHP-FPM 8.3**, using Unix socket `/var/run/php-fpm/default.sock`
* Configure nginx and PHP-FPM to work together
* Validate the setup with `curl` from the jump host

> **Note:** Application files `index.php` and `info.php` are already present under `/var/www/html`. Do **not** modify these files.

---

## 🏁 Steps Performed

### 1️⃣ SSH into App Server 1

```bash
ssh tony@stapp01
```

---

### 2️⃣ Install Nginx

```bash
sudo dnf install -y nginx
sudo systemctl enable --now nginx
```

* Configured nginx to listen on **port 8094**
* Set document root to `/var/www/html`

---

### 3️⃣ Install PHP-FPM 8.3

**Issue encountered:** Tried `yum-config-manager --enable remi-php83` → failed (no matching repo).

**Resolution:** CentOS Stream 9 already provides PHP 8.3 via AppStream:

```bash
sudo dnf module reset -y php
sudo dnf module enable -y php:8.3
sudo dnf install -y php php-fpm
```

---

### 4️⃣ Configure PHP-FPM

Edit `/etc/php-fpm.d/www.conf`:

```ini
listen = /var/run/php-fpm/default.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
user = nginx
group = nginx
```

Create socket directory and set permissions:

```bash
sudo mkdir -p /var/run/php-fpm
sudo chown -R nginx:nginx /var/run/php-fpm
```

Enable and start PHP-FPM:

```bash
sudo systemctl enable --now php-fpm
```

---

### 5️⃣ Configure Nginx to Use PHP-FPM

Create `/etc/nginx/conf.d/app.conf`:

```nginx
server {
    listen 8094;
    server_name stapp01;
    root /var/www/html;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/var/run/php-fpm/default.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

Remove default nginx config if present:

```bash
sudo rm -f /etc/nginx/conf.d/default.conf
```

Test and reload nginx:

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

### 6️⃣ Verify Application

From jump host:

```bash
curl http://stapp01:8094/index.php
curl http://stapp01:8094/info.php
```

* `index.php` → application content
* `info.php` → PHP info page

---

### 🐞 Mistakes & Resolution

* **Mistake:** Tried enabling `remi-php83` repo using `yum-config-manager`
* **Reason:** Works on CentOS 7/8 with Remi repo, but CentOS Stream 9 provides PHP 8.3 natively
* **Resolution:** Enabled PHP 8.3 module using `dnf module enable php:8.3`

---

## ✅ Final Outcome

1. **Nginx** running on port 8094
2. **PHP-FPM 8.3** running via Unix socket `/var/run/php-fpm/default.sock`
3. Application files served correctly from `/var/www/html`
4. Fully functional PHP-based application environment

---

# End of Documentation
