# Copy File to Docker Container

## 📘 Task Overview
The Nautilus DevOps team needs to securely transfer an encrypted file from the Docker host on **App Server 3** into a running container named **ubuntu_latest**.  
The file must be copied **without any modification** to ensure confidentiality and integrity.

---

## 📝 Requirements

- **Source file on host:** `/tmp/nautilus.txt.gpg`
- **Target container:** `ubuntu_latest`
- **Destination path inside container:** `/opt/`
- **File must remain unchanged (no edit/transform)**

---

## 🚀 Steps Performed

### 1. Verify Running Container
```bash
docker ps

Confirmed that ubuntu_latest is running.


2. Copy File from Host to Container
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/opt/

This command securely copies the file while preserving its contents and permissions.



3. Verify Inside the Container
Enter the container:
docker exec -it ubuntu_latest bash


Check file:
ls -l /opt/nautilus.txt.gpg


Confirmed that the file exists in the /opt/ directory exactly as copied.



🔐 Integrity Verification (Optional)
Checksum comparison:
md5sum /tmp/nautilus.txt.gpg
md5sum /opt/nautilus.txt.gpg


Both checksums matched → file was not modified.



🎯 Final Outcome
The encrypted file /tmp/nautilus.txt.gpg was successfully copied from the host into the container.
The file now exists at /opt/nautilus.txt.gpg inside the ubuntu_latest container.
File integrity and confidentiality were preserved.


✔ Task completed successfully.



