# Docker Python App Deployment

## **Objective**
Dockerize a Python application and deploy it on **App Server 1**, exposing it on port **8097** (host) mapped to **5003** (container).

---

## **Directory Structure**

/python_app/
Dockerfile
/src/
requirements.txt
server.py


---

## **Step 1: Verify Directories**

```bash
sudo mkdir -p /python_app/src
ls -l /python_app


Expected output:
drwxr-xr-x 2 root root 4096 Sep 24 src

Ensure requirements.txt and server.py exist inside /python_app/src/.


Step 2: Create server.py
sudo vi /python_app/src/server.py


Content:
from flask import Flask
app = Flask(__name__)

@app.route("/")
def home():
    return "Welcome to xFusionCorp Industries!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5003)



Step 3: Create Dockerfile
cd /python_app
sudo vi Dockerfile


If vi not found:

sudo yum install -y vim


Dockerfile content:
# Use a Python base image
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Copy requirements.txt
COPY src/requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application source code
COPY src/ .

# Expose application port
EXPOSE 5003

# Run the Python server
CMD ["python", "server.py"]


Verify:
ls -l /python_app/Dockerfile


Step 4: Build Docker Image
docker build -t nautilus/python-app .


If error occurs:
failed to resolve source metadata for python:3.11-slim  
dial tcp 10.0.0.6:443: i/o timeout


Reason: Docker Hub registry timeout.

Fix: Pull image manually

docker pull python:3.11-slim


Step 5: Deploy Container
Remove old container (if any):
docker rm -f pythonapp_nautilus 2>/dev/null


Run container:
docker run -d --name pythonapp_nautilus -p 8097:5003 nautilus/python-app


Verify:
docker ps


Step 6: Test Application
curl http://localhost:8097/


Expected output:
Welcome to xFusionCorp Industries!


Notes / Lessons Learned
Dockerfile must be located at /python_app/Dockerfile (grader checks exact path).

If base image fails to download, manually pull it.

Container name and port mapping must match exactly as instructed.


Task Completed Successfully ✔

