# 5. Docker Networking

This section covers how to manage and create networks in Docker to allow containers to communicate with each other.

---

## 🌐 Manual Networking

You can create user-defined bridge networks to provide better isolation and interoperability between containers.

```bash
# Create a new bridge network
docker network create voting-net

# List all networks
docker network ls

# Inspect a network to see its configuration and connected containers
docker network inspect <network_name_or_id>

# Remove a network
docker network rm <network_id_1> <network_id_2>
```

### Example: Creating a Custom Network

```bash
docker network create \
  --driver bridge \
  --subnet 182.18.0.0/24 \
  --gateway 182.18.0.1 \
  wp-mysql-network
```

---

## 🧱 Building Multi-Container Voting App (Manual Network)

This example shows how to manually build and connect containers for a voting application on a custom network.

```bash
# 1. Build the application images
cd vote/ && docker build -t voting-app .
cd ../worker/ && docker build -t worker-app .
cd ../result/ && docker build -t result-app .

# 2. Create a network
docker network create voting-net

# 3. Run dependency containers on the network
docker run -d --network=voting-net --name redis redis
docker run -d --network=voting-net --name db \
  -e POSTGRES_PASSWORD=qwerty postgres:9.4

# 4. Run application containers on the network
docker run -d --network=voting-net -p 5000:80 --name voting-app voting-app
docker run -d --network=voting-net -p 5001:80 --name result-app result-app
docker run -d --network=voting-net --name worker-app worker-app
```

---

## MongoDB & Mongo Express Networking

This demonstrates connecting a MongoDB container with a Mongo Express web UI on the same network.

```bash
# 1. Create a network
docker network create mongo-network

# 2. Run the MongoDB container on the network
docker run -d -p 27017:27017 --name mongo \
  --network mongo-network \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=qwerty mongo

# 3. Run the Mongo Express container, linking it to the MongoDB container
docker run -d -p 8081:8081 --name mongo-express \
  --network mongo-network \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_URL="mongodb://admin:qwerty@mongo:27017" \
  mongo-express
