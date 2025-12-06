# PostgreSQL Database Setup – Nautilus Stratos DC

## 📌 Task Overview

The Nautilus application development team plans to deploy a new application requiring **PostgreSQL**. The task was to configure PostgreSQL database and user as per requirements.

**Objective:**

* PostgreSQL server is already installed on **stdb01**
* Create a database user `kodekloud_joy` with password `LQfKeWWxWD`
* Create a database `kodekloud_db5`
* Grant full privileges to `kodekloud_joy` on `kodekloud_db5`
* ⚠️ Do not restart PostgreSQL service

---

## 🛠 Step-by-Step Execution

### 1️⃣ Login to DB Server

```bash
ssh peter@stdb01
sudo su - postgres
psql
```

---

### 2️⃣ Create Database User

```sql
CREATE USER kodekloud_joy WITH PASSWORD 'LQfKeWWxWD';
```

**Verify user creation:**

```sql
\du
# Should list kodekloud_joy ✅
```

---

### 3️⃣ Create Database

**Common Mistake:** Forgot semicolon

```sql
CREATE DATABASE kodekloud_db5
-- Waiting for more input (postgres-# prompt)
```

**Correct command:**

```sql
CREATE DATABASE kodekloud_db5;
```

**Verify database creation:**

```sql
\l
# Should list kodekloud_db5 ✅
```

---

### 4️⃣ Grant Privileges

```sql
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db5 TO kodekloud_joy;
```

**Verification:**

```sql
\du   # Check user exists
\l    # Check database ownership and access
```

---

### 5️⃣ Test Connection

```bash
psql -U kodekloud_joy -d kodekloud_db5 -h localhost
# Password: LQfKeWWxWD
```

Prompt should appear as:

```text
kodekloud_db5=>
```

✅ Successful login confirms setup is correct.

---

## 🐞 Troubleshooting Notes

* ❌ Forgot semicolon after SQL statements → PostgreSQL went into multiline mode
* ❌ Entered second `CREATE DATABASE` inside unfinished command → syntax error
* ✅ Fixed by canceling with `Ctrl+C` and re-running commands correctly
* ✅ User and database created, privileges granted, connection verified

---

## ✅ Final Outcome

* PostgreSQL user `kodekloud_joy` created
* Database `kodekloud_db5` created
* Full privileges granted to `kodekloud_joy`
* Verified login and database access without restarting PostgreSQL service

# End of Documentation
