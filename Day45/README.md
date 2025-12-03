# Resolve Dockerfile Issues – Custom HTTPD SSL Image

This document outlines the troubleshooting and resolution steps performed to fix a failing Dockerfile and successfully build a custom `httpd:2.4.43` image on **App Server 1 (Stratos DC)**.

---

## 📌 Task Requirements

### Dockerfile Location
/opt/docker



### Requirements Summary
- Must use **base image:** `httpd:2.4.43` (cannot be changed)
- Update Apache to listen on **port 8080** instead of 80
- Enable SSL configuration:
  - Enable SSL-related modules
  - Include the SSL config file
- Copy SSL certificates:
  - `server.crt`
  - `server.key`
- Copy website content:
  - `index.html`
- Fix the Dockerfile errors so the image builds successfully

---

## ❌ Issues in the Original Dockerfile

### 1. Wrong `httpd.conf` Path
Used incorrect path:
/usr/local/apache2/conf.d/httpd.conf


But the correct path in the `httpd:2.4.43` image is:
/usr/local/apache2/conf/httpd.conf



### 2. SSL Module & Include Lines Failing
Because the wrong path was referenced, all `sed` operations failed.

### 3. COPY Path Errors
Build failed when `certs/` or `html/` directory was missing in `/opt/docker` (build context).

---

## ✅ Corrected Dockerfile

```dockerfile
FROM httpd:2.4.43

# Update Apache to listen on port 8080
RUN sed -i "s/Listen 80/Listen 8080/g" /usr/local/apache2/conf/httpd.conf

# Enable SSL modules and include SSL config
RUN sed -i '/LoadModule ssl_module modules\/mod_ssl.so/s/^#//g' /usr/local/apache2/conf/httpd.conf
RUN sed -i '/LoadModule socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//g' /usr/local/apache2/conf/httpd.conf
RUN sed -i '/Include conf\/extra\/httpd-ssl.conf/s/^#//g' /usr/local/apache2/conf/httpd.conf

# Copy SSL certificates
COPY certs/server.crt /usr/local/apache2/conf/server.crt
COPY certs/server.key /usr/local/apache2/conf/server.key

# Copy website content
COPY html/index.html /usr/local/apache2/htdocs/


Notes:
sed commands now point to the correct httpd.conf location.

Assumes:
/opt/docker/certs/
/opt/docker/html/

directories exist and contain required files.


🚀 Build the Docker Image
docker build -t my-httpd-ssl /opt/docker

-t my-httpd-ssl → Tags the image
/opt/docker → Must contain:

Dockerfile

certs/server.crt

certs/server.key

html/index.html


🔍 Verify Image Creation
docker images | grep my-httpd-ssl


🧠 Lessons Learned
1. Wrong configuration path → All sed commands failed
   Fixed with correct path: /usr/local/apache2/conf/httpd.conf

2. Build context issues
   COPY works only if files exist inside the build directory.

3. Docker build context principle
   All referenced files must be inside /opt/docker.


🎉 Final Summary
Successfully built a custom HTTPD Docker image with:

1. Apache listening on port 8080

2. SSL enabled

3. SSL certificates properly copied

4. index.html deployed into Apache document root


The Dockerfile now builds successfully without altering:
Base image

Any valid configuration

Any data provided

