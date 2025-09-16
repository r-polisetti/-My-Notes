
# 🐳 Docker Notes

## Quick Reference Table

| Task                          | Command Example |
|-------------------------------|-----------------|
| Build image                   | `docker build -t myapp .` |
| Build w/o cache               | `docker build -t myapp . --no-cache` |
| List images                   | `docker images` |
| Remove image                  | `docker rmi myapp` |
| Prune unused images           | `docker image prune` |
| Login to Docker Hub           | `docker login -u myuser` |
| Push image                    | `docker push myuser/myapp` |
| Pull image                    | `docker pull ubuntu:20.04` |
| Search image                  | `docker search nginx` |
| Run container                 | `docker run --name mycontainer ubuntu` |
| Run with port mapping         | `docker run -p 8080:80 nginx` |
| Run in background             | `docker run -d nginx` |
| Start / Stop container        | `docker start|stop mycontainer` |
| Remove container              | `docker rm mycontainer` |
| Exec into container           | `docker exec -it mycontainer bash` |
| Logs                          | `docker logs -f mycontainer` |
| Inspect container             | `docker inspect mycontainer` |
| List running containers       | `docker ps` |
| List all containers           | `docker ps -a` |
| Resource usage stats          | `docker stats` |
| Create volume                 | `docker volume create myvol` |
| Mount volume                  | `docker run -v myvol:/data busybox` |
| List networks                 | `docker network ls` |
| Create network                | `docker network create mynet` |
| Prune system                  | `docker system prune` |
| Docker help                   | `docker --help` |
| Docker info                   | `docker info` |

---

## Table of Contents
- [CLI Cheat Sheet](#cli-cheat-sheet)
- [Concepts](#concepts)
- [Dockerfile Basics](#dockerfile-basics)
- [Docker Compose](#docker-compose)
- [Troubleshooting](#troubleshooting)
- [Installation & Resources](#installation--resources)
- [Best Practices](#best-practices)

---

## CLI Cheat Sheet

### Build Images
```bash
docker build -t myapp:latest .
docker build -t myapp:latest . --no-cache
````

### Manage Images

```bash
docker images
docker rmi myapp:latest
docker image prune
```

### Docker Hub

```bash
docker login -u myusername
docker push myusername/myapp:latest
docker search ubuntu
docker pull ubuntu:20.04
```

### Containers

```bash
docker run --name mycontainer ubuntu
docker run -p 8080:80 nginx
docker run -d nginx
docker start mycontainer
docker stop mycontainer
docker rm mycontainer
docker exec -it mycontainer bash
docker logs -f mycontainer
docker inspect mycontainer
docker ps
docker ps -a
docker container stats
```

### Volumes

```bash
docker volume create myvolume
docker run -v myvolume:/data busybox
docker volume ls
```

### Networks

```bash
docker network ls
docker network create mynetwork
docker run -d --network=mynetwork nginx
```

### System Cleanup

```bash
docker system prune
```

### General Commands

```bash
docker -d
docker --help
docker info
```

---

## Concepts

### Images

Lightweight, standalone packages that contain everything needed to run an application.
Example:

```bash
docker pull python:3.12
```

### Containers

Runtime instances of images. Portable, isolated, and consistent.
Example:

```bash
docker run -it python:3.12 python
```

### Volumes

Persistent storage for containers.
Example:

```bash
docker run -v myvolume:/app/data busybox
```

### Networks

Allow containers to communicate with each other.
Example:

```bash
docker network create backend
docker run -d --network=backend --name=db mysql:8
docker run -d --network=backend --name=app myapp
```

### Docker Hub

Registry for finding and sharing container images.
🔗 [Docker Hub](https://hub.docker.com)

---

## Dockerfile Basics

A **Dockerfile** is a script with instructions to build images.

### Common Instructions

* **FROM** → Base image
* **RUN** → Execute commands during build
* **COPY** → Copy files into the image
* **WORKDIR** → Set working directory
* **ENV** → Define environment variables
* **EXPOSE** → Document listening ports
* **CMD** → Default command to run
* **ENTRYPOINT** → Main executable for the container

### Example: Simple Node.js App

```dockerfile
# Use official Node.js image
FROM node:18

# Set working directory
WORKDIR /app

# Copy package.json and install deps
COPY package*.json ./
RUN npm install

# Copy rest of the code
COPY . .

# Expose app port
EXPOSE 3000

# Start the app
CMD ["npm", "start"]
```

Build & run:

```bash
docker build -t mynodeapp .
docker run -p 3000:3000 mynodeapp
```

### Example: Multi-Stage Build (optimized)

```dockerfile
# Stage 1: Build app
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

# Stage 2: Create minimal runtime image
FROM alpine:3.18
WORKDIR /app
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

---

## Docker Compose

Define and run multi-container applications with a `docker-compose.yml`.

### Example

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: example
```

Run:

```bash
docker-compose up -d
docker-compose down
```

---

## Troubleshooting

* **Port already in use**

  ```bash
  docker ps -a
  docker stop <container_id>
  docker rm <container_id>
  ```

* **Permission denied with volumes**

  ```bash
  sudo chown -R $USER:$USER ./data
  ```

* **Clean dangling volumes**

  ```bash
  docker volume prune
  ```

* **Remove all containers and images (⚠️ destructive)**

  ```bash
  docker rm -f $(docker ps -aq)
  docker rmi -f $(docker images -q)
  ```

* **Check detailed logs**

  ```bash
  docker inspect <container_name>
  ```

---

## Installation & Resources

* [Docker Desktop (Mac, Linux, Windows)](https://docs.docker.com/desktop)
* [Awesome Compose Examples](https://github.com/docker/awesome-compose)
* [Docker Documentation](https://docs.docker.com)
* [Play with Docker](https://labs.play-with-docker.com/)

---

## Best Practices

* Use `.dockerignore` to reduce build size
* Use **multi-stage builds** for lean images
* Prefer `COPY` over `ADD` unless necessary
* Keep containers **single-purpose**
* Tag images properly (`myapp:1.0.0` instead of `latest`)
* Regularly prune unused resources (`docker system prune`)
* Scan images for vulnerabilities (`docker scan myimage`)

---

```

