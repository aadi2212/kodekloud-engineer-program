# Temporary User Setup with Expiry – App Server 2 (Stratos DC)

## 📌 Task Overview

A developer named **kirsty** requires temporary access to the Nautilus project. To ensure proper access management, a **temporary user account** with an expiry date is needed.

**Objective:**

* Create user `kirsty` on App Server 2
* Set account expiry date to **2024-01-28**
* Ensure username is lowercase

---

## 🛠 Step-by-Step Execution

### 1️⃣ SSH into App Server 2

```bash
ssh steve@172.16.238.11
# password: Am3ric@
```

---

### 2️⃣ Create the User with Expiry Date

```bash
sudo useradd -e 2024-01-28 kirsty
```

> ⚡ Note:
>
> * `-e` sets the account expiration date
> * Username must be **lowercase**

---

### 3️⃣ Set a Password for the User

```bash
sudo passwd kirsty
```

* Password set to: `kirsty@123`

---

### 4️⃣ Verify Account Details

```bash
sudo chage -l kirsty
```

**Expected Output:**

```
Account expires    : Jan 28, 2024
```

---

## ✅ Final Outcome

* Temporary user `kirsty` created successfully
* Account expiry correctly set to **Jan 28, 2024**
* User can log in until the expiration date

# End of Documentation
