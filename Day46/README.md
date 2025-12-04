# Deploy an App on Docker Containers

This document outlines the steps followed to deploy a PHP (Apache) web application and a MariaDB database using Docker Compose on **App Server 1** in the Stratos Datacenter.

---

## 📌 Task Objective

Create a Docker Compose file at:

/opt/finance/docker-compose.yml



The compose stack must run:

- **web service** → PHP + Apache  
- **db service** → MariaDB  

---

## 🔧 Requirements Summary

### **Web Service**
- Container name: `php_web`
- Image: `php` with any `apache` tag (example: `php:apache`)
- Ports: `6000:80`
- Volume: `/var/www/html:/var/www/html`

### **DB Service**
- Container name: `mysql_web`
- Image: `mariadb` (preferably `latest`)
- Ports: `3306:3306`
- Volume: `/var/lib/mysql:/var/lib/mysql`
- Environment:
  - `MYSQL_DATABASE=database_web`
  - `MYSQL_USER=<non-root user>`
  - `MYSQL_PASSWORD=<complex password>`
  - `MYSQL_ROOT_PASSWORD=<backup root password>`

---

## 📁 1. Directory & File Setup

```bash
mkdir -p /opt/finance
cd /opt/finance
touch docker-compose.yml


❌ 2. Initial Docker Compose File (Had an Error)
version: '3.8'

services:
  web:
    container_name: php_web
    image: php:apache
    ports:
      - "6000:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    container: mysql_web          # ❌ Incorrect key (should be container_name)
    image: mariadb:latest
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_web
      MYSQL_USER: app_user
      MYSQL_PASSWORD: C0mpl3x@Pass
      MYSQL_ROOT_PASSWORD: R00t@Backup


❗ 3. Error Encountered
Running:
docker compose -f /opt/finance/docker-compose.yml up -d


Error output:
validating /opt/finance/docker-compose.yml: services.db Additional property container is not allowed


Root Cause
Used the invalid key container instead of container_name.


✅ 4. Corrected Docker Compose File (Final Working Version)
version: '3.8'

services:
  web:
    container_name: php_web
    image: php:apache
    ports:
      - "6000:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    container_name: mysql_web
    image: mariadb:latest
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_web
      MYSQL_USER: app_user
      MYSQL_PASSWORD: C0mpl3x@Pass
      MYSQL_ROOT_PASSWORD: R00t@Backup


🚀 5. Deployment
docker compose -f /opt/finance/docker-compose.yml up -d


✔ Containers launched successfully.


🔍 6. Verification
Check running containers:
docker ps


Expected output:
CONTAINER ID   NAME        IMAGE           PORTS                          STATUS
abc123xyz456   php_web     php:apache      0.0.0.0:6000->80/tcp           Up
def789uvw012   mysql_web   mariadb:latest  0.0.0.0:3306->3306/tcp         Up


🌐 7. Test the Web Application
curl http://<server-ip>:6000/

If configured correctly, you should receive a webpage response.


🎉 Final Outcome
Initial deployment failed due to invalid YAML key (container).

Issue resolved using correct key container_name.

Docker Compose stack now running successfully with:
1. php_web on port 6000

2. mysql_web on port 3306


This completes the Docker-based deployment for the Nautilus DevOps team.
