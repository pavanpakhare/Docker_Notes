# Docker_Notes
# Docker Tutorial: From Basic to Advanced

## Table of Contents
1. [Introduction to Docker](#introduction-to-docker)
2. [Installation](#installation)
3. [Basic Docker Commands](#basic-docker-commands)
4. [Working with Docker Images](#working-with-docker-images)
5. [Managing Containers](#managing-containers)
6. [Docker Networking](#docker-networking)
7. [Docker Volumes](#docker-volumes)
8. [Docker Compose](#docker-compose)
9. [Dockerfile Deep Dive](#dockerfile-deep-dive)
10. [Best Practices](#best-practices)
11. [Advanced Topics](#advanced-topics)

## Introduction to Docker

### What is Docker?
Docker is an open-source platform for developing, shipping, and running applications in containers. Containers allow you to package an application with all its dependencies into a standardized unit for software development.

### Key Concepts
- **Container**: A lightweight, standalone, executable package that includes everything needed to run a piece of software
- **Image**: A read-only template with instructions for creating a container
- **Dockerfile**: A text document containing commands to assemble an image
- **Registry**: A repository for Docker images (Docker Hub is the default public registry)

### Benefits of Docker
- Consistent environments
- Isolation
- Portability
- Scalability
- Resource efficiency

## Installation

### Linux (Ubuntu)
```bash
# Update packages
sudo apt-get update

# Install prerequisites
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Add Docker repository
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Install Docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io

# Verify installation
sudo docker run hello-world
```

### Windows/macOS
Download and install Docker Desktop from [Docker's official website](https://www.docker.com/products/docker-desktop)

## Basic Docker Commands

### Check Docker version
```bash
docker --version
docker info
```

### Run a container
```bash
docker run hello-world
```

### List containers
```bash
docker ps          # Show running containers
docker ps -a       # Show all containers
```

### Stop/Start containers
```bash
docker stop <container_id>
docker start <container_id>
```

### Remove containers
```bash
docker rm <container_id>
docker container prune  # Remove all stopped containers
```

## Working with Docker Images

### Search for images
```bash
docker search nginx
```

### Pull an image
```bash
docker pull nginx
```

### List images
```bash
docker images
```

### Remove images
```bash
docker rmi <image_id>
docker image prune  # Remove unused images
```

### Inspect an image
```bash
docker inspect <image_id>
```

## Managing Containers

### Run a container in interactive mode
```bash
docker run -it ubuntu /bin/bash
```

### Run a container in detached mode
```bash
docker run -d nginx
```

### Execute commands in a running container
```bash
docker exec -it <container_id> /bin/bash
```

### View container logs
```bash
docker logs <container_id>
```

### Resource limitations
```bash
docker run -it --memory="1g" --cpus="1.0" ubuntu
```

## Docker Networking

### List networks
```bash
docker network ls
```

### Create a network
```bash
docker network create my-network
```

### Connect a container to a network
```bash
docker run --network=my-network nginx
```

### Network types
- bridge (default)
- host
- none
- overlay (for swarm)

### Port mapping
```bash
docker run -p 8080:80 nginx  # HostPort:ContainerPort
```

## Docker Volumes

### Create a volume
```bash
docker volume create my-volume
```

### List volumes
```bash
docker volume ls
```

### Mount a volume
```bash
docker run -v my-volume:/data nginx
```

### Bind mounts (host directories)
```bash
docker run -v /host/path:/container/path nginx
```

## Docker Compose

Docker Compose is a tool for defining and running multi-container applications.

### Basic example (docker-compose.yml)
```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: example
```

### Compose commands
```bash
docker-compose up -d
docker-compose down
docker-compose ps
docker-compose logs
```

## Dockerfile Deep Dive

### Basic Dockerfile example
```dockerfile
FROM node:14
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

### Common Dockerfile instructions
- `FROM`: Base image
- `RUN`: Execute commands
- `COPY`: Copy files from host to container
- `ADD`: Similar to COPY but with additional features
- `WORKDIR`: Set working directory
- `EXPOSE`: Document which ports are exposed
- `CMD`: Default command to run
- `ENTRYPOINT`: Configure a container to run as an executable
- `ENV`: Set environment variables
- `ARG`: Build-time variables
- `VOLUME`: Create mount point

### Multi-stage builds
```dockerfile
# Build stage
FROM node:14 as builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Production stage
FROM nginx:alpine
COPY --from=builder /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

## Best Practices

1. **Use official images** when possible
2. **Keep images small**
   - Use alpine-based images
   - Clean up in the same RUN command
   - Use multi-stage builds
3. **One process per container**
4. **Use .dockerignore** to exclude unnecessary files
5. **Don't run as root** (use USER instruction)
6. **Use specific tags** instead of 'latest'
7. **Leverage build cache** (order instructions properly)
8. **Scan images for vulnerabilities**
9. **Use health checks**
10. **Document with LABEL**

## Advanced Topics

### Docker Swarm
```bash
# Initialize swarm
docker swarm init

# Join nodes
docker swarm join --token <token> <manager-ip>:2377

# Deploy a service
docker service create --name web --replicas 3 -p 8080:80 nginx
```

### Kubernetes Basics
```bash
# Minikube (local Kubernetes)
minikube start

# Kubectl commands
kubectl get pods
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=LoadBalancer
```

### Docker Security
- Content trust
- Secrets management
- Image signing
- User namespaces
- AppArmor/SELinux profiles

### CI/CD Integration
- GitHub Actions
- GitLab CI
- Jenkins
- CircleCI

### Monitoring and Logging
- Prometheus
- Grafana
- ELK Stack
- Fluentd

### Custom Plugins and Extensions
- Docker SDK
- Building custom storage drivers
- Network plugins

