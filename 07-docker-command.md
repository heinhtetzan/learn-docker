Let's go **step by step** and explain `docker pull`, `docker run`, and the `-itpd` options, with examples for your list of images: Ubuntu, Nginx, Node, PHP, MySQL, MongoDB, and MinIO.

---

### **1. `docker pull`**

`docker pull` downloads an image from Docker Hub (or another registry) to your local machine.

**Syntax:**

```bash
docker pull <image>:<tag>
```

* `<image>` → the name of the image (e.g., `ubuntu`, `nginx`)
* `<tag>` → version (optional, default is `latest`)

**Examples:**

```bash
docker pull ubuntu:22.04
docker pull nginx:latest
docker pull node:20
docker pull php:8.2
docker pull mysql:8.0
docker pull mongo:6.0
docker pull minio/minio:latest
```

---

### **2. `docker run`**

`docker run` creates a **container** from an image and runs it.

**Basic Syntax:**

```bash
docker run [OPTIONS] <image> [command]
```

**Key Options (`-itpd`):**

* `-i` → **interactive** (keeps STDIN open)
* `-t` → allocates a **pseudo-TTY** (lets you have a terminal in the container)
* `-p <host>:<container>` → maps **host port** to **container port** (needed to access services from outside)
* `-d` → **detached mode**, runs container in the background

> `-itpd` is just a combination of `-i -t -p -d`.

---

### **3. Examples for each image**

#### **Ubuntu**

```bash
docker run -it ubuntu:22.04 /bin/bash
```

* `-it` → get a shell inside Ubuntu
* No `-d` needed because we want interactive shell
* `/bin/bash` → start bash inside container

---

#### **Nginx**

```bash
docker run -d -p 8080:80 nginx:latest
```

* `-d` → run in background
* `-p 8080:80` → access Nginx via `http://localhost:8080`
* No `-it` needed (no interactive shell)

---

#### **Node**

```bash
docker run -it -p 3000:3000 node:20 /bin/bash
```

* `-it` → interactive bash
* `-p 3000:3000` → expose port 3000 for apps
* Can run Node app later:

```bash
node app.js
```

---

#### **PHP**

```bash
docker run -it -p 8000:80 php:8.2-apache /bin/bash
```

* `-p 8000:80` → Apache in container accessible on host port 8000
* Start web server with:

```bash
apache2-foreground
```

---

#### **MySQL**

```bash
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql:8.0
```

* `-d` → run in background
* `-p 3306:3306` → map MySQL port
* `-e MYSQL_ROOT_PASSWORD=root` → set root password

---

#### **MongoDB**

```bash
docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=secret mongo:6.0
```

* `-d` → background
* `-p 27017:27017` → expose MongoDB port
* `-e` → environment variables for credentials

---

#### **MinIO**

```bash
docker run -d -p 9000:9000 -p 9001:9001 \
  -e MINIO_ROOT_USER=minioadmin \
  -e MINIO_ROOT_PASSWORD=minioadmin \
  minio/minio server /data --console-address ":9001"
```

* `-p 9000:9000` → MinIO API port
* `-p 9001:9001` → web console port
* `-e` → root user & password
* `server /data` → run MinIO server storing data in `/data`

---

✅ **Summary of options:**

| Option | Meaning                      |
| ------ | ---------------------------- |
| `-i`   | Interactive, keep STDIN open |
| `-t`   | TTY, allocate terminal       |
| `-p`   | Port mapping host:container  |
| `-d`   | Detached, run in background  |

---

Perfect! Let’s expand on `docker run` and add **more useful options** you’ll often need when working with containers for Ubuntu, Nginx, Node, PHP, MySQL, MongoDB, MinIO. I’ll also explain what each does.

---

## **1. Core options we already know**

| Option | Meaning                           |
| ------ | --------------------------------- |
| `-i`   | Interactive (keep STDIN open)     |
| `-t`   | Allocate TTY (terminal)           |
| `-p`   | Map host port → container port    |
| `-d`   | Detached mode (run in background) |

---

## **2. Additional useful options**

| Option                               | Example                                 | Explanation                                          |
| ------------------------------------ | --------------------------------------- | ---------------------------------------------------- |
| `--name`                             | `--name my-ubuntu`                      | Give your container a custom name                    |
| `-v` / `--mount`                     | `-v /host/path:/container/path`         | Mount host directory to container (persistent data)  |
| `--rm`                               | `--rm`                                  | Automatically remove container when stopped          |
| `-e`                                 | `-e ENV=value`                          | Set environment variable inside container            |
| `--network`                          | `--network my-network`                  | Connect container to a Docker network                |
| `--restart`                          | `--restart unless-stopped`              | Automatically restart container on failure or reboot |
| `--cpu` / `--memory`                 | `--cpu 2 --memory 1g`                   | Limit container CPU or memory usage                  |
| `--env-file`                         | `--env-file ./env.list`                 | Load multiple env variables from file                |
| `--user`                             | `--user 1000:1000`                      | Run container as specific user                       |
| `--health-cmd` / `--health-interval` | Check container health (useful for DBs) |                                                      |

---

## **3. Example with all useful options**

### **Ubuntu**

```bash
docker run -it --name ubuntu-dev --rm -v ~/projects:/projects ubuntu:22.04 /bin/bash
```

