# Pull and Retag BusyBox Image

## 📘 Task Overview
The Nautilus project developers are preparing to test features in a containerized environment. As part of the setup, we must:

- Pull the **busybox:musl** image on **App Server 2**.
- Re-tag it locally as **busybox:news**.

---

## 🎯 Requirements
1. Pull image: `busybox:musl`
2. Create a new tag: `busybox:news`
3. Ensure both images exist locally.

---

## 🚀 Steps Performed

### 1. Pull the BusyBox musl Image
```bash
docker pull busybox:musl


This fetches the BusyBox image built with musl libc.


2. Re-tag the Image as busybox:news
docker tag busybox:musl busybox:news

This creates a new local tag pointing to the same image.



3. Verify Images
docker images


Expected Output Example:
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
busybox      musl      44f1048931f5   11 months ago   1.46MB
busybox      news      44f1048931f5   11 months ago   1.46MB


Both tags share the same IMAGE ID, confirming they point to the same underlying image.


✅ Final Outcome
busybox:musl successfully pulled.

busybox:news tag successfully created.

Both images available locally for developer testing.

✔ Task completed successfully.

