# 9. Container Orchestration

Container orchestration automates the deployment, management, scaling, and networking of containers.

---

## 🚜 Docker Swarm

Docker Swarm is Docker's native orchestration tool.

```bash
# Initialize a new swarm (on a manager node)
docker swarm init

# Join an existing swarm as a worker node (run on worker)
docker swarm join --token <token> <manager-ip>:<port>

# Create a service with 3 replicas
docker service create --replicas=3 --name my-web-server nginx

# Scale a service to a new number of replicas
docker service scale my-web-server=5

# List running services
docker service ls

# Inspect a service
docker service inspect my-web-server

# Remove a service
docker service rm my-web-server
```

---

## 🌀 Kubernetes (k8s)

Kubernetes is a powerful, open-source container orchestration system.

```bash
# Run a deployment with 1000 replicas
kubectl run my-web-server --image=my-web-server --replicas=1000

# Scale the deployment to 2000 replicas
kubectl scale --replicas=2000 deployment/my-web-server

# Perform a rolling update to a new image version
kubectl rolling-update my-web-server --image=web-server:2

# Roll back to the previous version
kubectl rolling-update my-web-server --rollback

# Get information about the cluster
kubectl cluster-info

# Get a list of all nodes in the cluster
kubectl get nodes
```

---

## 💡 Docker Internals

Understanding the components that make Docker work.

```
Docker CLI → Docker Daemon → containerd → runc → Container
```

*   **Docker CLI**: The command-line tool you interact with.
*   **Docker Daemon**: The background service that manages containers.
*   **containerd**: An industry-standard container runtime that manages the complete container lifecycle.
*   **runc**: A lightweight, portable container runtime that creates and runs containers according to the OCI specification.
