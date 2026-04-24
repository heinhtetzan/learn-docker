# 🔰 **1. Universal Basic Linux Commands**

### 📁 **File & Folder**

| Action           | Command            |
| ---------------- | ------------------ |
| List files       | `ls` / `ls -la`    |
| Change directory | `cd folder`        |
| Make folder      | `mkdir folder`     |
| Remove file      | `rm file`          |
| Remove folder    | `rm -r folder`     |
| Copy             | `cp source target` |
| Move             | `mv source target` |

### 📄 **File content**

| Action           | Command     |
| ---------------- | ----------- |
| Read file        | `cat file`  |
| View with scroll | `less file` |
| Edit file (nano) | `nano file` |
| Edit file (vim)  | `vim file`  |

### 🔎 **Search**

| Action      | Command                   |
| ----------- | ------------------------- |
| Find file   | `find /path -name "file"` |
| Search text | `grep "text" file`        |

---

# 🟦 **2. Ubuntu / Debian (APT)**

### 🔄 **Update & Upgrade**

```bash
sudo apt update
sudo apt upgrade
```

### 📦 **Install Package**

```bash
sudo apt install package-name
```

### ❌ **Remove Package**

```bash
sudo apt remove package-name
sudo apt purge package-name   # remove config files too
```

### 🔍 **Search Package**

```bash
apt search keyword
```

---

# 🟥 **3. Fedora (DNF)**

### 🔄 **Update**

```bash
sudo dnf update
```

### 📦 **Install Package**

```bash
sudo dnf install package-name
```

### ❌ **Remove Package**

```bash
sudo dnf remove package-name
```

### 🔍 **Search Package**

```bash
dnf search keyword
```

---

# 🟧 **4. Arch Linux (Pacman)**

### 🔄 **Update (Full system upgrade)**

```bash
sudo pacman -Syu
```

### 📦 **Install Package**

```bash
sudo pacman -S package-name
```

### ❌ **Remove Package**

```bash
sudo pacman -R package-name
sudo pacman -Rns package-name   # remove dependencies/config
```

### 🔍 **Search Package**

```bash
pacman -Ss keyword
```

---

# 🔥 **5. Process Management (Kill)**

Works on **all distros**.

### 🧩 **Find Process**

```bash
ps aux | grep name
```

### ❌ **Kill by PID**

```bash
kill PID
```

### 🔪 **Force kill**

```bash
kill -9 PID
```

### 🔥 **Kill all processes by name**

```bash
killall firefox
```

---

# 🟩 **6. System Info**

### **Check hardware**

```bash
neofetch      # install via apt/dnf/pacman if needed
lscpu
lsblk
free -h
df -h
```

### **Check OS version**

```bash
lsb_release -a
cat /etc/os-release
```

