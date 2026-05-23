# Docker Lab Assignment

This repository contains solutions for Docker Lab tasks.


# Difference Between CMD & ENTRYPOINT

## CMD

CMD is used to provide default commands for a container.

- It can be overridden when running the container.
- Used for optional/default execution commands.

Example:

```dockerfile
CMD ["python","app.py"]
```

Run:

```bash
docker run myapp
```

Docker executes:

```bash
python app.py
```

Override example:

```bash
docker run myapp python test.py
```

---

## ENTRYPOINT

ENTRYPOINT defines the main executable command for the container.

- It is harder to override.
- Used when the container should always run a specific executable.

Example:

```dockerfile
ENTRYPOINT ["python"]
```

Run:

```bash
docker run myapp app.py
```

Docker executes:

```bash
python app.py
```

---

## Main Difference

| CMD | ENTRYPOINT |
|---|---|
| Default command | Main executable |
| Easily overridden | Less flexible |
| Optional behavior | Fixed behavior |

---

# Difference Between COPY & ADD

## COPY

COPY is used to copy files and folders from the host machine into the container.

Example:

```dockerfile
COPY . /app
```

This copies all files into `/app`.

---

## ADD

ADD works like COPY but has extra features:

- Can extract compressed files automatically.
- Can download files from URLs.

Example:

```dockerfile
ADD test.tar.gz /app
```

This extracts the archive automatically.

---

## Main Difference

| COPY | ADD |
|---|---|
| Simple file copy | Copy + extra features |
| Preferred in most cases | Used for advanced operations |
| More predictable | More complex |

---

## Best Practice

Use `COPY` unless you specifically need features provided by `ADD`.

---

# Problem 1

## Run hello-world container

```bash
docker run hello-world
```

## Check all containers

```bash
docker ps -a
```

## Start stopped container

```bash
docker start CONTAINER_ID
```

## Remove container

```bash
docker rm CONTAINER_ID
```

## Remove image

```bash
docker rmi hello-world
```

---

# Problem 2

## Run ubuntu interactive container

```bash
docker run -it ubuntu bash
```

## Run echo command

```bash
echo docker
```

## Create file

```bash
touch hello-docker
```

## Exit container

```bash
exit
```

## Stop container

```bash
docker stop CONTAINER_ID
```

## Remove container

```bash
docker rm CONTAINER_ID
```

## Remove all stopped containers

```bash
docker container prune -f
```

### Comment

The file `hello-docker` is removed after deleting the container because containers are ephemeral.

---

# Problem 3

## Deploy MySQL database

```bash
docker run -d --name app-database -e MYSQL_ROOT_PASSWORD=P4sSw0rd0! -p 3306:3306 mysql:latest
```

## Check running containers

```bash
docker ps
```

## Show logs

```bash
docker logs app-database
```

## Access MySQL container

```bash
docker exec -it app-database bash
```

## Login to MySQL

```bash
mysql -u root -p
```

Password:

```text
P4sSw0rd0!
```

---

# Problem 4

## Run Nginx container

```bash
docker run -d -p 8080:80 --name mynginx nginx
```

## Copy HTML file

```bash
docker cp index.html mynginx:/usr/share/nginx/html/
```

## Commit container

```bash
docker commit mynginx IMAGE_NAME
```

---

# Problem 5

## Python App

### app.py

```python
print("Hello Docker")
```

---

## Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY . .

CMD ["python","app.py"]
```

---

## Build Image

```bash
docker build -t python-app .
```

---

## Run Container

```bash
docker run python-app
```

---

## Multi-stage Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY app.py .

CMD ["python","app.py"]
```

---

## Push to Docker Hub

```bash
docker login
```

```bash
docker tag python-app YOUR_USERNAME/python-app
```

```bash
docker push YOUR_USERNAME/python-app
```

---

# Docker Concepts Learned

- Docker Images
- Containers
- Dockerfile
- Interactive Mode
- Background Containers
- Environment Variables
- Port Mapping
- Docker Hub
- MySQL Containers
- Nginx Containers
- Python Containerization

---

# Author

Mohamed Lotfy Mohamed Atya