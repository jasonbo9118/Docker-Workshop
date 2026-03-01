# Docker-Workshop
Data Engineering Zoomcamp

# 🐳 Docker for Data Engineering — Complete Guide

---

## 1️⃣ Introduction to Docker

**What Docker is:**

- Docker is a tool for running **containers**, which are like **mini computers inside your computer**.  
- Containers are **isolated**, meaning they have their own programs, libraries, and files.  
- Docker allows you to **run your code consistently anywhere**, from your laptop to the cloud.  

**Why Docker matters in data engineering:**

- Data engineers work with multiple tools: Python, Spark, Kafka, Airflow, Postgres, etc.  
- Installing all these tools on your machine can cause **version conflicts** or break things.  
- Docker ensures **reproducibility, isolation, and portability**.  

---

## 2️⃣ Key Docker Concepts

| Concept | Meaning | Example in Data Engineering |
|---------|---------|----------------------------|
| **Image** | Blueprint for containers | `python:3.13-slim` or `postgres:15` |
| **Container** | Running instance of an image | Python ETL container |
| **Volume** | Shared folder between host and container | `/host/data:/container/data` |
| **Dockerfile** | File that defines how to build a container | Install Python, dependencies, copy scripts |
| **Docker Compose** | Tool to run multiple containers together | Run Spark + Postgres + Airflow as a system |
| **Network** | Allows containers to communicate | Python container connects to Postgres container |

---

## 3️⃣ Basic Docker Commands

```bash
docker run -it IMAGE bash      # Start a container interactively
docker ps                      # List running containers
docker ps -a                   # List all containers (running + stopped)
docker stop CONTAINER_ID       # Stop a running container
docker rm CONTAINER_ID         # Delete a container
docker images                  # List downloaded images
docker rmi IMAGE_ID            # Delete an image
docker build -t NAME .         # Build a Docker image from Dockerfile
docker exec -it CONTAINER bash # Run commands in a running container
````

---

## 4️⃣ Dockerfile Basics

A **Dockerfile** defines the environment your code runs in.

```dockerfile
# Use Python 3.13 slim image
FROM python:3.13-slim

# Set working directory inside container
WORKDIR /app

# Copy ETL script
COPY etl/etl.py /app/etl.py

# Install Python libraries
RUN pip install pandas psycopg2-binary

# Default command to run
CMD ["python", "etl.py"]
```

---

## 5️⃣ Using Volumes to Share Files

Volumes let containers **read/write real files on your host**.

```bash
docker run -it -v $(pwd)/data:/app/data python:3.13-slim bash
```

* `$(pwd)/data` → folder on host
* `/app/data` → folder inside container
* Any file changes inside container **affect host files**, and vice versa

---

## 6️⃣ Running Containers

### Interactive (with terminal):

```bash
docker run -it -v $(pwd):/app python:3.13-slim bash
cd /app/test
python list_files.py
```

### Detached (background):

```bash
docker run -d -v $(pwd):/app python:3.13-slim sleep infinity
docker exec -it CONTAINER_ID bash
```

### Run Python scripts directly:

```bash
docker run --rm -v $(pwd):/app -w /app python:3.13-slim python test/list_files.py
```

* `--rm` → delete container automatically after finishing
* `-w /app` → set working directory inside container

---

## 7️⃣ Docker Compose Basics

Docker Compose allows you to **run multiple containers together**, perfect for data pipelines.

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: testdb
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

  python-etl:
    build: .
    depends_on:
      - postgres
    volumes:
      - ./data:/data
    environment:
      DB_HOST: postgres
      DB_NAME: testdb
      DB_USER: postgres
      DB_PASS: postgres

volumes:
  pgdata:
```

Run everything with:

```bash
docker-compose up
```

---

## 8️⃣ Data Engineering Use Cases

1. **ETL pipelines**: CSV → Transform → Postgres
2. **Big Data frameworks**: Spark, Hadoop, Kafka in containers
3. **Workflow orchestration**: Airflow + Postgres + Python scripts in separate containers
4. **Experimentation**: Test different Python or DB versions without messing up your machine

---

## 9️⃣ Advanced Tips

* **Networking** → containers talk to each other using service names
* **Environment variables** → dynamically configure containers (`DB_USER`, `DB_PASS`)
* **Persistent storage** → use volumes for databases and data files
* **Reproducibility** → `Dockerfile` + `docker-compose.yml` = anyone can run your pipeline the same way

---

## 🔟 Recommended Learning Path

1. Learn basic Linux commands (`ls`, `cd`, `pwd`, `cat`)
2. Learn Python ETL basics (Pandas, CSV, database connections)
3. Learn basic Docker commands (`docker run`, `docker ps`, `docker stop`)
4. Learn Dockerfile and build your first Python image
5. Learn volumes and shared folders (`-v`)
6. Learn Docker Compose for multi-container pipelines
7. Learn networking between containers (Python + Postgres)
8. Practice small pipelines: CSV → transform → Postgres
9. Scale to bigger pipelines: Airflow + Spark + Kafka

---

### ✅ TL;DR

* Docker = isolated, portable mini-computer
* Containers = run Python/DB/tools anywhere
* Volumes = share host files with container
* Dockerfile = blueprint for container
* Docker Compose = run multiple containers together
* Critical for real-world data engineering pipelines





