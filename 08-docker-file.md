## **1. What is a Dockerfile?**

A **Dockerfile** is a **text file** with a set of instructions that tells Docker **how to build an image** for your application.

* **Image** = blueprint for your container (like a snapshot of an environment).
* **Container** = running instance of that image.

Think of it as a recipe to create a **portable environment** that works the same everywhere.

---

## **2. Why use Dockerfile?**

* Ensures **consistency**: your app runs the same on any machine.
* Makes **deployment easier**: you don’t worry about missing dependencies.
* Enables **isolation**: your app doesn’t affect the host OS.
* Supports **automation**: build, test, and deploy pipelines.

---

## **3. How Dockerfile works**

Docker reads commands **line by line**, creates an image layer for each command, and builds a final image.

**Basic commands**:

| Command      | Purpose                                                 |
| ------------ | ------------------------------------------------------- |
| `FROM`       | Base image, e.g., `ubuntu`, `node`, `php`               |
| `WORKDIR`    | Set working directory inside container                  |
| `COPY`       | Copy files from host to container                       |
| `ADD`        | Like COPY but supports URLs & tar extraction            |
| `RUN`        | Run a shell command inside the image (install packages) |
| `CMD`        | Default command to run when container starts            |
| `ENTRYPOINT` | Like CMD, but more rigid, used to run main app          |
| `ENV`        | Set environment variable                                |
| `EXPOSE`     | Expose a port from the container                        |
| `VOLUME`     | Mount a volume for persistent data                      |
| `USER`       | Specify which user to run as                            |

---

## **4. Example: Static Website**

Let’s say you have an `index.html` file.

**Dockerfile**:

```dockerfile
# 1. Use a lightweight web server image
FROM nginx:alpine

# 2. Copy your static files to nginx's html folder
COPY ./index.html /usr/share/nginx/html/index.html

# 3. Expose port 80
EXPOSE 80

# 4. Default command is already nginx, so no need to override
```

**Build & run**:

```bash
docker build -t static-web .
docker run -p 8080:80 static-web
```

