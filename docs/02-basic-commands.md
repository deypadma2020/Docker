# 2. Basic Docker Commands

This section covers essential commands for managing images, containers, and system resources.

---

## 📦 Working with Docker Images & Containers

```bash
# List all Docker images
docker images

# Search for images (e.g., MySQL)
docker search mysql

# Run Ubuntu interactively
docker run -it ubuntu

# Inside container: list files, create a directory
ls
mkdir klizos
ls
```

## 🚦 Container Lifecycle Commands

```bash
# Show running containers
docker ps

# Show all containers (including stopped)
docker ps -a

# Stop a running container
docker stop <container_id_or_name>

# Remove a stopped container
docker rm <container_id_or_name>

# Attach to a running container's terminal
docker attach <container_id_or_name>
```

## 🛠️ Exec into Running Containers

This is often safer than `attach` as it starts a new process inside the container.

```bash
# Execute a bash shell in a running container
docker exec -it <container_id_or_name> bash

# Example: Check container's hosts file
cat /etc/hosts

# Example: Check container's OS release
cat /etc/os-release
```

## 🔄 Docker System Cleanup

```bash
# Stop and remove all containers forcefully
docker rm -f $(docker ps -aq)

# Remove all images
docker rmi -f $(docker images -q)

# Remove all unused volumes, networks, and dangling images
docker system prune -af --volumes
```

## 🚀 Miscellaneous Docker Commands

```bash
# Show Docker version
docker -v

# Detailed Docker version information
docker version

# Inspect a container or image for detailed information
docker inspect <container_or_image_name_or_id>

# Show container logs
docker logs <container_id_or_name>
