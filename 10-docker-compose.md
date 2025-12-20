### Why Docker Compose is useful

#### 1. One command to run everything

Instead of running many long `docker run` commands:

```bash
docker run php
docker run mysql
docker run nginx
```

You just run:

```bash
docker compose up
```

👉 This is **simpler and faster**, especially for backend projects like **Laravel**.

---

#### 2. Perfect for multi-service apps (like Laravel)

A Laravel app usually needs:

* PHP (app)
* MySQL / PostgreSQL (database)
* Redis (cache)
* Nginx (web server)

Docker Compose lets you define **all services in one file**:

```yaml
services:
  app:
  db:
  nginx:
```

They start, stop, and talk to each other automatically.

---

#### 3. Easy networking between containers

With Docker Compose:

* Services can connect using **service names**
* No need to manage IP addresses

Example:

```env
DB_HOST=db
```

👉 Works out of the box.

---

#### 4. Same setup for everyone (no “works on my machine”)

* Your laptop
* Your teammate’s laptop
* Server (Ubuntu)

All use the **same `docker-compose.yml`**

👉 This is why teams love Docker Compose.

---

#### 5. Easy environment configuration

You can define:

* Ports
* Environment variables
* Volumes

Example:

```yaml
ports:
  - "8000:80"
```

Change config without touching code.

---

#### 6. Great for development & testing

* Hot reload with volumes
* Reset database easily
* Start/stop services anytime

```bash
docker compose down
docker compose up -d
```

---

#### 7. Easier than Kubernetes (for small–medium apps)

* Docker Compose = **local dev & small servers**
* Kubernetes = **large-scale production**

👉 For learning, teaching, and most projects, **Compose is the right tool**.

---

### When you SHOULD use Docker Compose

✅ Laravel / Node / React full-stack apps
✅ Backend + DB + Cache
✅ Teaching Docker
✅ Small to medium production servers

### When you should NOT

❌ Massive microservices
❌ Auto-scaling across many servers

---

If you want, I can explain this:

* **Docker vs Docker Compose**
* **Laravel without Compose vs with Compose**
* **Real Laravel docker-compose example (simple)**

---

## 3️⃣ How Docker Compose works (concept)

```
docker-compose.yml
│
├── frontend (HTML, CSS, JS)
├── backend  (PHP)
├── database (MySQL)
│
└── shared network (auto-created)
```

### Key idea

* **Each service = one container**
* Services talk using **service name**

  * PHP connects to MySQL using `mysql`
  * Not `localhost`

---

## 4️⃣ Project structure (simple & clean)

```
docker-compose-demo/
│
├── docker-compose.yml
│
├── backend/
│   └── index.php
│
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── app.js
```

---

## 5️⃣ docker-compose.yml (Core File)

```yaml
version: "3.9"

services:
  php:
    image: php:8.2-apache
    container_name: php-backend
    volumes:
      - ./backend:/var/www/html
    ports:
      - "8000:80"
    depends_on:
      - mysql

  mysql:
    image: mysql:8.0
    container_name: mysql-db
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: demo_db
      MYSQL_USER: demo
      MYSQL_PASSWORD: demo
    volumes:
      - mysql_data:/var/lib/mysql
    ports:
      - "3306:3306"

  frontend:
    image: nginx:alpine
    container_name: frontend
    volumes:
      - ./frontend:/usr/share/nginx/html
    ports:
      - "8080:80"

volumes:
  mysql_data:
```

---

## 6️⃣ Backend (PHP) – Connect to MySQL

### `backend/index.php`

```php
<?php
$host = "mysql"; // service name
$db   = "demo_db";
$user = "demo";
$pass = "demo";

try {
    $pdo = new PDO(
        "mysql:host=$host;dbname=$db",
        $user,
        $pass
    );
    echo "✅ PHP connected to MySQL successfully";
} catch (PDOException $e) {
    echo "❌ Connection failed: " . $e->getMessage();
}
```

📌 **Important**

* ❌ `localhost` → wrong
* ✅ `mysql` → correct (service name)

---

## 7️⃣ Frontend (HTML / CSS / JS)

### `frontend/index.html`

```html
<!DOCTYPE html>
<html>
<head>
  <title>Frontend</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>Docker Compose Frontend</h1>
  <button onclick="callApi()">Call PHP API</button>
  <p id="result"></p>

  <script src="app.js"></script>
</body>
</html>
```

### `frontend/app.js`

```js
function callApi() {
  fetch("http://localhost:8000")
    .then(res => res.text())
    .then(data => {
      document.getElementById("result").innerText = data;
    });
}
```

### `frontend/style.css`

