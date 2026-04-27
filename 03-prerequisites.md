# **1. Virtualization (VM)**


### **What is Virtualization?**

Virtualization is running a **full OS inside another OS** using a **hypervisor**.

---

### 🖥️ Virtualization / VM Software Examples

* Oracle VM VirtualBox → Desktop VM
* VMware Workstation → Desktop VM
* VMware Fusion → Mac VM
* Parallels Desktop → Mac VM

---

### ☁️ Server / Cloud Virtualization

* VMware vSphere → Server VM
* Proxmox VE → Server VM
* Microsoft Hyper-V Server → Server VM
* Apache CloudStack → Cloud VM management

---


### **Layers in Virtualization**

```
Hardware
   │
Hypervisor (VMware, VirtualBox, KVM)
   │
Guest OS (Ubuntu, Windows, macOS)
   │
Apps
```

### **Key idea**

* VM has **its own kernel**
* Full OS needs CPU, RAM, disk
* Heavy but isolated

---

# **2. Containerization (Docker)**

### **What is Containerization?**

Containerization **shares the host OS kernel** but isolates apps using namespaces & cgroups.

### **Layers in Containerization**

```
Hardware
   │
Host OS Kernel (Linux kernel)
   │
Docker Engine
   │
Containers (share kernel)
```

### **Key idea**

* No separate OS inside container
* Very lightweight
* Fast startup (milliseconds)
* Uses **host Linux kernel**

This is why Docker is much faster than VMs.

---

# **3. Why Docker needs Linux kernel**

Docker uses:

* **Namespaces** → isolation
* **cgroups** → resource limits
* **OverlayFS** → layers

These exist only in the **Linux kernel**.
So Docker always needs Linux underneath.

---

# **4. Docker on Different OS**

## **A. Docker on Linux (Native)**

Works perfectly and directly.

```
Linux Host → Docker Engine → Containers
```

No VM required.

---

## **B. Docker on macOS**

Because macOS uses **XNU kernel**, not Linux.
Docker cannot run directly.

So Docker Desktop runs:

```
macOS
  │
Lightweight Linux VM (Lima)
  │
Docker Engine
  │
Containers
```

Containers actually run **inside Linux VM**.

---

## **C. Docker on Windows**

There are two modes.

### **1) Windows with WSL2 (Recommended)**

```
Windows
  │
WSL2 (real Linux kernel in VM)
  │
Docker Engine
  │
Containers
```

### **2) Windows Hyper-V (Old)**

```
Windows
  │
Hyper-V Linux VM
  │
Docker Engine
  │
Containers
```

Same idea: **Linux VM + Docker**.

---

# **5. Docker vs VM (Super Simple)**

| Feature          | Docker        | Virtual Machine |
| ---------------- | ------------- | --------------- |
| Uses host kernel | ✅ Yes (Linux) | ❌ No            |
| Boot time        | ms            | seconds–minutes |
| Resources        | Very light    | Heavy           |
| Isolation        | Medium        | Strong          |
| Runs full OS     | ❌ No          | ✅ Yes           |
| Performance      | Very fast     | Slower          |

---
