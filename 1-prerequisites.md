
# **Linux OS Layers (Top → Bottom)**

## **1. User Applications (User Space)**

Where normal programs run.
Examples:

* Browsers (Chrome, Firefox)
* Terminal apps
* VS Code
* Docker CLI

➡️ These apps interact with the OS through system calls.

---

## **2. System Libraries (glibc, stdlib, OpenSSL, etc.)**

Provide common functions to applications.
Examples:

* glibc (standard C library)
* libstdc++ (C++ standard library)
* OpenSSL
* GTK, Qt

➡️ Apps use these libraries instead of directly interacting with the kernel.

---

## **3. System Call Interface (SCI)**

This is the bridge between **user space** and the **kernel**.

Examples of system calls:

* `read()`
* `write()`
* `fork()`
* `open()`
* `socket()`

➡️ Allows apps to request low-level operations (filesystem, network, processes).

---

## **4. Linux Kernel (Core of the OS)**

The heart of Linux — manages resources & hardware.

Kernel responsibilities:

* Process management
* Memory management
* Device drivers
* File systems
* Networking stack
* Security

➡️ Everything must go through the kernel to access hardware.

---

## **5. Hardware**

Actual computer components:

* CPU
* RAM
* Hard disk / SSD
* Network card
* GPU
* USB, etc.

➡️ Kernel communicates with hardware through **device drivers**.

---

# **Simple Diagram (Text Version)**

```
+----------------------------+
| User Applications          |
+----------------------------+
| System Libraries           |
+----------------------------+
| System Call Interface      |
+----------------------------+
| Linux Kernel               |
+----------------------------+
| Hardware                   |
+----------------------------+
```


