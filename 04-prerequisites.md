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

---

### 📄 **File Content**

| Action           | Command     |
| ---------------- | ----------- |
| Read file        | `cat file`  |
| View with scroll | `less file` |
| Edit (nano)      | `nano file` |
| Edit (vim)       | `vim file`  |

---

### 🔎 **Search**

| Action      | Command                   |
| ----------- | ------------------------- |
| Find file   | `find /path -name "file"` |
| Search text | `grep "text" file`        |

---

# 🟦 **2. Ubuntu / Debian (APT)**

### 📦 **Package Management**

| Action           | Command                         |
| ---------------- | ------------------------------- |
| Update packages  | `sudo apt update`               |
| Upgrade packages | `sudo apt upgrade`              |
| Install package  | `sudo apt install package-name` |
| Remove package   | `sudo apt remove package-name`  |
| Remove + config  | `sudo apt purge package-name`   |
| Search package   | `apt search keyword`            |

---

# 🟦 **3. Alpine Linux (APK)**

### 📦 **Package Management**

| Action           | Command                     |
| ---------------- | --------------------------- |
| Update packages  | `sudo apk update`           |
| Upgrade packages | `sudo apk upgrade`          |
| Install package  | `sudo apk add package-name` |
| Remove package   | `sudo apk del package-name` |
| Search package   | `apk search keyword`        |

---

# 🔥 **4. Process Management**

| Action              | Command           |            |
| ------------------- | ----------------- | ---------- |
| Find process        | `ps aux           | grep name` |
| Kill by PID         | `kill PID`        |            |
| Force kill          | `kill -9 PID`     |            |
| Kill by name        | `killall firefox` |            |
| Interactive monitor | `htop`            |            |

---

# 🟩 **5. System Info**

| Action         | Command               |
| -------------- | --------------------- |
| System summary | `neofetch`            |
| CPU info       | `lscpu`               |
| Disk devices   | `lsblk`               |
| RAM usage      | `free -h`             |
| Disk usage     | `df -h`               |
| OS info        | `cat /etc/os-release` |
| Kernel info    | `uname -a`            |
| System details | `hostnamectl`         |

---