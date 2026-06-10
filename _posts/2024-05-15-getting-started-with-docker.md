---
title: "Getting Started with Docker: A Practical Guide"
categories:
  - DevOps
  - Containers
tags:
  - docker
  - containers
  - tutorial
date: 2024-05-15
---

Docker has revolutionized how we build, ship, and run applications. This guide walks through the essentials you need to start using Docker effectively.

## Why Docker?

Before containers, setting up a development environment meant installing databases, runtimes, and dependencies directly on your machine — leading to the classic "it works on my machine" problem. Docker solves this by packaging everything into isolated containers.

## Basic Concepts

### Images and Containers

An **image** is a read-only template with instructions for creating a container. A **container** is a runnable instance of an image.

```dockerfile
# Example Dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

### Key Commands

```bash
# Build an image
docker build -t my-app .

# Run a container
docker run -d -p 8080:80 --name my-container my-app

# View running containers
docker ps

# View logs
docker logs my-container

# Stop and remove
docker stop my-container && docker rm my-container
```

## Docker Compose for Multi-Service Apps

```yaml
# docker-compose.yml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8080:80"
    depends_on:
      - db
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

## Best Practices

1. **Use multi-stage builds** to keep images small
2. **Don't run as root** in your containers
3. **Use `.dockerignore`** to exclude unnecessary files
4. **Tag images meaningfully** — avoid `:latest` in production

Docker is one of those tools where an afternoon of learning pays dividends for years.