```css
body {
  font-family: sans-serif;
  padding: 40px;
}
```

---

## 8️⃣ Run the project (Step-by-step)

### Start all services

```bash
docker compose up -d
```

### Check containers

```bash
docker compose ps
```

### Stop everything

```bash
docker compose down
```

---

## 9️⃣ Access URLs

| Service     | URL                                            |
| ----------- | ---------------------------------------------- |
| Frontend    | [http://localhost:8080](http://localhost:8080) |
| PHP Backend | [http://localhost:8000](http://localhost:8000) |
| MySQL       | localhost:3306                                 |

---

## 🔟 Key teaching points (Very important)

### 🧠 Concepts students must remember

1. **One service = one container**
2. **Service name = hostname**
3. `depends_on` → startup order
4. Volumes → data persistence
5. Ports → host ↔ container

---

## 1️⃣1️⃣ Common mistakes (warn students)

❌ Using `localhost` inside containers
❌ Forgetting ports
❌ Not using volumes for MySQL
❌ Running containers manually instead of compose

---

## 1️⃣2️⃣ One-line summary (for students)

> **Docker Compose lets you define and run multi-container apps with one file and one command.**

---

Great question — this is a **real microservices-style Docker Compose setup**, perfect for **advanced teaching** 👍
I’ll explain **what/why/how**, then give a **clean working example** with:

* **Frontend** → Vite + Tailwind + TypeScript
* **Backend services**

  * **Product service** → PHP
  * **Order service** → Node.js
  * **Auth service** → Go

All connected via **Docker Compose network**.

---

# 1️⃣ Architecture (Big Picture)

```
Browser
   │
   ▼
Frontend (Vite + TS + Tailwind)
   │
   ├── /api/products → PHP service
   ├── /api/orders   → Node.js service
   └── /api/auth     → Go service
```

Each backend:

* runs in **its own container**
* communicates via **service name**
* independent language & runtime

This is **exactly how real companies structure systems**.

---

# 2️⃣ Project Structure

```
docker-compose-microservices/
│
├── docker-compose.yml
│
├── frontend/
│   ├── Dockerfile
│   ├── index.html
│   ├── src/
│   │   ├── main.ts
│   │   └── style.css
│   └── package.json
│
├── product-php/
│   └── index.php
│
├── order-node/
│   ├── Dockerfile
│   ├── index.js
│   └── package.json
│
└── auth-go/
    ├── Dockerfile
    └── main.go
```

---

# 3️⃣ docker-compose.yml (Core)

```yaml
version: "3.9"

services:
  frontend:
    build: ./frontend
    ports:
      - "5173:5173"
    depends_on:
      - product
      - order
      - auth

  product:
    image: php:8.2-apache
    volumes:
      - ./product-php:/var/www/html
    ports:
      - "8001:80"

  order:
    build: ./order-node
    ports:
      - "8002:3000"

  auth:
    build: ./auth-go
    ports:
      - "8003:8080"
```

📌 Docker Compose creates **one shared network automatically**
Services resolve each other by **service name**:

* `product`
* `order`
* `auth`

---

# 4️⃣ Product Service (PHP)

### `product-php/index.php`

```php
<?php
header("Content-Type: application/json");

echo json_encode([
  "service" => "product",
  "products" => [
    ["id" => 1, "name" => "Laptop"],
    ["id" => 2, "name" => "Phone"]
  ]
]);
```

Test:

```
http://localhost:8001
```

---

# 5️⃣ Order Service (Node.js)

### `order-node/package.json`

```json
{
  "name": "order-service",
  "version": "1.0.0",
  "main": "index.js",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

### `order-node/index.js`

```js
const express = require("express");
const app = express();

app.get("/orders", (req, res) => {
  res.json({
    service: "order",
    orders: [
      { id: 1, product: "Laptop" }
    ]
  });
});

app.listen(3000, () => {
  console.log("Order service running on port 3000");
});
```

### `order-node/Dockerfile`

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["node", "index.js"]
```

Test:

```
http://localhost:8002/orders
```

---

# 6️⃣ Auth Service (Go)

### `auth-go/main.go`

```go
package main

import (
	"encoding/json"
	"net/http"
)

func main() {
	http.HandleFunc("/login", func(w http.ResponseWriter, r *http.Request) {
		json.NewEncoder(w).Encode(map[string]string{
			"service": "auth",
			"status":  "logged in",
		})
	})

	http.ListenAndServe(":8080", nil)
}
```

### `auth-go/Dockerfile`

```dockerfile
FROM golang:1.22-alpine
WORKDIR /app
COPY . .
RUN go build -o auth
CMD ["./auth"]
```

Test:

```
http://localhost:8003/login
```

---

# 7️⃣ Frontend (Vite + Tailwind + TypeScript)

### `frontend/Dockerfile`

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["npm", "run", "dev", "--", "--host"]
```

### `frontend/package.json`

```json
{
  "name": "frontend",
  "scripts": {
    "dev": "vite"
  },
  "dependencies": {
    "vite": "^5.0.0"
  }
}
```

### `frontend/src/main.ts`

```ts
async function loadData() {
  const product = await fetch("http://localhost:8001").then(r => r.json());
  const order = await fetch("http://localhost:8002/orders").then(r => r.json());
  const auth = await fetch("http://localhost:8003/login").then(r => r.json());

  document.body.innerHTML = `
    <h1>Microservices Demo</h1>
    <pre>${JSON.stringify({ product, order, auth }, null, 2)}</pre>
  `;
}

loadData();
```

---

# 8️⃣ Run Everything

```bash
docker compose up --build
```

Open:

```
http://localhost:5173
```

---

# 9️⃣ Key Teaching Points (Important)

### 🧠 What students must understand

1. **Each backend = independent service**
2. **Different languages can live together**
3. Frontend talks via **HTTP**
4. Docker Compose = **local microservices**
5. This scales to Kubernetes later

---

# 🔟 One-line summary

> **Docker Compose lets you run multi-language microservices with one command and one network.**

---

Got it 👍
Below is a **simple, practical way to add Docker Compose directly into an existing Laravel project** (no restructuring headache).

I’ll assume:

* You already have a Laravel project
* You want **Nginx + PHP-FPM + MySQL**
* You want it **easy for local development**

---

# 1️⃣ Laravel Project Structure (After Adding Docker)

```txt
your-laravel-project/
├── app/
├── bootstrap/
├── config/
├── public/
├── resources/
├── routes/
├── storage/
├── docker/
│   ├── nginx/
│   │   └── default.conf
│   └── php/
│       └── Dockerfile
├── docker-compose.yml
├── .env
├── artisan
├── composer.json
└── package.json
```

✅ Docker files live **inside the Laravel repo**

---

# 2️⃣ docker-compose.yml (Root of Laravel)

```yaml
version: "3.9"

services:
  app:
    build:
      context: ./docker/php
    container_name: laravel_app
    volumes:
      - .:/var/www/html
    working_dir: /var/www/html
    networks:
      - laravel

  web:
    image: nginx:alpine
    container_name: laravel_nginx
    ports:
      - "8080:80"
    volumes:
      - .:/var/www/html
      - ./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - app
    networks:
      - laravel

  db:
    image: mysql:8.0
    container_name: laravel_db
    environment:
      MYSQL_DATABASE: laravel
      MYSQL_USER: laravel
      MYSQL_PASSWORD: secret
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - db_data:/var/lib/mysql
    ports:
      - "3306:3306"
    networks:
      - laravel

networks:
  laravel:

volumes:
  db_data:
```

---

# 3️⃣ PHP Dockerfile (`docker/php/Dockerfile`)

```dockerfile
FROM php:8.2-fpm

RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libzip-dev \
    && docker-php-ext-install pdo pdo_mysql zip

# Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html
```

---

# 4️⃣ Nginx Config (`docker/nginx/default.conf`)

```nginx
server {
    listen 80;
    root /var/www/html/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass app:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

---

# 5️⃣ Update Laravel `.env`

Change **only DB settings**:

```env
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

❌ `localhost`
✅ `db` (service name)

---

# 6️⃣ Add `.dockerignore` (Important)

```dockerignore
node_modules
vendor
.git
.gitignore
.env
```

> Speeds up build and avoids leaking secrets

---

# 7️⃣ Start Docker

```bash
docker compose up -d --build
```

---

# 8️⃣ First-Time Laravel Setup (Inside Container)

```bash
docker compose exec app composer install
docker compose exec app php artisan key:generate
docker compose exec app php artisan migrate
```

---

# 9️⃣ Access Your App

```
http://localhost:8080
```

---

# 🔟 Daily Development Commands

```bash
docker compose up -d
docker compose down
docker compose logs -f
docker compose exec app bash
```

---

# ✅ Why This Approach Is Best

✔ No change to Laravel code
✔ Works on Mac, Windows, Linux
✔ Easy for team onboarding
✔ Production-like environment
✔ You can add Redis / Queue later

---

If you want next:

* 🚀 **Laravel + Vite (hot reload)**
* 🔐 **Production Docker Compose**
* 📦 **One-command setup for new devs**
* ⚡ **Laravel Octane with Docker**

Tell me your goal and I’ll tailor it.

