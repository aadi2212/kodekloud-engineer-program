# Dockerfile – Custom Apache on Port 6200

## 📘 Task Overview
The Nautilus DevOps team received a requirement from the application development team to create a **custom Docker image**.  
This image must run **Apache2** on **Ubuntu 24.04**, configured to listen on **port 6200**.

The Dockerfile must be created at:
/opt/docker/Dockerfile



---

## 📝 Requirements

- **Base Image:** `ubuntu:24.04`
- **Install:** `apache2`
- **Configure Apache to listen on:** `6200`
- **Do NOT change:** document root or other Apache settings
- **Expose Port:** 6200
- **Run Apache:** in foreground

---

## 🚀 Steps Performed

### 1. Prepare Directory and Create Dockerfile
```bash
mkdir -p /opt/docker
cd /opt/docker
vi Dockerfile



2. Dockerfile Content

FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y apache2 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN sed -i 's/80/6200/g' /etc/apache2/ports.conf && \
    sed -i 's/*:80/*:6200/g' /etc/apache2/sites-available/000-default.conf

EXPOSE 6200

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]


🔨 Build the Custom Image
docker build -t custom-apache:latest .


▶️ Run a Container Using the Image
docker run -d -p 6200:6200 custom-apache:latest


✅ Verification
Test via curl:
curl http://localhost:6200

Expected: Apache default welcome page HTML.


🎯 Final Outcome
1.Dockerfile created at /opt/docker/Dockerfile

2.Custom image built using ubuntu:24.04

3.Apache2 configured to run on port 6200

4.Container tested and serving successfully on 6200


✔ Task completed successfully!
