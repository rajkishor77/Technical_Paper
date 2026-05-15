# 🐳 Docker — Complete Study Notes
> **After reading this, every Docker concept will be crystal clear.**
> Last Updated: May 2026 | Research-backed & Production-ready

---

## 📌 Table of Contents
1. [What is Docker?](#1-what-is-docker)
2. [Before Docker — The Problem](#2-before-docker--the-problem)
3. [Docker vs Virtual Machines](#3-docker-vs-virtual-machines)
4. [Docker Architecture](#4-docker-architecture)
5. [Core Components](#5-core-components)
6. [Dockerfile — Complete Reference](#6-dockerfile--complete-reference)
7. [Docker Images — In Depth](#7-docker-images--in-depth)
8. [Docker Containers — In Depth](#8-docker-containers--in-depth)
9. [Docker Networking](#9-docker-networking)
10. [Docker Volumes & Storage](#10-docker-volumes--storage)
11. [Docker Compose](#11-docker-compose)
12. [Multi-Stage Builds](#12-multi-stage-builds)
13. [Docker Hub & Registries](#13-docker-hub--registries)
14. [Docker Security](#14-docker-security)
15. [Docker in CI/CD](#15-docker-in-cicd)
16. [All Docker Commands — Explained](#16-all-docker-commands--explained)
17. [Best Practices](#17-best-practices)
18. [Docker vs Alternatives](#18-docker-vs-alternatives)
19. [Common Interview Questions](#19-common-interview-questions)

---

## 1. What is Docker?

Docker is an **OS-level virtualization (containerization) platform** that allows you to package an application and all its dependencies into a single, portable unit called a **container**. Containers share the host operating system's kernel — making them far lighter and faster than traditional virtual machines.

### Simple Analogy 📦
> Think of Docker like a **shipping container**.
> - Before containers, ships loaded cargo item by item (different shapes, sizes, risks).
> - After standard shipping containers, any cargo goes in one box — loaded once, transported anywhere, unloaded perfectly.
> - Docker does the same for software: one container, runs anywhere.

### Why Docker in 2026?
- Container usage has reached **~92% of IT professionals** (Docker State of App Development Report, 2025)
- Docker commands **~42.77% of the DevOps tech stack share**
- Modern employers expect at least working knowledge of Docker for full-stack and backend roles

---

## 2. Before Docker — The Problem

### The "Works on My Machine" Problem
```
Developer's Laptop:  Python 3.9, Django 4.2, PostgreSQL 14  ✅ Works
Test Server:         Python 3.7, Django 3.0, PostgreSQL 12  ❌ Crashes
Production:          Python 3.11, Django 5.0, PostgreSQL 15  ❌ Crashes differently
```

### Problems Before Docker
- Different OS versions and library versions across environments
- Dependency conflicts between projects on the same machine
- Manual, error-prone environment setup on every new server
- "It worked in dev" but fails in production — constantly

### Docker's Solution
Docker bundles the **application code + its exact runtime + its exact dependencies** into one container. That container runs identically everywhere — developer laptop, test server, production cloud, CI pipeline. No more surprises.

---

## 3. Docker vs Virtual Machines

### Visual Comparison
```
┌───────────────────────────────┐   ┌─────────────────────────────────────────┐
│       Virtual Machines         │   │            Docker Containers             │
├───────────────────────────────┤   ├─────────────────────────────────────────┤
│  App A  │  App B  │  App C    │   │  Container A │ Container B │ Container C │
├─────────┼─────────┼───────────┤   ├─────────────────────────────────────────┤
│ Guest   │ Guest   │ Guest     │   │          Docker Engine (Runtime)         │
│  OS     │  OS     │  OS       │   ├─────────────────────────────────────────┤
├─────────┴─────────┴───────────┤   │           Host Operating System         │
│         Hypervisor            │   ├─────────────────────────────────────────┤
├───────────────────────────────┤   │               Host Hardware             │
│      Host Operating System    │   └─────────────────────────────────────────┘
├───────────────────────────────┤
│           Hardware            │
└───────────────────────────────┘
```

### Feature Comparison

| Feature | Virtual Machine | Docker Container |
|---------|----------------|-----------------|
| OS | Each VM has its own full OS | Shares host OS kernel |
| Size | GBs (includes full OS) | MBs (just app + libs) |
| Startup Time | Minutes | Seconds (often milliseconds) |
| Performance | Slower (hardware emulation) | Near-native speed |
| Isolation | Full hardware-level isolation | Process-level isolation |
| Portability | Harder to move (large files) | Very portable (lightweight) |
| Resource Usage | High (full OS per app) | Low (shared kernel) |
| Use Case | Different OS environments | Same OS, multiple apps |

### When to Use Which?
- **Docker** — microservices, CI/CD pipelines, cloud deployments, horizontal scaling
- **VM** — running a completely different OS (e.g., Windows on Linux), or when strong hardware-level isolation is required

---

## 4. Docker Architecture

Docker uses a **client-server architecture**.

```
┌─────────────────────────────────────────────────────────────────┐
│                        Docker Client (CLI)                       │
│   docker build  │  docker run  │  docker pull  │  docker push   │
└─────────────────────────────┬───────────────────────────────────┘
                              │  REST API (UNIX socket / TCP)
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                      Docker Daemon (dockerd)                     │
│                                                                  │
│  ┌────────────┐  ┌─────────────┐  ┌──────────┐  ┌───────────┐  │
│  │   Images   │  │ Containers  │  │ Networks │  │  Volumes  │  │
│  └────────────┘  └─────────────┘  └──────────┘  └───────────┘  │
│                     (containerd / runc)                          │
└─────────────────────────────┬───────────────────────────────────┘
                              │  push / pull
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                   Docker Registry (Docker Hub)                   │
│               (stores and distributes images)                    │
└─────────────────────────────────────────────────────────────────┘
```

### How It Works — Step by Step
1. You type `docker run nginx` in the terminal
2. The **Docker Client** sends this command to the **Docker Daemon** via REST API
3. The Daemon checks if the `nginx` image exists locally
4. If not, it **pulls** the image from Docker Hub (registry)
5. The Daemon creates and starts a **container** from that image
6. The container runs as an isolated process on the host machine

---

## 5. Core Components

### 5.1 Docker Engine
The heart of Docker. It has three parts:
- **Docker Daemon (`dockerd`)** — background service managing images, containers, networks, and volumes
- **Docker CLI (Client)** — command-line tool that sends commands to daemon via REST API
- **REST API** — the interface between client and daemon

### 5.2 Dockerfile
A plain text file with instructions to **build a Docker image**. Written in Docker's DSL (Domain Specific Language). The daemon reads it top-to-bottom and executes each instruction as a new **layer**.

### 5.3 Docker Image
- A **read-only, layered template** used to create containers
- Contains: OS base layer + app runtime + dependencies + app code + config files
- Think of it as a **blueprint** or a **class** in OOP
- Images are built from Dockerfiles and stored in registries
- Multiple containers can be started from the same image simultaneously

### 5.4 Docker Container
- A **running instance** of a Docker image — like an object created from a class
- Lightweight, isolated process with its own filesystem, network, and process space
- **Ephemeral by default** — data is lost when the container is removed (use volumes for persistence)
- Multiple containers can run from the same image without interfering with each other

```
Dockerfile  →  (docker build)  →  Image  →  (docker run)  →  Container
(instructions)    (blueprint/template)          (live, running instance)
```

### 5.5 Docker Hub
- The **default public registry** for Docker images
- Hosts millions of **Official Images** (nginx, postgres, python, ubuntu, redis, mysql, etc.)
- You can push your own images (public or private repositories)
- Alternatives: AWS ECR, Azure ACR, GCP Artifact Registry, GitHub Container Registry (GHCR), Harbor (self-hosted)

### 5.6 Docker Registry
- A **storage and distribution system** for Docker images
- Docker Hub is the default public registry
- Organizations run **private registries** for proprietary images
- Image naming format: `registry/username/image:tag`
  - e.g., `docker.io/library/nginx:latest` (official)
  - e.g., `gcr.io/mycompany/myapp:v1.2.3` (private GCR)

---

## 6. Dockerfile — Complete Reference

A Dockerfile is a set of instructions Docker reads to assemble an image. Each instruction creates a new **layer** in the image.

### All Dockerfile Instructions Explained

```dockerfile
# ─── FROM ──────────────────────────────────────────────────────
# Base image to start from. Every Dockerfile must begin with FROM.
# Always use specific version tags — never just "latest" in production.
FROM python:3.11-slim

# ─── LABEL ─────────────────────────────────────────────────────
# Add metadata to the image (author, version, description, etc.)
LABEL maintainer="yourname@example.com"
LABEL version="1.0.0"
LABEL description="My Django Application"

# ─── ARG ───────────────────────────────────────────────────────
# Build-time variable — only available during docker build, NOT at runtime.
# Pass with: docker build --build-arg APP_ENV=prod .
ARG APP_ENV=production
ARG BUILD_DATE

# ─── ENV ───────────────────────────────────────────────────────
# Environment variable — available at BOTH build time AND runtime.
# Accessible inside container as a regular environment variable.
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV APP_HOME=/app

# ─── WORKDIR ───────────────────────────────────────────────────
# Set the working directory inside the container.
# All subsequent RUN, COPY, CMD, ENTRYPOINT commands use this dir.
# Creates the directory automatically if it does not exist.
WORKDIR /app

# ─── COPY ──────────────────────────────────────────────────────
# Copy files/directories from the build context (host) into the image.
# Preferred over ADD for simple file copying.
COPY requirements.txt .         # copy requirements file first
COPY . .                        # then copy all source code

# ─── ADD ───────────────────────────────────────────────────────
# Like COPY, but also supports:
#   - Downloading from URLs
#   - Auto-extracting .tar.gz archives
# Use COPY unless you specifically need ADD's extra capabilities.
ADD https://example.com/setup.tar.gz /tmp/

# ─── RUN ───────────────────────────────────────────────────────
# Execute a shell command during the BUILD process.
# Each RUN creates a new image layer.
# Best practice: chain commands with && to minimize layers.
RUN apt-get update && apt-get install -y \
    gcc \
    libpq-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir -r requirements.txt

# ─── USER ──────────────────────────────────────────────────────
# Switch to a non-root user for security.
# All subsequent RUN, CMD, ENTRYPOINT instructions run as this user.
# ALWAYS do this before CMD in production images.
RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser
USER appuser

# ─── EXPOSE ────────────────────────────────────────────────────
# Documents which port the container application listens on.
# This does NOT actually publish the port to the host.
# You still need -p flag in docker run to make it accessible.
EXPOSE 8000

# ─── VOLUME ────────────────────────────────────────────────────
# Declares a mount point inside the container.
# Data written to this path persists even if the container is removed.
VOLUME ["/app/data", "/app/logs"]

# ─── CMD ───────────────────────────────────────────────────────
# The DEFAULT command to run when a container starts.
# Can be completely overridden by providing a command in docker run.
# Only one CMD per Dockerfile — the last one wins.
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]

# ─── ENTRYPOINT ────────────────────────────────────────────────
# The FIXED command that always runs when the container starts.
# Cannot be overridden without using --entrypoint flag.
# CMD becomes the default arguments passed to ENTRYPOINT.
ENTRYPOINT ["gunicorn"]
CMD ["myproject.wsgi:application", "--bind", "0.0.0.0:8000"]

# ─── HEALTHCHECK ───────────────────────────────────────────────
# Define how Docker checks if the container is healthy.
# Used by orchestrators (Kubernetes, Docker Swarm) to detect failures.
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8000/health/ || exit 1

# ─── ONBUILD ───────────────────────────────────────────────────
# Instructions to execute when this image is used as a base by another Dockerfile.
# Used for shared base images in a team environment.
ONBUILD COPY . /app
ONBUILD RUN pip install -r requirements.txt

# ─── STOPSIGNAL ────────────────────────────────────────────────
# Set the system signal to stop the container (default: SIGTERM)
STOPSIGNAL SIGTERM

# ─── SHELL ─────────────────────────────────────────────────────
# Change the default shell used for RUN commands.
SHELL ["/bin/bash", "-c"]
```

### CMD vs ENTRYPOINT — Key Difference

| | CMD | ENTRYPOINT |
|--|-----|-----------|
| Purpose | Default command (can be overridden) | Fixed main executable |
| Override | Yes — `docker run image mycommand` replaces CMD | No — need `--entrypoint` flag |
| Best for | General containers | Containers used as specific CLI tools |
| Together | CMD provides default args to ENTRYPOINT | ENTRYPOINT is the executable |

### Real-World Django Dockerfile
```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Set environment to avoid .pyc files and buffer output
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1

# Install system dependencies first (changes rarely = cached)
RUN apt-get update && apt-get install -y \
    libpq-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements before source code (maximize cache hits)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application source code
COPY . .

# Create non-root user for security
RUN addgroup --system django && adduser --system --ingroup django django
USER django

EXPOSE 8000

HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD curl -f http://localhost:8000/ || exit 1

CMD ["gunicorn", "myproject.wsgi:application", "--bind", "0.0.0.0:8000"]
```

### .dockerignore (Always create this!)
```
# .dockerignore — exclude files from the build context
__pycache__/
*.pyc
*.pyo
.env
.env.*
.git
.gitignore
*.md
README*
node_modules/
.DS_Store
venv/
.venv/
*.log
tests/
docs/
coverage/
.pytest_cache/
```
> Without `.dockerignore`, Docker sends ALL files (including `.git`, `node_modules`, `.env`) to the daemon — massively slowing every build and potentially leaking secrets into images.

---

## 7. Docker Images — In Depth

### Image Layers & Caching
Every Dockerfile instruction creates a **layer**. Layers are stacked and cached. If a layer's input has not changed, Docker reuses the cached result — dramatically speeding up rebuilds.

```
Layer 5:  COPY . .                 ← changes most often → not cached often
Layer 4:  RUN pip install          ← changes when requirements.txt changes
Layer 3:  COPY requirements.txt .  ← changes rarely → almost always cached
Layer 2:  RUN apt-get install      ← changes rarely → almost always cached
Layer 1:  FROM python:3.11-slim    ← never changes → always cached
```

**Golden Rule:** Instructions that change most often go at the BOTTOM. This maximizes cache hits and minimizes rebuild time.

### Image Tags
```
image_name:tag

nginx:latest          # tag = latest (avoid in production — unpredictable)
nginx:1.25.3          # specific version = reproducible, recommended
python:3.11-slim      # slim = no build tools, smaller than full image
python:3.11-alpine    # alpine = smallest (musl libc, great for tiny images)
```

### Image Naming Format
```
[registry/][username/]image_name[:tag]

Examples:
nginx                                                    # official, Docker Hub
python:3.11-slim                                         # official, specific tag
myusername/myapp:v1.0                                    # user image, Docker Hub
gcr.io/myproject/myapp:v1.2.3                           # Google Container Registry
123.dkr.ecr.us-east-1.amazonaws.com/myapp:latest        # AWS ECR
ghcr.io/myorg/myapp:main                                 # GitHub Container Registry
```

---

## 8. Docker Containers — In Depth

### Container Lifecycle
```
                docker create
                     ↓
             [Created] — not running yet
                     ↓
                docker start  (or docker run = create + start)
                     ↓
             [Running] — processes executing
                ↙         ↘
          docker pause    docker stop (graceful SIGTERM → wait → SIGKILL)
               ↓               ↓
           [Paused]        [Stopped/Exited]
               ↓               ↓
         docker unpause    docker start  (restart)
                               ↓
                          docker rm (delete the container)
```

### Resource Limits (Always Set in Production!)
```bash
docker run \
  --memory="512m" \          # max 512 MB RAM
  --memory-swap="1g" \       # max 1 GB RAM + swap combined
  --cpus="1.5" \             # max 1.5 CPU cores
  --pids-limit=100 \         # max 100 processes in container
  myapp
```

### Restart Policies
```bash
--restart=no                # never restart (default)
--restart=always            # always restart on any exit
--restart=on-failure        # restart only if exit code != 0
--restart=on-failure:3      # restart up to 3 times on failure
--restart=unless-stopped    # restart always except when manually stopped
```

---

## 9. Docker Networking

Docker has 5 built-in network drivers. Each container gets its own isolated network namespace.

### Network Types

#### Bridge Network (Default) 🌉
```
Container A ─┐
Container B ─┼── docker0 bridge ── Host Network ── Internet
Container C ─┘
```
- Default for standalone containers
- Containers on the same bridge communicate via IP
- **Named bridge networks add DNS**: containers can reach each other by container name
- Isolated from host network (uses NAT for internet access)

```bash
# Create a named bridge network
docker network create mynetwork

# Run containers on it — they can now talk by name
docker run -d --network mynetwork --name web nginx
docker run -d --network mynetwork --name db postgres
# Inside "web" container: ping db → works by DNS!
```

#### Host Network 🖥️
```
Container → shares host's network stack directly → no port mapping needed
```
- No network isolation — container's ports ARE the host's ports
- Best performance (no NAT overhead)
- Use case: High-performance monitoring agents, apps needing raw network access

```bash
docker run --network host nginx
# nginx listens on host port 80 directly — no -p needed
```

#### None Network 🚫
- Container has absolutely no network access
- Completely isolated
- Use case: Batch jobs, security-sensitive computation

```bash
docker run --network none myapp
```

#### Overlay Network 🔗
- Connects containers **across multiple Docker hosts** (different machines in a cluster)
- Required for Docker Swarm multi-host deployments
- Uses VXLAN tunneling underneath

#### Macvlan Network 🔌
- Assigns a real MAC address to the container
- Container appears as a physical network device
- Use case: Legacy apps that require direct L2 network access

### Network Driver Summary

| Network | Isolation | Multi-Host | Use Case |
|---------|-----------|------------|----------|
| bridge | ✅ | ❌ | Default single-host app networking |
| host | ❌ | ❌ | Maximum network performance |
| none | Full | ❌ | No network needed |
| overlay | ✅ | ✅ | Docker Swarm, distributed clusters |
| macvlan | ✅ | ✅ | Legacy apps needing MAC address |

### Port Mapping
```bash
-p 8080:80           # host port 8080 → container port 80
-p 5432:5432         # host 5432 → container 5432 (PostgreSQL)
-p 0.0.0.0:80:80     # all host interfaces → container 80
-p 127.0.0.1:80:80   # localhost ONLY → container 80 (more secure)
-P                   # auto-assign random host port to all EXPOSE ports
```

---

## 10. Docker Volumes & Storage

Containers are **ephemeral** — data is lost when a container is removed. Volumes solve this.

### 3 Types of Storage

#### 1. Named Volumes ✅ (Recommended for Production)
```bash
docker volume create mydata
docker run -v mydata:/var/lib/postgresql/data postgres
```
- Managed by Docker (stored in `/var/lib/docker/volumes/`)
- Survives container removal and restart
- Easy to back up and share between containers
- Best for: databases, persistent app data, logs

#### 2. Bind Mounts (Best for Development)
```bash
# Mount current directory into container
docker run -v $(pwd):/app myapp
# or newer syntax
docker run --mount type=bind,source=$(pwd),target=/app myapp
```
- Maps a specific host directory into the container
- Changes on host are immediately reflected inside the container
- Perfect for **live code reload during development**
- Less portable (depends on host path)

#### 3. tmpfs Mounts (Temporary, In-Memory)
```bash
docker run --tmpfs /app/tmp myapp
```
- Stored in host RAM only — never written to disk
- Deleted when container stops
- Use case: Sensitive temporary data (session tokens, API keys during processing)

### Volume vs Bind Mount

| Feature | Named Volume | Bind Mount |
|---------|-------------|------------|
| Managed by | Docker | Host OS |
| Location | Docker manages path | You specify path |
| Portability | High | Low (path-dependent) |
| Use in production | ✅ Yes | ⚠️ Development only |
| Backup | Easy | Manual |

---

## 11. Docker Compose

Docker Compose manages **multi-container applications** with a single `docker-compose.yml` file.

### Real-World Example: Django + PostgreSQL + Redis + Celery
```yaml
version: '3.9'

services:

  # Django Web App
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    ports:
      - "8000:8000"
    volumes:
      - .:/app                        # bind mount for live dev reload
    environment:
      - DEBUG=1
      - DATABASE_URL=postgres://postgres:secret@db:5432/mydb
      - REDIS_URL=redis://redis:6379/0
    depends_on:
      db:
        condition: service_healthy    # wait until db health check passes
      redis:
        condition: service_started
    networks:
      - app_network
    restart: unless-stopped

  # PostgreSQL Database
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: secret
    volumes:
      - postgres_data:/var/lib/postgresql/data   # named volume = persistent
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - app_network

  # Redis (Cache + Celery Broker)
  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    networks:
      - app_network

  # Celery Worker
  celery:
    build: .
    command: celery -A myproject worker -l info
    volumes:
      - .:/app
    environment:
      - DATABASE_URL=postgres://postgres:secret@db:5432/mydb
      - REDIS_URL=redis://redis:6379/0
    depends_on:
      - db
      - redis
    networks:
      - app_network

volumes:
  postgres_data:    # Docker-managed named volume

networks:
  app_network:
    driver: bridge
```

### Docker Compose Commands
```bash
docker compose up                  # start all services (foreground)
docker compose up -d               # start in detached/background mode
docker compose up --build          # rebuild images before starting
docker compose down                # stop and remove containers
docker compose down -v             # also remove named volumes (DELETES DATA!)
docker compose ps                  # list service status
docker compose logs                # view logs from all services
docker compose logs web            # logs from specific service
docker compose logs -f web         # follow / tail logs in real time
docker compose exec web bash       # open shell inside running service
docker compose exec web python manage.py migrate   # run a one-off command
docker compose build               # rebuild all images
docker compose pull                # pull all images from registry
docker compose restart web         # restart a specific service
docker compose stop                # stop containers without removing them
docker compose start               # start previously stopped containers
docker compose config              # validate and print the merged compose file
docker compose run web pytest      # run a one-off container with a command
```

---

## 12. Multi-Stage Builds

Multi-stage builds use **multiple FROM instructions** in one Dockerfile. You compile/build in a large image, then copy only the final artifact into a small runtime image — leaving all build tools behind.

### The Problem Without Multi-Stage
```
Full build image:   ~1.2 GB  (includes compiler, build tools, test libs, source)
What you need at runtime: ~50 MB  (compiled binary or app + runtime only)
```

### Python/Django Multi-Stage Example
```dockerfile
# Stage 1: Builder — install everything needed to build
FROM python:3.11-slim AS builder

WORKDIR /app

RUN apt-get update && apt-get install -y gcc libpq-dev \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
# Install into a separate prefix so we can copy them cleanly
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt


# Stage 2: Runtime — tiny final image
FROM python:3.11-slim AS runtime

WORKDIR /app

# Copy ONLY installed packages from builder — no compiler included!
COPY --from=builder /install /usr/local

# Copy application source
COPY . .

# Security: non-root user
RUN addgroup --system app && adduser --system --ingroup app appuser
USER appuser

EXPOSE 8000
CMD ["gunicorn", "myproject.wsgi:application", "--bind", "0.0.0.0:8000"]
```

### Node.js / React Multi-Stage Example
```dockerfile
# Stage 1: Build the React app
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci                          # clean, reproducible install
COPY . .
RUN npm run build                   # output → /app/build

# Stage 2: Serve with tiny Nginx image
FROM nginx:alpine AS runtime
COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
```
Builder stage:  ~1.2 GB (Node + all node_modules + source)
Final image:    ~25 MB  (nginx + built static files only)
```

### Layer Cache Optimization
```dockerfile
# BAD: Cache busted on every source code change
FROM python:3.11-slim
COPY . .                               # all files copied first
RUN pip install -r requirements.txt    # reinstalls EVERYTHING every time

# GOOD: Dependencies cached separately from source
FROM python:3.11-slim
COPY requirements.txt .                # copy requirements FIRST (rarely changes)
RUN pip install -r requirements.txt    # cached until requirements.txt changes
COPY . .                               # source code last (changes often)
```

---

## 13. Docker Hub & Registries

### Push Your Image to Docker Hub
```bash
# Build with your Docker Hub username
docker build -t yourusername/myapp:v1.0 .

# Login to Docker Hub
docker login

# Push to Docker Hub
docker push yourusername/myapp:v1.0

# Pull it anywhere in the world
docker pull yourusername/myapp:v1.0
```

### Tagging Strategy for Production
```bash
# Tag with semantic version AND latest
docker tag myapp:v1.2.3 myusername/myapp:v1.2.3
docker tag myapp:v1.2.3 myusername/myapp:latest

# Tag with git commit SHA (used in CI — very traceable)
docker build -t myapp:$(git rev-parse --short HEAD) .
```

### Private Registry Logins
```bash
# AWS ECR
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin 123.dkr.ecr.us-east-1.amazonaws.com

# GitHub Container Registry
echo $GITHUB_TOKEN | docker login ghcr.io -u YOUR_USERNAME --password-stdin
```

---

## 14. Docker Security

### 1. Never Run as Root
```dockerfile
# BAD — root by default
FROM python:3.11
CMD ["python", "app.py"]

# GOOD — switch to non-root before CMD
FROM python:3.11
RUN addgroup --system app && adduser --system --ingroup app appuser
USER appuser
CMD ["python", "app.py"]
```

### 2. Use Specific Version Tags in Production
```dockerfile
FROM python:latest       # BAD  — unpredictable, changes without warning
FROM python:3.11.9-slim  # GOOD — pinned, reproducible
```

### 3. Scan Images for Vulnerabilities
```bash
docker scout cves myimage:latest    # Docker Scout (built-in, 2024+)
trivy image myimage:latest          # Trivy (open-source, very popular)
```

### 4. Drop Unnecessary Linux Capabilities
```bash
# Drop ALL capabilities, add back only what is needed
docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE nginx
```

### 5. Use Read-Only Filesystem
```bash
docker run --read-only myapp
# Allow writes only to specific directories
docker run --read-only --tmpfs /tmp myapp
```

### 6. Never Bake Secrets into Images
```bash
# BAD — secret stored in image layer, visible in docker history
ENV AWS_SECRET_KEY=mysecretkey123

# GOOD — pass at runtime as environment variable
docker run -e AWS_SECRET_KEY=$AWS_SECRET_KEY myapp

# BEST — use Docker Secrets (Swarm) or external secrets manager (Vault, AWS Secrets Manager)
```

### 7. Set Resource Limits (Prevent DoS)
```bash
docker run --memory="256m" --cpus="0.5" --pids-limit=100 myapp
```

---

## 15. Docker in CI/CD

Docker ensures your application runs identically in every stage of the pipeline.

### CI/CD Flow
```
Developer pushes code to GitHub
          ↓
GitHub Actions triggered
          ↓
docker build → docker test → docker push (to registry)
          ↓
Production: docker pull → docker run
(or Kubernetes: kubectl rollout → new pods with new image)
```

### GitHub Actions Example
```yaml
# .github/workflows/docker.yml
name: Build, Test, and Push

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and Push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            myusername/myapp:latest
            myusername/myapp:${{ github.sha }}
```

---

## 16. All Docker Commands — Explained

### IMAGE COMMANDS

```bash
# Build an image from Dockerfile in the current directory
# -t = tag (name:version)
docker build -t myapp:v1.0 .

# Build using a specific Dockerfile
docker build -f Dockerfile.prod -t myapp:prod .

# Build with build-time arguments
docker build --build-arg APP_ENV=prod -t myapp .

# Pull an image from a registry to local machine
docker pull nginx:1.25

# Push a local image to a registry
docker push myusername/myapp:v1.0

# List all locally stored images
docker images
docker image ls

# Show full details of an image (JSON: layers, config, entrypoint, etc.)
docker image inspect nginx

# Show history of image layers (size of each layer)
docker image history nginx

# Tag an existing image with a new name/version
docker tag myapp:latest myapp:v1.0
docker tag myapp:v1.0 myusername/myapp:v1.0

# Remove a specific image
docker rmi myapp:v1.0
docker image rm myapp:v1.0

# Remove all dangling images (untagged intermediate layers)
docker image prune

# Remove all unused images (not used by any container)
docker image prune -a

# Save an image to a .tar file (for offline transfer)
docker save -o myapp.tar myapp:v1.0

# Load an image from a .tar file
docker load -i myapp.tar

# Search for images on Docker Hub
docker search nginx
```

---

### CONTAINER COMMANDS

```bash
# ─── Create & Start ─────────────────────────────────────────────
# Run a container (create + start in one command)
docker run nginx

# -d = detached/background mode (don't block terminal)
docker run -d nginx

# --name = give the container a custom name
docker run -d --name mywebserver nginx

# -p = publish port (host_port:container_port)
docker run -d -p 8080:80 --name mywebserver nginx

# -e = set environment variable
docker run -d -e POSTGRES_PASSWORD=secret --name mydb postgres

# -v = mount volume (named_volume:container_path or host_path:container_path)
docker run -d -v mydata:/var/lib/postgresql/data --name mydb postgres
docker run -d -v $(pwd):/app --name myapp myimage    # bind mount

# --network = connect to a specific network
docker run -d --network mynetwork --name web nginx

# --restart = restart policy
docker run -d --restart=unless-stopped nginx

# --memory + --cpus = resource limits
docker run -d --memory="512m" --cpus="1.0" myapp

# -it = interactive terminal (stays open, useful for debugging)
docker run -it ubuntu bash

# --rm = auto-remove container when it exits (great for one-off tasks)
docker run --rm -it python:3.11 python
docker run --rm myapp pytest        # run tests and clean up

# ─── List & Inspect ─────────────────────────────────────────────
# List currently running containers
docker ps

# List ALL containers including stopped ones
docker ps -a

# Show containers with their disk size
docker ps -a -s

# Show specific fields in table format
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"

# Full details of a container (JSON: IP, mounts, env vars, network, config)
docker inspect mywebserver

# Get the container's IP address only
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mywebserver

# View port mappings of a container
docker port mywebserver

# View what changed in the container's filesystem since it started
docker diff mywebserver

# ─── Control ────────────────────────────────────────────────────
# Start a previously stopped container
docker start mywebserver

# Stop a running container gracefully (SIGTERM → waits → SIGKILL)
docker stop mywebserver

# Force-stop a container immediately (SIGKILL — no wait)
docker kill mywebserver

# Restart a container (stop + start)
docker restart mywebserver

# Pause all processes in a container (freeze, not stop)
docker pause mywebserver

# Resume a paused container
docker unpause mywebserver

# Rename a container
docker rename old_name new_name

# Remove a stopped container
docker rm mywebserver

# Force-remove a running container (stops + removes in one step)
docker rm -f mywebserver

# Remove all stopped containers at once
docker container prune

# ─── Logs & Monitoring ──────────────────────────────────────────
# View container logs (stdout + stderr)
docker logs mywebserver

# Follow / tail logs in real time (like tail -f)
docker logs -f mywebserver

# Show only last N lines
docker logs --tail 100 mywebserver

# Show logs with timestamps
docker logs -t mywebserver

# Show logs since a specific time
docker logs --since 2026-01-01T00:00:00 mywebserver

# View real-time CPU, RAM, network, disk I/O for all containers
docker stats

# Stats for a specific container
docker stats mywebserver

# View running processes inside a container
docker top mywebserver

# ─── Execute & Interact ─────────────────────────────────────────
# Run a command inside a running container
docker exec mywebserver ls /app
docker exec mywebserver python manage.py check

# Open an interactive bash shell inside a running container
docker exec -it mywebserver bash
docker exec -it mywebserver sh            # use sh for Alpine-based images

# Run as a specific user
docker exec -it --user root mywebserver bash

# Copy files between host and container
docker cp mywebserver:/app/output.txt ./output.txt    # container → host
docker cp ./config.py mywebserver:/app/config.py       # host → container

# ─── Advanced ───────────────────────────────────────────────────
# Commit a container's current state as a new image
docker commit mywebserver myimage:modified

# Export a container's filesystem as a tar archive
docker export mywebserver -o mycontainer.tar

# Wait for a container to stop, then print exit code
docker wait mywebserver

# Attach to a running container's stdin/stdout
docker attach mywebserver
```

---

### NETWORK COMMANDS

```bash
# List all networks
docker network ls

# Create a custom bridge network
docker network create mynetwork

# Create with a specific subnet and gateway
docker network create --subnet=192.168.1.0/24 --gateway=192.168.1.1 mynetwork

# Inspect a network (see connected containers, IP assignments)
docker network inspect mynetwork

# Connect a running container to a network
docker network connect mynetwork mycontainer

# Disconnect a container from a network
docker network disconnect mynetwork mycontainer

# Remove a network
docker network rm mynetwork

# Remove all unused networks
docker network prune
```

---

### VOLUME COMMANDS

```bash
# Create a named volume
docker volume create mydata

# List all volumes
docker volume ls

# Inspect a volume (see actual host mount path and metadata)
docker volume inspect mydata

# Remove a specific volume
docker volume rm mydata

# Remove all unused volumes (volumes not mounted by any container)
docker volume prune

# Use a named volume when starting a container
docker run -d -v mydata:/var/lib/postgresql/data postgres

# Backup a volume (copy contents to a tar file)
docker run --rm -v mydata:/data -v $(pwd):/backup ubuntu \
  tar czf /backup/mydata_backup.tar.gz -C /data .

# Restore a volume from backup
docker run --rm -v mydata:/data -v $(pwd):/backup ubuntu \
  tar xzf /backup/mydata_backup.tar.gz -C /data
```

---

### SYSTEM COMMANDS

```bash
# Show Docker disk usage (images, containers, volumes, build cache)
docker system df

# Detailed disk usage breakdown
docker system df -v

# Clean up: stopped containers + unused networks + dangling images + build cache
docker system prune

# Full cleanup: everything unused including unnamed images and volumes
docker system prune -a --volumes

# View Docker version (client and server)
docker version

# View Docker system info (resources, runtime, plugin, storage driver)
docker info

# View real-time events from the Docker daemon
docker events

# Login to Docker Hub (or other registry)
docker login
docker login ghcr.io           # login to a specific registry

# Logout
docker logout
```

---

## 17. Best Practices

### ✅ DOs

**Dockerfile:**
- Use **specific version tags** (e.g., `python:3.11.9-slim`, not `python:latest`)
- Use **slim or alpine variants** to minimize image size
- **Copy dependency files first**, then install, then copy source code — maximize cache hits
- Always use **.dockerignore** to exclude `.git`, `.env`, `node_modules`, logs, and test data
- Use **multi-stage builds** to eliminate build tools from runtime images
- Always **switch to a non-root user** with `USER` before `CMD`
- Add a **HEALTHCHECK** so orchestrators know when the app is actually ready

**Security:**
- Never put secrets in `ENV` instructions — they appear in `docker history` forever
- Use `--cap-drop=ALL` and add back only the capabilities you need
- Scan images with `docker scout` or Trivy before pushing to production
- Use `--read-only` filesystems where possible

**Runtime:**
- Always set **memory and CPU limits** in production
- Use `--restart=unless-stopped` for long-running services
- Use **named volumes**, not bind mounts, for production persistent data
- Use **named bridge networks** — containers can discover each other by name via DNS

**Performance:**
- Order Dockerfile instructions from least-changed to most-changed (top to bottom)
- Combine multiple `RUN` instructions with `&&` to reduce layer count
- Rebuild base images regularly to get security patches

### ❌ DON'Ts

- Never use `FROM image:latest` in production Dockerfiles
- Never store secrets in `ENV`, `ARG`, or bake them into image layers
- Never run `docker run` without memory limits in production — a bad container can OOM the host
- Never skip `.dockerignore` — build context size directly affects build speed
- Never leave containers running as root
- Never ignore `docker system prune` — disk can fill up silently with stale images and containers

---

## 18. Docker vs Alternatives

| Feature | Docker | Podman | containerd | LXC/LXD |
|---------|--------|--------|-----------|---------|
| Architecture | Daemon-based | Daemonless | Daemon-based | Daemon-based |
| Root Required | Optional (Rootless) | Never (Rootless native) | Yes | Yes |
| Docker CLI Compatible | Native | ✅ Drop-in replacement | ❌ | ❌ |
| Compose Support | ✅ Docker Compose | ✅ Podman Compose | ❌ | ❌ |
| Kubernetes CRI Runtime | ❌ | ✅ | ✅ (default K8s runtime) | ❌ |
| Security | Good | Better (rootless by default) | Good | Good |
| Maturity | Very mature | Fast-growing | Very mature | Mature |
| Best For | Dev, CI/CD, Swarm | RHEL, security-focused | Kubernetes nodes | System-level containers |

> **Note:** Kubernetes deprecated Docker as a container runtime in 2022, switching to `containerd`. However, Docker remains the dominant tool for **building images** and **local development workflows** in 2026.

---

## 19. Common Interview Questions

### Q1: What is Docker and how is it different from a Virtual Machine?
**Answer:** Docker is a containerization platform that packages applications and their dependencies into containers. Unlike VMs, containers share the host OS kernel instead of running a full guest OS per application. This makes containers far lighter (MBs vs GBs), faster to start (seconds vs minutes), and more resource-efficient — while still providing process-level isolation.

### Q2: What is the difference between a Docker Image and a Docker Container?
**Answer:** A Docker Image is a read-only, layered blueprint (like a class in OOP) containing the application, runtime, and dependencies. A Docker Container is a running instance of that image (like an object created from a class) — it has its own writable filesystem layer, network, and process space. One image can spawn many containers simultaneously without conflicts.

### Q3: What is a Dockerfile and what are its most important instructions?
**Answer:** A Dockerfile is a text file with sequential build instructions. Key instructions: `FROM` (base image), `RUN` (execute commands at build time), `COPY` (add files from host), `ENV` (runtime environment variables), `WORKDIR` (working directory), `EXPOSE` (document listening port), `CMD` (default runtime command), `ENTRYPOINT` (fixed executable), `USER` (switch to non-root), `HEALTHCHECK` (health probe for orchestrators).

### Q4: What is the difference between CMD and ENTRYPOINT?
**Answer:** `CMD` defines the default command that runs when a container starts — it can be completely overridden by passing a command to `docker run`. `ENTRYPOINT` defines the fixed main executable that always runs — it cannot be overridden without `--entrypoint`. When both are used together, `ENTRYPOINT` is the executable and `CMD` provides default arguments to it.

### Q5: What is a Docker Volume and why is it needed?
**Answer:** By default, all data inside a container is lost when the container is removed (containers are ephemeral). Docker Volumes are persistent storage areas managed by Docker that exist independently of the container lifecycle. Named volumes are the recommended way to persist important data (database files, uploads) — data survives container removal, image updates, and restarts.

### Q6: What is Docker Compose and when would you use it?
**Answer:** Docker Compose is a tool for defining and running multi-container applications using a single `docker-compose.yml` file. Instead of running multiple complex `docker run` commands, you declare all services (web, db, cache, worker), their networks, volumes, and dependencies in one file, and manage everything with commands like `docker compose up` and `docker compose down`. Essential for local development of any app with more than one service.

### Q7: What is a multi-stage Docker build and why does it matter?
**Answer:** A multi-stage build uses multiple `FROM` instructions in one Dockerfile. A large "builder" stage compiles the code, then only the compiled artifact is copied into a tiny "runtime" stage — leaving behind all compilers, build tools, and intermediate files. This reduces final image size dramatically (sometimes from 1 GB to under 50 MB), which means faster deploys, lower storage costs, and a smaller attack surface.

### Q8: Explain Docker networking. What is the difference between bridge and host?
**Answer:** Docker networking controls how containers communicate with each other and the outside world. The bridge network (default) creates an isolated virtual network on the host — containers communicate by IP, and named bridges add DNS service discovery. The host network removes all network isolation — the container's ports are directly the host's ports, giving best performance but no isolation.

### Q9: How do you pass secrets to a Docker container securely?
**Answer:** Never bake secrets into images (they appear in `docker history`). Options in order of preference: (1) Pass at runtime via environment variables (`-e KEY=val`) — simple but visible in `docker inspect`. (2) Use Docker Secrets in Swarm mode — secrets are mounted as files inside the container. (3) Use an external secrets manager (AWS Secrets Manager, HashiCorp Vault) and fetch at startup. Never use `ENV` in a Dockerfile for secrets.

### Q10: What is the purpose of `.dockerignore` and why is it important?
**Answer:** `.dockerignore` tells Docker which files and directories to exclude from the build context sent to the daemon during `docker build`. Without it, everything (`.git`, `node_modules`, `.env` files, logs, test data) is sent to the daemon — massively slowing builds and potentially leaking sensitive information into the image. A well-configured `.dockerignore` can reduce build context from several GB to just MB.

---

## 🔄 Docker — Complete Workflow at a Glance

```
Write Application Code
        ↓
Create Dockerfile + .dockerignore
        ↓
docker build -t myapp:v1.0 .         → Image created with cached layers
        ↓
docker run -d -p 8000:8000 myapp     → Test locally
        ↓
docker push myusername/myapp:v1.0    → Push to registry (Docker Hub / ECR / GHCR)
        ↓
CI/CD Pipeline: build → test → push → deploy
        ↓
Production: docker pull + docker run (or Kubernetes rollout)
        ↓
Application runs identically everywhere ✅
```

---

## 📝 Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| Docker | OS-level containerization; packages app + deps into portable containers |
| Container | Lightweight, isolated running instance of an image |
| Image | Read-only layered blueprint used to create containers |
| Dockerfile | Text file with build instructions; each instruction = one layer |
| Docker Engine | Core: Daemon + CLI Client + REST API |
| Docker Hub | Default public image registry; push and pull images |
| Docker Compose | Multi-container app via single YAML file; one command to manage all |
| Named Volume | Docker-managed persistent storage; survives container removal |
| Bind Mount | Host directory mounted into container; used for live dev reload |
| Bridge Network | Default isolated virtual network; named bridges add DNS by container name |
| Host Network | Container shares host's network stack; no isolation, best performance |
| Multi-Stage Build | Multiple FROM stages; only copy artifact to final slim runtime image |
| Layer | Each Dockerfile instruction = one cached, reusable image layer |
| .dockerignore | Excludes files from build context (like .gitignore but for Docker) |
| `docker run` | Create + start a new container from an image |
| `docker exec` | Run a command inside an already-running container |
| `docker build` | Build an image from a Dockerfile |
| `docker ps` | List running containers (`-a` for all including stopped) |
| `docker logs` | View stdout/stderr output from a container |
| `docker inspect` | Full JSON details of any Docker object (container/image/volume/network) |
| `docker stats` | Real-time CPU, RAM, network usage of containers |
| `docker prune` | Clean up stopped containers, unused images, volumes, networks |
| ENTRYPOINT | Fixed executable; always runs; cannot be easily overridden |
| CMD | Default command or args; can be overridden in `docker run` |
| HEALTHCHECK | How Docker tests if a container is alive and serving correctly |
| `--restart` | Policy for auto-restarting containers on crash or reboot |
| ARG | Build-time variable; only available during `docker build` |
| ENV | Runtime environment variable; available inside running container |

---

> 💡 **Production Golden Rule for Docker:**
> Always combine — **Specific Tags + Multi-Stage Builds + Non-Root User + .dockerignore + Resource Limits + Named Volumes + HEALTHCHECK + Secrets at Runtime**.
> These 8 principles make your containers secure, lean, fast, and truly production-ready.

---
*Notes prepared with research from: Docker Official Docs, DevToolbox (Feb 2026), NuCamp (Jan 2026), DevOps Training Institute (Dec 2025), AppSignal, GeeksforGeeks (2025), CloudDevOpsHub (2025)*