* Interactive shell
* Named container `ubuntu-dev`
* Auto-remove when stopped
* Mount host `~/projects` to `/projects` in container

---

### **Nginx**

```bash
docker run -d --name webserver -p 8080:80 -v ~/nginx/conf:/etc/nginx/conf.d -v ~/nginx/html:/usr/share/nginx/html --restart unless-stopped nginx:latest
```

* Detached mode
* Named `webserver`
* Map port 8080 → 80
* Mount config & html from host
* Auto-restart if container stops

---

### **Node**

```bash
docker run -it --name node-app -p 3000:3000 -v ~/myapp:/usr/src/app -w /usr/src/app node:20 /bin/bash
```

* `-w` → set working directory
* Mount code folder to container
* Interactive bash shell

---

### **PHP (with Apache)**

```bash
docker run -d --name php-web -p 8000:80 -v ~/php-app:/var/www/html --restart unless-stopped php:8.2-apache
```

* Detached mode
* Mount web project folder
* Auto-restart

---

### **MySQL**

```bash
docker run -d --name mysql-db -p 3306:3306 \
  -v ~/mysql/data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=root \
  --restart unless-stopped \
  mysql:8.0
```

* Persist DB data on host
* Auto-restart

---

### **MongoDB**

```bash
docker run -d --name mongo-db -p 27017:27017 \
  -v ~/mongo/data:/data/db \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=secret \
  --restart unless-stopped \
  mongo:6.0
```

---

### **MinIO**

```bash
docker run -d --name minio \
  -p 9000:9000 -p 9001:9001 \
  -v ~/minio/data:/data \
  -e MINIO_ROOT_USER=minioadmin \
  -e MINIO_ROOT_PASSWORD=minioadmin \
  --restart unless-stopped \
  minio/minio server /data --console-address ":9001"
```

---

## ✅ **Tips**

* Always use `--name` → easier to manage containers (`docker stop mysql-db` etc.)
* Use `-v` → data won’t disappear after container stops
* Use `--restart unless-stopped` for services like DB or web server
* Combine `-it` with `/bin/bash` only when you want to enter the container

---

Ah! Now we’re moving from **running containers** to **managing containers**. Let me explain the most commonly used Docker commands for **viewing, stopping, removing, and inspecting containers and images**. I’ll include examples for your stack (Ubuntu, Nginx, Node, PHP, MySQL, MongoDB, MinIO).

---

## **1. List running containers: `docker ps`**

```bash
docker ps
```

* Shows **only running containers**
* Columns: `CONTAINER ID`, `IMAGE`, `COMMAND`, `STATUS`, `PORTS`, `NAMES`

**Useful options:**

* `-a` → show **all containers**, including stopped ones

```bash
docker ps -a
```

* `--filter` → filter by name, status, etc.

```bash
docker ps -a --filter "name=mysql-db"
```

---

## **2. Stop a running container: `docker stop`**

```bash
docker stop <container_name_or_id>
```

* Gracefully stops a running container
  **Example:**

```bash
docker stop mysql-db
docker stop node-app
```

---

## **3. Kill a container immediately: `docker kill`**

```bash
docker kill <container_name_or_id>
```

* Forcefully stops a container
* Useful if `docker stop` is too slow

---

## **4. Remove a container: `docker rm`**

```bash
docker rm <container_name_or_id>
```

* Deletes a container **(must be stopped first)**
* Remove all stopped containers:

```bash
docker rm $(docker ps -a -q)
```

---

## **5. Remove an image: `docker rmi`**

```bash
docker rmi <image_name_or_id>
```

* Deletes an image from your local machine
* Remove all unused images:

```bash
docker image prune -a
```

---

## **6. View images: `docker images`**

```bash
docker images
```

* Lists downloaded images
* Columns: `REPOSITORY`, `TAG`, `IMAGE ID`, `SIZE`

---

## **7. View container logs: `docker logs`**

```bash
docker logs <container_name_or_id>
```

* Shows stdout/stderr logs from container
* Follow logs in real-time:

```bash
docker logs -f nginx-web
```

---

## **8. Enter a running container: `docker exec`**

```bash
docker exec -it <container_name_or_id> /bin/bash
```

* `-it` → interactive bash inside container
* Example:

```bash
docker exec -it ubuntu-dev /bin/bash
docker exec -it node-app /bin/bash
```

---

## **9. Remove all stopped containers & unused resources**

```bash
docker system prune -a
```

* Deletes:

  * Stopped containers
  * Unused images
  * Unused networks
  * Unused build cache

---

## **10. Inspect a container**

```bash
docker inspect <container_name_or_id>
```

* Shows detailed JSON info:

  * IP address
  * Mounted volumes
  * Ports
  * Network settings

---

### **Quick Reference Commands**

| Task                    | Command                            |
| ----------------------- | ---------------------------------- |
| List running containers | `docker ps`                        |
| List all containers     | `docker ps -a`                     |
| Stop container          | `docker stop <name>`               |
| Kill container          | `docker kill <name>`               |
| Remove container        | `docker rm <name>`                 |
| Remove image            | `docker rmi <image>`               |
| List images             | `docker images`                    |
| Follow logs             | `docker logs -f <name>`            |
| Enter container         | `docker exec -it <name> /bin/bash` |
| Remove all unused       | `docker system prune -a`           |

---

