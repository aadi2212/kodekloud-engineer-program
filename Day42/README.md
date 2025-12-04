# Docker Network Creation – macvlan Setup

## 📘 Task Overview
The Nautilus DevOps team needs to prepare docker environments for multiple applications.  
As part of this setup, a **macvlan-based docker network** must be created on **App Server 3**.

---

## 📝 Requirements

- **Network Name:** `blog`  
- **Driver:** `macvlan`  
- **Subnet:** `192.168.0.0/24`  
- **IP Range:** `192.168.0.0/24`  
- **Gateway:** `192.168.0.1`  

---

## 🚀 Steps Performed

### 1. SSH into App Server 3
```bash
ssh steve@172.16.238.12


2. Create a macvlan Docker Network

sudo docker network create -d macvlan \
  --subnet=192.168.0.0/24 \
  --ip-range=192.168.0.0/24 \
  --gateway=192.168.0.1 \
  blog


Explanation:
-d macvlan → Use macvlan driver

--subnet → Defines network subnet

--ip-range → Restricts IP allocation range

--gateway → Sets the gateway

blog → Name of the network



3. Verify the Created Network
List Docker Networks
sudo docker network ls


Inspect the blog Network
sudo docker network inspect blog


Expected Output Snippet:

"Name": "blog",
"Driver": "macvlan",
"IPAM": {
  "Config": [
    {
      "Subnet": "192.168.0.0/24",
      "IPRange": "192.168.0.0/24",
      "Gateway": "192.168.0.1"
    }
  ]
}


📚 Notes & Learnings
1.macvlan allows containers to appear as independent physical hosts on the network.

2.Subnet-based macvlan networks typically require a gateway to function properly.


Always verify configuration using:
docker network inspect blog


✔ Network created and verified successfully!

