# Docker Lab Assignment

> **Author:** Mohamed Saied | **Track:** Full Stack .NET @ ITI | **Topic:** Docker Containerization

---

## Table of Contents

1. [CMD vs ENTRYPOINT](#cmd-vs-entrypoint)
2. [COPY vs ADD](#copy-vs-add)
3. [Problem 1 — Hello World](#problem-1--hello-world)
4. [Problem 2 — Interactive Ubuntu](#problem-2--interactive-ubuntu)
5. [Problem 3 — MySQL Container](#problem-3--mysql-container)
6. [Problem 4 — Custom Nginx Server](#problem-4--custom-nginx-server)
7. [Problem 5 — Python App Containerization](#problem-5--python-app-containerization)
8. [Concepts Covered](#concepts-covered)

---

## CMD vs ENTRYPOINT

Both instructions define what runs when a container starts — but they behave differently.

### CMD

Sets a **default command** that runs if nothing is specified at runtime. It gets completely replaced if you pass arguments to `docker run`.

```dockerfile
CMD ["python", "app.py"]
```

```bash
# No args → runs: python app.py
docker run myapp

# With args → overrides CMD entirely, runs: python test.py
docker run myapp python test.py
```

---

### ENTRYPOINT

Sets the **main executable** of the container. Runtime arguments are *appended* to it, not replacing it.

```dockerfile
ENTRYPOINT ["python"]
```

```bash
# Passes app.py as an argument → runs: python app.py
docker run myapp app.py
```

---

### Key Differences

| | CMD | ENTRYPOINT |
|---|---|---|
| **Role** | Default command | Fixed executable |
| **Override behavior** | Fully replaced by CLI args | CLI args are appended |
| **Flexibility** | High | Low |
| **Best for** | Optional/default logic | Single-purpose containers |

---

## COPY vs ADD

Both copy files into the image. `ADD` just does more than you might expect.

### COPY

Straightforward file copying from host to container. No extras, no surprises.

```dockerfile
COPY . /app
```

### ADD

Works like `COPY` but also:
- Auto-extracts `.tar`, `.tar.gz`, and other compressed archives
- Can fetch files from remote URLs

```dockerfile
ADD archive.tar.gz /app   # Extracts automatically
```

### Key Differences

| | COPY | ADD |
|---|---|---|
| **File copy** | ✅ | ✅ |
| **Auto-extract archives** | ❌ | ✅ |
| **Download from URL** | ❌ | ✅ |
| **Recommended default** | ✅ | Only when needed |

> **Rule of thumb:** Use `COPY` by default. Reach for `ADD` only when you need archive extraction or URL downloading.

---

## Problem 1 — Hello World

**Goal:** Pull and run the `hello-world` image, inspect containers, then clean up.

```bash
# Run the image
docker run hello-world

# List all containers (including stopped)
docker ps -a

# Start a stopped container
docker start <CONTAINER_ID>

# Remove the container
docker rm <CONTAINER_ID>

# Remove the image
docker rmi hello-world
```

---

## Problem 2 — Interactive Ubuntu

**Goal:** Launch an Ubuntu container interactively, run commands inside it, then tear it down.

```bash
# Start Ubuntu with an interactive shell
docker run -it ubuntu bash

# Inside the container:
echo docker
touch hello-docker
exit

# Back on host — stop and remove
docker stop <CONTAINER_ID>
docker rm <CONTAINER_ID>

# Or remove all stopped containers at once
docker container prune -f
```

> **Why did `hello-docker` disappear?**
> Containers are ephemeral by design. Any data written inside a container is lost when it's removed, unless you use a **volume** to persist it.

---

## Problem 3 — MySQL Container

**Goal:** Deploy a MySQL database in detached mode, verify it's running, and connect to it.

```bash
# Run MySQL in the background
docker run -d \
  --name app-database \
  -e MYSQL_ROOT_PASSWORD=P4sSw0rd0! \
  -p 3306:3306 \
  mysql:latest

# Verify it's running
docker ps

# Check startup logs
docker logs app-database

# Open a shell inside the container
docker exec -it app-database bash

# Connect to MySQL
mysql -u root -p
# Password: P4sSw0rd0!
```

---

## Problem 4 — Custom Nginx Server

**Goal:** Serve a custom HTML page through Nginx, then save the modified container as a new image.

```bash
# Start an Nginx container, map port 8080 → 80
docker run -d -p 8080:80 --name mynginx nginx

# Copy your custom HTML into the container
docker cp index.html mynginx:/usr/share/nginx/html/

# Commit the container state as a new image
docker commit mynginx my-custom-nginx
```

---

## Problem 5 — Python App Containerization

**Goal:** Package a Python script into a Docker image, run it locally, and push it to Docker Hub.

### app.py

```python
print("Hello Docker")
```

### Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY . .

CMD ["python", "app.py"]
```

### Build & Run

```bash
docker build -t python-app .
docker run python-app
```

### Publish to Docker Hub

```bash
docker login
docker tag python-app <YOUR_USERNAME>/python-app
docker push <YOUR_USERNAME>/python-app
```

---

## Concepts Covered

| Concept | Description |
|---|---|
| **Docker Images** | Build, tag, and manage reusable snapshots |
| **Containers** | Run, stop, start, and remove isolated environments |
| **Dockerfile** | Automate image creation with build instructions |
| **Interactive Mode** | Access a shell inside a running container (`-it`) |
| **Detached Mode** | Run containers in the background (`-d`) |
| **Environment Variables** | Pass config at runtime with `-e` |
| **Port Mapping** | Expose container ports to the host with `-p` |
| **Docker Hub** | Authenticate and publish images to a public registry |
| **MySQL Containers** | Deploy stateful database services |
| **Nginx Containers** | Serve custom static content |
| **Python Containerization** | Package and ship Python apps as containers |
