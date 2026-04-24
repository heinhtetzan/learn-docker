# Docker Networking

## 1. Why Docker Networking Matters

Docker networking allows **containers to communicate** with:

* Other containers
* The host machine
* External networks (internet)

Without networking:

* APIs cannot talk to databases
* Frontend cannot call backend
* Microservices cannot communicate

---

## 2. Basic Networking Concepts (Before Docker)

### Host

Your computer or server running Docker

### Port

A number used to identify a service

* HTTP → 80
* HTTPS → 443
* MySQL → 3306

### IP Address

Unique address for a machine or container

### Network

A group of machines that can communicate

---

## 3. Docker Networking Model (Big Picture)

Docker creates **virtual networks** similar to:

* Wi-Fi network at home
* Office LAN

Each container:

* Has its own IP
* Can join one or more networks

---

## 4. Docker Network Drivers (Types)

### 1️⃣ bridge (MOST COMMON)

* Default network
* Used for containers on the same host
* Containers can talk using container names

### 2️⃣ host

* Container shares host network
* No isolation
* High performance

### 3️⃣ none

* No network
* Fully isolated

### 4️⃣ overlay (advanced)

* Used in Docker Swarm
* Multi-host networking

---

## 5. Default Bridge Network

Check default networks:

```bash
docker network ls
```

Inspect bridge network:

```bash
docker network inspect bridge
```

Run a container on default bridge:

```bash
docker run -d --name app nginx
```

❌ Containers **CANNOT** resolve each other by name on default bridge

---

## 6. User-Defined Bridge Network (BEST PRACTICE)

### Create a network

```bash
docker network create my_network
```

### Run containers on it

```bash
docker run -d --name backend --network my_network nginx

docker run -d --name frontend --network my_network nginx
```

### Test communication

```bash
docker exec -it frontend ping backend
```

✅ Container name works as DNS

---

## 7. Port Mapping (Container ↔ Host)

### Syntax

```bash
-p HOST_PORT:CONTAINER_PORT
```

### Example

```bash
docker run -d -p 8080:80 nginx
```

| Access         | Meaning          |
| -------------- | ---------------- |
| localhost:8080 | Host side        |
| 80             | Inside container |

---

## 8. Container-to-Container Communication

### Example: Backend + Database

```bash
docker network create app_net

# MySQL
docker run -d \
 --name mysql \
 --network app_net \
 -e MYSQL_ROOT_PASSWORD=1234 \
 mysql:8

# Backend
docker run -d \
 --name api \
 --network app_net \
 my-api-image
```

### Backend DB Host

```
DB_HOST=mysql
```

❗ Never use `localhost` inside containers

---

## 9. Docker Compose Networking (IMPORTANT)

Docker Compose automatically:

* Creates a network
* Adds DNS

### Example

```yaml
version: "3"
services:
  backend:
    image: my-api
    ports:
      - "3000:3000"

  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: 1234
```

### Access DB from backend

```
DB_HOST=db
```

---

## 10. Common Mistakes (Very Important)

❌ Using `localhost` to access another container

❌ Forgetting to expose ports

❌ Mixing host and container ports

❌ Running containers on different networks

---

## 11. Debugging Docker Networking

### Check container IP

```bash
docker inspect container_name
```

### Enter container

```bash
docker exec -it container_name sh
```

### Test connection

```bash
ping service_name
curl http://service_name:port
```

---

## 12. Teaching Flow Recommendation

1. Networking basics
2. Default bridge
3. User-defined bridge
4. Port mapping
5. Container DNS
6. Docker Compose
7. Real project example

---

## 13. Real-World Example to Teach

### Frontend → Backend → Database

* React → API → MySQL
* All services in one network
* Access by service name

---
Got it 👍
Here is **ONLY** for **`ifconfig`**, **`ping`**, and **`curl`** — **package name, install command, and example** (Ubuntu).

---

## 1️⃣ `ifconfig`

### 📦 Package name

```text
net-tools
```

### 📥 Install

```bash
sudo apt update
sudo apt install net-tools
```

### ▶️ Command

```bash
ifconfig
```

### 🧪 Example use

```bash
ifconfig eth0
```

---

## 2️⃣ `ping`

### 📦 Package name

```text
iputils-ping
```

### 📥 Install

```bash
sudo apt update
sudo apt install iputils-ping
```

### ▶️ Command

```bash
ping google.com
```

### 🧪 Example use

```bash
ping -c 4 8.8.8.8
```

---

## 3️⃣ `curl`

### 📦 Package name

```text
curl
```

### 📥 Install

```bash
sudo apt update
sudo apt install curl
```

### ▶️ Command

```bash
curl https://example.com
```

### 🧪 Example use

```bash
curl -I https://example.com
```

---

## 📌 Quick Summary Table

| Tool     | Package        | Install Command                 |
| -------- | -------------- | ------------------------------- |
| ifconfig | `net-tools`    | `sudo apt install net-tools`    |
| ping     | `iputils-ping` | `sudo apt install iputils-ping` |
| curl     | `curl`         | `sudo apt install curl`         |

---

Here is the **same thing**, but for **Alpine Linux** (only **ifconfig**, **ping**, **curl**) — **package, install command, and example**.

---

# 🏔 Alpine Linux – Networking Commands

Alpine uses **`apk`** (not `apt`).

---

## 1️⃣ `ifconfig`

### 📦 Package name

```text
net-tools
```

### 📥 Install

```bash
apk update
apk add net-tools
```

### ▶️ Command

```bash
ifconfig
```

### 🧪 Example

```bash
ifconfig eth0
```

---

## 2️⃣ `ping`

### 📦 Package name

```text
iputils
```

### 📥 Install

```bash
apk update
apk add iputils
```

### ▶️ Command

```bash
ping google.com
```

### 🧪 Example

```bash
ping -c 4 8.8.8.8
```

---

## 3️⃣ `curl`

### 📦 Package name

```text
curl
```

### 📥 Install

```bash
apk update
apk add curl
```

### ▶️ Command

```bash
curl https://example.com
```

### 🧪 Example

```bash
curl -I https://example.com
```

---

## 📌 Quick Summary (Alpine)

| Tool     | Package   | Install             |
| -------- | --------- | ------------------- |
| ifconfig | net-tools | `apk add net-tools` |
| ping     | iputils   | `apk add iputils`   |
| curl     | curl      | `apk add curl`      |

---

## 🧠 Teaching note (important)

* Alpine is **minimal** → many tools are **NOT installed by default**
* In **Docker Alpine images**, you almost always need:

```bash
apk add --no-cache curl iputils net-tools
```

---


