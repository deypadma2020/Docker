# 4. Using Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications. It uses a YAML file to configure the application's services.

---

## 🛠️ Docker Compose Commands

```bash
# Start all services defined in docker-compose.yml and build images if they don't exist
docker-compose up

# Start services and force a rebuild of the images
docker-compose up --build

# Run services in detached mode (in the background)
docker-compose up -d

# Stop and remove containers, networks, volumes, and images created by 'up'
docker-compose down

# Add -v to remove named volumes, --rmi all to remove all images
docker-compose down -v --rmi all --remove-orphans
```

---

## 🌐 Docker Compose Examples

### Voting App (Version 3)

This example defines a multi-service application with dependencies.

```yaml
version: '3.8'
services:
  redis:
    image: redis

  db:
    image: postgres:9.4
    environment:
      POSTGRES_PASSWORD: qwerty

  vote:
    image: voting-app
    ports:
      - "5000:80"
    depends_on:
      - redis

  worker:
    image: worker-app
    depends_on:
      - db
      - redis

  result:
    image: result-app
    ports:
      - "5001:80"
    depends_on:
      - db
```

### Web App with Redis

A simpler example showing a web application that depends on a Redis cache.

```yaml
version: '3'
services:
  redis:
    image: redis:alpine
  clickcounter:
    image: kodekloud/click-counter
    ports:
      - "8085:5000"
    depends_on:
      - redis
```

### MongoDB with Mongo Express

This `docker-compose.yml` sets up a MongoDB database and a web-based admin interface.

```yaml
# Filename: mongodb.yaml
version: '3'
services:
  mongo:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=qwerty
    volumes:
      - mongo-data:/data/db
  mongo-express:
    image: mongo-express
    ports:
      - "8081:8081"
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=qwerty
      - ME_CONFIG_MONGODB_SERVER=mongo
    depends_on:
      - mongo
volumes:
  mongo-data:
    driver: local
```

```bash
# Use -f to specify a non-default compose file name
docker-compose -f mongodb.yaml up -d
docker-compose -f mongodb.yaml down
