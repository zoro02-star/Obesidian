## 1. What Docker Actually Is

**Docker** is a platform that packages an application and all its dependencies into a **container** so it runs the same everywhere.

Think of it like:

App + Dependencies + Runtime + Config = Container

This solves the classic problem:

> “It works on my machine but not on the server.”

Unlike virtual machines:

|Feature|Virtual Machine|Docker Container|
|---|---|---|
|OS|Full OS|Shares host OS kernel|
|Size|GBs|MBs|
|Boot Time|Minutes|Seconds|
|Isolation|Strong|Lightweight|

---

# 2. Core Docker Concepts

## Image

A **Docker Image** is a blueprint for a container.

Example:

node:20  
python:3.12  
nginx  
postgres

Images are **read-only templates**.

Example:

docker pull node

---

## Container

A **container** is a running instance of an image.

Image → Container

Example:

docker run nginx

Now nginx is running inside a container.

---

## Dockerfile

A **Dockerfile** defines how to build an image.

Example:

FROM node:20  
  
WORKDIR /app  
  
COPY package.json .  
  
RUN npm install  
  
COPY . .  
  
CMD ["npm","start"]

Steps explained:

|Instruction|Meaning|
|---|---|
|FROM|base image|
|WORKDIR|working directory|
|COPY|copy files|
|RUN|execute command|
|CMD|run when container starts|

---

# 3. Docker Architecture

Docker Client  
      |  
      v  
Docker Daemon  
      |  
      v  
Images → Containers

Components:

|Component|Purpose|
|---|---|
|Docker Client|CLI you run commands from|
|Docker Daemon|background service|
|Images|templates|
|Containers|running instances|
|Registry|stores images|

Public registry example:

- Docker Hub
    

---

# 4. Basic Docker Commands

## Check installation

docker --version

---

## Pull image

docker pull nginx

---

## Run container

docker run nginx

---

## Run with port mapping

docker run -p 8080:80 nginx

Meaning:

host_port : container_port

Open:

http://localhost:8080

---

## List containers

Running:

docker ps

All:

docker ps -a

---

## Stop container

docker stop <container_id>

---

## Remove container

docker rm <container_id>

---

# 5. Building Your Own Image

Example project:

app/  
  server.js  
  package.json  
  Dockerfile

Build image:

docker build -t myapp .

Run it:

docker run -p 3000:3000 myapp

---

# 6. Volumes (Very Important)

Containers are **ephemeral**.

If container dies → data lost.

Solution: **Volumes**

Example:

docker run -v /data:/app/data myapp

Meaning:

host folder → container folder

For your recon framework this will be useful:

/recon-output → container

Example:

docker run -v ~/recon:/data recon-dashboard

---

# 7. Docker Networking

Containers communicate via networks.

Default types:

|Network|Purpose|
|---|---|
|bridge|container to container|
|host|share host network|
|none|isolated|

Example:

Frontend → Backend → Database

Containers talk internally like:

http://backend:8000

---

# 8. Docker Compose (Very Important)

For multi-container apps.

Example stack:

React frontend  
FastAPI backend  
PostgreSQL database

Use:

- Docker Compose
    

Example `docker-compose.yml`:

version: "3"  
  
services:  
  
  frontend:  
    build: ./frontend  
    ports:  
      - "3000:3000"  
  
  backend:  
    build: ./backend  
    ports:  
      - "8000:8000"  
  
  db:  
    image: postgres  
    environment:  
      POSTGRES_PASSWORD: secret

Run everything:

docker compose up

Stop:

docker compose down

---

# 9. Docker for Your Recon Dashboard

Your future architecture could look like:

Bug Bounty Recon Platform

┌───────────────┐  
│   Frontend    │  
│   React UI    │  
└───────┬───────┘  
        │ API  
┌───────▼───────┐  
│   Backend     │  
│  FastAPI      │  
│  Recon Parser │  
└───────┬───────┘  
        │  
┌───────▼────────┐  
│   Database     │  
│  PostgreSQL    │  
└────────────────┘

Docker containers:

frontend  
backend  
database  
redis (optional)

---

# 10. Example Folder Structure

For your tool:

recon-dashboard  
│  
├── docker-compose.yml  
│  
├── frontend  
│   └── React app  
│  
├── backend  
│   └── FastAPI  
│  
├── parser  
│   └── recon data ingestion  
│  
└── data  
    └── mounted recon files

---

# 11. Useful Docker Commands for Developers

View logs:

docker logs container_id

Open shell in container:

docker exec -it container_id bash

See images:

docker images

Remove image:

docker rmi image_id

---

# 12. Docker Security Basics

Important for bug bounty tools.

Never run containers as root.

Example:

RUN adduser appuser  
USER appuser

Scan images:

- Trivy
    

Limit container resources:

--memory  
--cpu

---

# 13. When to Use Docker

Use Docker when:

- building apps
    
- testing environments
    
- running microservices
    
- bug bounty tools
    
- CI/CD pipelines
    

Many security tools run via Docker.

Example:

- OWASP ZAP
    
- Faraday
    
- Metasploit Framework