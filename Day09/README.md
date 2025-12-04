# MariaDB Service Troubleshooting – Stratos DC

## 📌 Task Overview

The Nautilus application in Stratos DC was unable to connect to the **MariaDB database**.

**Objective:**

* Identify the cause of MariaDB service failure
* Restore MariaDB service to running state
* Ensure database connectivity for the application

---

## 🛠 Step-by-Step Execution

### 1️⃣ Check MariaDB Service Status

```bash
sudo systemctl status mariadb
```

* Service was **inactive/failed**

---

### 2️⃣ Check MariaDB Logs

```bash
sudo journalctl -xeu mariadb
```

* Found error:
  `Make sure the /var/lib/mysql is empty before running mariadb-prepare-db-dir`
* Indicates **data directory issue**

---

### 3️⃣ Verify MariaDB Data Directory Configuration

```bash
grep -i datadir /etc/my.cnf /etc/my.cnf.d/*.cnf
```

* Output: `datadir=/var/lib/mysql`

---

### 4️⃣ Inspect Actual Data Directory

```bash
sudo ls -l /var/lib/
sudo ls -l /var/lib/mysqld
```

* Found MariaDB system tables (ibdata1, ib_logfile0, mysql/) inside `/var/lib/mysqld`
* Data directory was at the wrong location

---

### 5️⃣ Stop MariaDB Service Before Making Changes

```bash
sudo systemctl stop mariadb
```

---

### 6️⃣ Move Data Directory to Correct Path

```bash
sudo mv /var/lib/mysqld /var/lib/mysql
```

---

### 7️⃣ Fix Permissions

```bash
sudo chown -R mysql:mysql /var/lib/mysql
sudo chmod -R 750 /var/lib/mysql
```

---

### 8️⃣ Start MariaDB Service

```bash
sudo systemctl start mariadb
sudo systemctl status mariadb
```

* Service started successfully

---

### 9️⃣ Verify MariaDB Functionality

```bash
mysql -u root -p -e "SHOW DATABASES;"
```

* Confirmed default databases: `mysql`, `performance_schema`, etc.

---

## ✅ Final Outcome

* MariaDB service started and running normally
* Root cause: Data directory was incorrectly located at `/var/lib/mysqld`
* Resolution: Moved data directory to `/var/lib/mysql` and fixed permissions
* Application can now connect to MariaDB without issues ✅

# End of Documentation

