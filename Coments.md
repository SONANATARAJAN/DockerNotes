 

# 🐳 Docker  

> **Purpose:**
> This document helps you **understand Docker clearly**, avoid confusion between **images vs containers**, handle **ports**, **rebuild safely**, and **run applications confidently**.

---

## 📌 Table of Contents

1. [Check Ports & Kill Conflicts](#-check-ports--kill-conflicts)
2. [Docker Basics – Commands You Must Know](#-docker-basics--commands-you-must-know)
3. [Images vs Containers (Most Important)](#-images-vs-containers-most-important)
4. [Running Containers with Ports](#-running-containers-with-ports)
5. [Logs & Debugging](#-logs--debugging)
6. [Restart & Start Containers](#-restart--start-containers)
7. [docker-compose Workflow](#-docker-compose-workflow)
8. [Clean Rebuild (Recommended Way)](#-clean-rebuild-recommended-way)
9. [Manual Run (Without Compose)](#-manual-run-without-compose)
10. [Verification & Cleanup](#-verification--cleanup)
11. [One-Line Mental Model](#-one-line-mental-model)

---

## 🔍 Check Ports & Kill Conflicts

If your app fails due to **port already in use**:

```bash
sudo lsof -i :9090
```

Kill the process:

```bash
kill -9 <PID>
```

Alternative:

```bash
netstat -tulpn | grep 9090
```

---

## 🐳 Docker Basics – Commands You Must Know

### 🔹 Containers

```bash
docker ps          # Running containers only
docker ps -a       # All containers (running + stopped)
```

### 🔹 Inspect container

```bash
docker inspect <container_name_or_id>
```

### 🔹 Stop / Start / Remove

```bash
docker stop <container>
docker start <container>
docker rm <container>
```

---

## 🧠 Images vs Containers (MOST IMPORTANT)

### 🚫 These are NOT the same

| Name                | Type         | Meaning              |
| ------------------- | ------------ | -------------------- |
| **blossom-backend** | 🧱 Image     | Blueprint / Template |
| **cygnet-api**      | 🚀 Container | Running application  |

---

### 📦 Docker Image = Template

* Read-only
* Built once
* Used many times

```bash
docker build -t blossom-backend .
```

---

### 🏃 Docker Container = Running Instance

* Created from image
* Has ports, memory, logs

```bash
docker run -d --name cygnet-api -p 8080:8080 blossom-backend
```

---

### 🧠 Think Like This

```
Image = Class
Container = Object
```

---

## 🔌 Running Containers with Ports

```bash
docker run -d \
  -p 9090:8080 \
  --name spring_app \
  my-app-image
```

**Format:**

```
HOST_PORT : CONTAINER_PORT
```

---

## 📜 Logs & Debugging

```bash
docker logs spring_app
docker logs -f spring_app
```

---

## 🔁 Restart & Start Containers

```bash
docker restart cygnet-ui cygnet-api
docker start cygnet-ui cygnet-api
```

---

## 🧩 docker-compose Workflow

### Start everything

```bash
docker compose up -d
```

### Stop everything

```bash
docker compose down
```

---

## 🔥 Clean Rebuild (RECOMMENDED WAY)

> Use this when **code changes** or **old image is still running**

### ✅ Step 1: Stop containers

```bash
docker stop cygnet-api cygnet-ui mysql-db
```

### ✅ Step 2: Remove containers

```bash
docker rm cygnet-api cygnet-ui mysql-db
```

### ✅ Step 3: Remove old images

```bash
docker rmi blossom-backend blossom-frontend
docker rmi -f blossom-backend blossom-frontend
```

### ✅ Step 4: Rebuild fresh (no cache)

```bash
docker compose build --no-cache
```

### ✅ Step 5: Start new containers

```bash
docker compose up -d
```

---

## 🛠 Manual Run (Without Compose)

### MySQL

```bash
docker run -d --name mysql-db \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=root \
  mysql:8.0
```

### Backend

```bash
docker run -d --name cygnet-api \
  -p 8080:8080 \
  blossom-backend
```

### Frontend

```bash
docker run -d --name cygnet-ui \
  -p 5173:80 \
  blossom-frontend
```

---

## 🔍 Verification & Cleanup

### Check running containers

```bash
docker ps
```

### Clean unused stuff

```bash
docker system prune -f
docker system prune -a
```

---

## 🧪 Clean JAR Build (Spring Boot)

```bash
./gradlew clean bootJar
```

Verify:

```bash
ls build/libs
```

Check resource inside JAR:

```bash
jar tf build/libs/*.jar | grep queries.xml
```

---

## 🧠 One-Line Mental Model

```
Docker Image  → What to run
Docker Container → What is running
Ports → Door between host & container
docker-compose → Run everything together
```

---

## ✅ Final Tip (Very Important)

> ❌ **Never trust an old container**
> ✅ **Always rebuild images after code changes**

 