Visit **[http://localhost:8080](http://localhost:8080)**.

---

## **5. Example: React Development App (Node Server)**

For development:

**Dockerfile**:

```dockerfile
# 1. Base Node image
FROM node:20-alpine

# 2. Set working directory
WORKDIR /app

# 3. Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# 4. Copy the rest of the app
COPY . .

# 5. Expose React dev server port
EXPOSE 3000

# 6. Start the React dev server
CMD ["npm", "start"]
```

**Run**:

```bash
docker build -t react-dev .
docker run -p 3000:3000 react-dev
```

You now have **hot-reload development** in Docker.

---

## **6. Example: React Production Build**

For production, we **build the app first**, then serve it using Nginx.

**Dockerfile**:

```dockerfile
# Step 1: Build the React app
FROM node:20-alpine AS build

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Step 2: Serve with Nginx
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Build & run**:

```bash
docker build -t react-prod .
docker run -p 8080:80 react-prod
```

✅ This is **multi-stage build**, reducing image size and only keeping the final static files.

---

## **7. Example: Laravel App**

Laravel needs **PHP, Composer, and a web server** like Apache/Nginx.

**Dockerfile**:

```dockerfile
# 1. Use PHP + Apache image
FROM php:8.2-apache

# 2. Install system dependencies & PHP extensions
RUN apt-get update && apt-get install -y \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    git \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# 3. Enable Apache mod_rewrite
RUN a2enmod rewrite

# 4. Set working directory
WORKDIR /var/www/html

# 5. Copy Laravel project
COPY . .

# 6. Install Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
RUN composer install --no-dev --optimize-autoloader

# 7. Set permissions
RUN chown -R www-data:www-data /var/www/html/storage /var/www/html/bootstrap/cache

# 8. Expose port 80
EXPOSE 80

# 9. Start Apache
CMD ["apache2-foreground"]
```

**Optional docker-compose.yml** (for MySQL):

```yaml
version: "3.8"
services:
  app:
    build: .
    ports:
      - "8000:80"
    volumes:
      - .:/var/www/html
    depends_on:
      - db

  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: laravel
      MYSQL_USER: user
      MYSQL_PASSWORD: secret
    ports:
      - "3306:3306"
```

---

## ✅ Summary of Steps to Write a Dockerfile

1. **Choose base image** (`FROM`).
2. **Set working directory** (`WORKDIR`).
3. **Install dependencies** (`RUN`).
4. **Copy your code** (`COPY`/`ADD`).
5. **Expose necessary ports** (`EXPOSE`).
6. **Set default command** (`CMD`/`ENTRYPOINT`).
7. **Optional optimizations**: multi-stage builds, caching, volumes.

---
Perfect! Let’s go **step by step** on using **Dockerfile with Docker Hub**, including **build, push, pull, and run**. I’ll break it into a clear workflow.


---

## **1. Write a Dockerfile**

Let’s take a **simple static web example** (`index.html`):

**Dockerfile**:

```dockerfile
FROM nginx:alpine
COPY ./index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

---

## **2. Build the Docker Image**

Navigate to your project folder (where Dockerfile exists) and run:

```bash
docker build -t username/static-web:1.0 .
```

**Explanation:**

* `docker build` → build image from Dockerfile.
* `-t username/repo:tag` → tag the image. Replace `username` with your Docker Hub username.
* `.` → current folder (context).

Check the image:

```bash
docker images
```

You should see:

```
REPOSITORY           TAG       IMAGE ID       SIZE
username/static-web  1.0       <id>          5MB
```

---

## **3. Log in to Docker Hub**

```bash
docker login
```

* Enter your **Docker Hub username & password**.
* Successful login allows you to **push images**.

---

## **4. Push Image to Docker Hub**

```bash
docker push username/static-web:1.0
```

* Uploads the image to your Docker Hub repository.
* You can check it on [hub.docker.com/repositories](https://hub.docker.com/repositories).

---

## **5. Pull Image from Docker Hub**

On any machine with Docker:

```bash
docker pull username/static-web:1.0
```

* Downloads the image from Docker Hub to your local system.

---

## **6. Run the Container**

```bash
docker run -d -p 8080:80 username/static-web:1.0
```

**Explanation:**

* `-d` → run in detached mode (background).
* `-p hostPort:containerPort` → map host port 8080 to container port 80.
* `username/static-web:1.0` → image name + tag.

Check running container:

```bash
docker ps
```

You should see the container running. Open **[http://localhost:8080](http://localhost:8080)** in a browser.

---

## **7. Optional Commands**

* **Stop container**:

```bash
docker stop <container_id>
```

* **Remove container**:

```bash
docker rm <container_id>
```

* **Remove image**:

```bash
docker rmi username/static-web:1.0
```

* **List images**:

```bash
docker images
```

---

## ✅ **Workflow Summary**

1. Create **Dockerfile**.
2. `docker build -t username/repo:tag .` → build image.
3. `docker login` → authenticate with Docker Hub.
4. `docker push username/repo:tag` → upload image.
5. `docker pull username/repo:tag` → download image.
6. `docker run -d -p hostPort:containerPort username/repo:tag` → run container.

---



## **1. What is “state” in a container?**

The **state** of a container is basically the **data and changes that happen while the container is running**. This includes:

* Files created or modified inside the container
* Application data, logs, temporary files
* Running processes and memory (RAM)

By default, **containers are stateless**: when you stop or delete a container, all the changes inside it are lost. Only the original image remains unchanged.

---

## **2. Why statelessness is important**

* **Consistency**: Every time you start a container from the same image, it behaves the same.
* **Immutability**: Helps with deployment and scaling because you can replace containers safely.
* **Easy to recover**: You can destroy a container and spin up a new one without worrying about leftover data.

---

## **3. How to handle state in Docker**

If your app needs **persistent data**, Docker provides a few options:

### **a) Volumes (Recommended)**

* A volume is a directory on the host machine managed by Docker.
* It persists even if the container is deleted.

```bash
docker run -d -v mydata:/app/data my-app
```

* `/app/data` in container maps to `mydata` volume on host.

### **b) Bind Mounts**

* Maps a **specific folder on your host machine** into the container.

```bash
docker run -d -v /home/user/data:/app/data my-app
```

* Useful for local development so changes on host are reflected in container.

### **c) Database / External Storage**

* For apps like MySQL, Postgres, etc., you usually run a container for the database **and store its data in a volume**, so state is preserved even if the container restarts.

---

## **4. Ephemeral vs Persistent Containers**

| Type                | Data survives container restart? | Use Case                      |
| ------------------- | -------------------------------- | ----------------------------- |
| Ephemeral (default) | ❌ No                             | Stateless apps, microservices |
| Persistent          | ✅ Yes (using volumes)            | Databases, user uploads, logs |

---

💡 **Rule of thumb:**

* Treat containers as **stateless**.
* Keep state in **volumes or external services**.
* This makes scaling and updating containers easy.

---


