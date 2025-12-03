# Apache Setup Inside kkloud Container (Docker EXEC Operations)

## 📘 Task Overview
A team member working on the `kkloud` container went on PTO, and the remaining Apache configuration work must be completed.

The `kkloud` container is running on **App Server 1** in the **Stratos Datacenter**.

---

## 🎯 Requirements
- Install **apache2** inside the running `kkloud` container.
- Configure Apache to **listen on port 5001** (not bound to any specific IP).
- Ensure Apache is **running** and the **container remains active**.

---

## 🚀 Steps Performed

### 1. Access the kkloud Container
```bash
docker exec -it kkloud bash


2. Install Apache2
apt-get update -y
apt-get install apache2 -y


3. Configure Apache to Listen on Port 5001
Edit ports.conf
apt-get install nano -y
nano /etc/apache2/ports.conf


Change:
Listen 80


To:
Listen 5001


Edit Default VirtualHost
nano /etc/apache2/sites-available/000-default.conf


Change:
<VirtualHost *:80>


To:
<VirtualHost *:5001>



4. Restart Apache Service
service apache2 restart


5. Validate Apache Is Running
Check service status:
service apache2 status


Optional: verify listening ports
apt-get install net-tools -y
netstat -tulnp | grep 5001


6. Exit the Container
exit


7. Ensure Container Is Still Running
docker ps


⚠️ Warning Encountered (Expected)
Upon restarting Apache:
AH00558: apache2: Could not reliably determine the server's fully qualified domain name


Explanation:
This is harmless and happens when no ServerName is set in Apache configuration.
It does not affect functionality.


✅ Final Outcome
Apache installed inside kkloud container

Listening on port 5001

Apache service running successfully

Container remains active


✔ Task completed successfully


