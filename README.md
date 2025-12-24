# REGDB_E_CLASSNOTREG



```md
# Fix Docker Desktop WSL Error on Windows 11 (REGDB_E_CLASSNOTREG)

This repository documents a **real-world issue** faced while installing **Docker Desktop on Windows 11 (25H2)** and how it was **fixed step by step**.

I was stuck with this issue for **more than 1 week**, and most online solutions did **not work**.  
This guide shows the **correct and working solution**.

---

## ❌ Problem Faced

After installing Docker Desktop and logging into Docker Hub, Docker showed this error:

```

WSL needs updating
Your version of Windows Subsystem for Linux (WSL) is too old
Run: wsl --update

````

When running the command:

```bash
wsl --update
````

It failed with this error:

```
Class not registered
Error code: Wsl/CallMsi/Install/REGDB_E_CLASSNOTREG
```

---

## 🧠 Root Cause (Important)

On **new Windows 11 versions (24H2 / 25H2)**:

* WSL is **no longer fully bundled** with Windows
* Old systems expect a **legacy MSI-based WSL**
* `wsl --update` fails because **WSL is not registered at OS registry level**
* Docker depends on **WSL 2**, so Docker fails to start

👉 This is **NOT a Docker issue**
👉 This is **NOT a Linux issue**
👉 This is a **missing WSL installation issue**

---

## ✅ System Information

* OS: **Windows 11 25H2**
* Docker: **Docker Desktop**
* Linux Distro: **Ubuntu (via WSL 2)**

---

## ✅ Final Working Solution (Step-by-Step)

### 🔹 Step 1: Enable Required Windows Features

Press **Win + R**, type:

```
optionalfeatures
```

Enable these two options:

* ✔ Windows Subsystem for Linux
* ✔ Virtual Machine Platform

Click **OK** and **restart your system**.

---

### 🔹 Step 2: Install WSL from Microsoft Store (MANDATORY)

On Windows 11 (25H2), this step is **required**.

Open **PowerShell as Administrator** and run:

```powershell
winget install -e --id Microsoft.WSL
```

Wait for installation to complete.

---

### 🔹 Step 3: Restart System (Very Important)

Restart your computer to properly register WSL components.

---

### 🔹 Step 4: Install Ubuntu (Linux)

After restart, open **Command Prompt (Admin)** and run:

```bash
wsl --install -d Ubuntu
```

This will:

* Install WSL 2
* Install Ubuntu automatically

---

### 🔹 Step 5: Create Default UNIX User (Normal Step)

During Ubuntu installation, it will ask:

```
Creating a default UNIX user account
```

* Enter any username (example: `ubuntu`, `user`, `mluser`)
* Enter a password (cursor will not move — this is normal)

This is **Linux-only**, not your Windows password.

---

### 🔹 Step 6: Set WSL 2 as Default

```bash
wsl --set-default-version 2
```

---

### 🔹 Step 7: Verify WSL Installation

```bash
wsl --status
```

Expected:

* Default Version: 2
* Kernel version shown

Check running distro:

```bash
wsl -l -v
```

Expected:

```
Ubuntu    Running    2
```

---

### 🔹 Step 8: Start Docker Desktop

Open **Docker Desktop**

If needed:

```
Docker Desktop → Settings → General
✔ Use WSL 2 based engine
```

---

## ✅ Final Verification

Check Docker:

```bash
docker --version
```

Test container:

```bash
docker run hello-world
```

If you see:

```
Hello from Docker!
```

🎉 **Docker + WSL 2 is working perfectly**

---

## 🧩 Error Explained (For Understanding)

```
REGDB_E_CLASSNOTREG
```

Means:

* Required WSL system classes were **not registered**
* Windows could not find WSL installer components
* Happens when WSL is missing or partially installed

---

## 🧠 Key Takeaway

> On **Windows 11 (24H2 / 25H2)**,
> **WSL must be installed via Microsoft Store / winget**
> `wsl --update` alone is NOT enough.

---

## 🎯 Who This Repo Helps

* Beginners installing Docker
* ML / MLOps / GenAI learners
* Developers facing WSL update errors
* Windows 11 users stuck with Docker WSL issues

---

## ⭐ Final Note

If this repo saved your time:

* Give it a ⭐
* Share it with others
* Save someone from wasting a week 😄

