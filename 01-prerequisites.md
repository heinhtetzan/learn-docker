# 📘 Computer System Overview

## 🖥️ 1. Computer Hardware

* CPU (Central Processing Unit)
* RAM (Memory)
* Storage (SSD / HDD)
* Network Interface Card (NIC)
* GPU (Graphics Processing Unit)
* Motherboard
* Input Devices (Keyboard, Mouse)
* Output Devices (Monitor, Printer)

---

## 💻 2. Operating Systems

### Desktop / Server OS

* Linux
* Windows
* macOS

### Mobile OS

* Android
* iOS

---

## 🐧 3. Linux Main Distributions

### Debian-based

* Debian
* Ubuntu
* Linux Mint

### Arch-based

* Arch Linux
* Manjaro

### Red Hat-based

* Red Hat Enterprise Linux
* CentOS
* Fedora

---

## 🧱 4. OS Layers (Top → Bottom)

```text
+----------------------------+
| User Applications          |  ← Google Chrome, VS Code, Docker CLI
+----------------------------+
| Runtime / Engine           |  ← PHP Runtime, Node.js, JVM
+----------------------------+
| System Libraries           |  ← glibc, OpenSSL, libstdc++
+----------------------------+
| System Calls               |  ← socket(), connect(), read(), write(), open()
+----------------------------+
| Linux Kernel               |  ← process scheduling, memory management, TCP/IP, filesystem
+----------------------------+
| Hardware                   |  ← CPU, RAM, SSD, Network Card (NIC)
+----------------------------+
```

---

## 🔁 5. Example Flow (Code → System → Hardware)

### 🌐 HTTP Request (Node.js)

```text
Your Code        → fetch()
Runtime          → :contentReference[oaicite:13]{index=13}
Async Engine     → libuv
System Library   → glibc / :contentReference[oaicite:14]{index=14}
System Call      → socket(), connect(), read(), write()
Kernel           → networking stack
Hardware         → NIC → Internet
```

---

### 📂 File Read

```text
Your Code        → read file
Runtime          → Node.js / PHP
System Library   → glibc
System Call      → open(), read()
Kernel           → filesystem
Hardware         → SSD / Disk
```

---

### 🧵 Process Creation

```text
Command          → run program
System Call      → fork(), execve()
Kernel           → process scheduler
Hardware         → CPU
```

---

### ⚡ Async I/O (High Performance)

```text
Runtime          → :contentReference[oaicite:15]{index=15} / :contentReference[oaicite:16]{index=16}
System Call      → epoll_wait()
Kernel           → event-driven I/O
```

---

## 🔌 6. Code → Hardware Mapping (Generic Flow)

```text
Application Code
   ↓
Runtime (Node.js / PHP / Java)
   ↓
System Libraries (glibc, OpenSSL)
   ↓
System Calls (read, write, socket, etc.)
   ↓
Operating System Kernel
   ↓
Device Drivers
   ↓
Hardware (CPU, RAM, Disk, Network)
```

---

## 🎯 Summary (Key Idea)

* Developers write code at **application level**
* OS provides **abstraction layers**
* Only the **kernel can access hardware**
* Communication happens through **system calls**

