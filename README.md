# Spring Boot + Docker + Nginx (Reverse-Proxy)
---
# 🚀 Spring Boot Application Deployment using Docker & AWS EC2

## 📌 Overview

This project demonstrates how to deploy a **Spring Boot application** using **Docker, Docker Compose**, and **AWS EC2**.
It includes a **MySQL database container** and a **Spring Boot application container** connected via Docker network.

---

## 🧱 Tech Stack

* Java 17
* Spring Boot
* MySQL 8
* Docker
* Docker Compose
* AWS EC2 (Ubuntu)

---

## 📂 Project Structure

```
project-root/
│
├── src/                         # Spring Boot source code
├── pom.xml                      # Maven configuration
├── Dockerfile                   # Multi-stage build
├── docker-compose.yml           # Container orchestration
├── .env                         # Environment variables
├── README.md                    # Documentation
└── .gitignore
```

---

## ⚙️ Prerequisites

Before starting, ensure:

* AWS EC2 instance (Ubuntu)
* SSH access to EC2
* Git installed
* Docker & Docker Compose installed

---

## 🚀 Deployment Steps

### 🔹 Step 1: Launch EC2 Instance

* Choose Ubuntu OS
* Open ports:

  * 22 (SSH)
  * 8081 (Application)

---

### 🔹 Step 2: Update System

```bash
sudo apt update
```

---

### 🔹 Step 3: Install Docker & Docker Compose

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker

sudo apt install docker-compose -y
```

(Optional)

```bash
sudo usermod -aG docker ubuntu
```

---

### 🔹 Step 4: Clone Repository

```bash
git clone <your-repo-url>
cd your-project
```

---

### 🔹 Step 5: Create `.env` File

```env
MYSQL_ROOT_PASSWORD=1234
MYSQL_DATABASE=myapplication
APP_PORT=8081
```

---

### 🔹 Step 6: Build & Run Containers

```bash
docker compose up --build -d
```

---

### 🔹 Step 7: Verify Containers

```bash
docker ps
```

---

### 🔹 Step 8: Check Images

```bash
docker images
```

---

## 🌐 Access Application

Open in browser:

```
http://<EC2-PUBLIC-IP>:8081
```

---

## 🔗 How It Works

* Docker Compose creates:

  * MySQL container (`db`)
  * Spring Boot container (`app`)
* Both containers communicate via **Docker network**
* Application connects to DB using:

  ```
  jdbc:mysql://db:3306/<database>
  ```

---

## ⚠️ Common Issues & Fixes

### ❌ Application not starting

* Check logs:

```bash
docker logs app_container
```

---

### ❌ Database connection error

* Ensure:

  * Correct DB URL (`db`, not localhost)
  * DB name matches `.env`

---

### ❌ Port not accessible

* Check EC2 security group
* Ensure correct port mapping

---

## 🔄 Useful Commands

```bash
# Stop containers
docker compose down

# Restart
docker compose restart

# View logs
docker compose logs -f

# Rebuild fresh
docker compose build --no-cache
docker compose up -d
```

---

## 🔒 Best Practices

* Do not commit `.env` file (add to `.gitignore`)
* Avoid exposing MySQL port in production
* Use environment variables for configuration
* Use multi-stage Docker builds for smaller images

---

## 🚀 Future Improvements

* Add Nginx reverse proxy
* Enable HTTPS (SSL)
* Use managed database (AWS RDS)
* Setup CI/CD pipeline (GitHub Actions / Jenkins)

---

## 🧠 Learning Outcome

* Containerization using Docker
* Multi-container orchestration
* Cloud deployment using EC2
* Service-to-service communication in Docker

---

## 👨‍💻 Author

**Dheeru**

---

## ⭐ If you like this project

Give it a ⭐ on GitHub!



















<img width="1132" height="774" alt="image" src="https://github.com/user-attachments/assets/461d49db-abe8-476c-9dad-544b8a285d41" />

---
## Configuration Details
### `application.properties`
```properties
spring.datasource.url=jdbc:mysql://mysql-container:3306/myapplication?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=1234
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```
---
## admin login
- username: admin
- password: admin
---
## nginx configuration
---
### Create Nginx config: On the Nginx server:
```bash
sudo vi /etc/nginx/sites-available/reverse-proxy
```
- Paste this:
```bash
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name _;

    location / {
        proxy_pass http://13.235.57.138:8081;
        proxy_http_version 1.1;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
### Enable config
```bash
sudo rm /etc/nginx/sites-enabled/default
sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
---
*Script Done by SAK*
