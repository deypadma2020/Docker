# 7. Docker Registries

A Docker registry is a storage and distribution system for named Docker images.

---

## 🏷️ Tagging & Pushing to Docker Hub

Docker Hub is the default public registry. You need to tag your local images with your Docker Hub username before pushing.

```bash
# Tag a local image with a new name for Docker Hub
# format: docker tag <local_image>:<tag> <username>/<repository>:<tag>
docker tag clean_file-geocoder:latest deypadma/mapbox_geocoder:v1.0.0

# Log in to Docker Hub
docker login -u deypadma

# Push the tagged image to Docker Hub
docker push deypadma/mapbox_geocoder:v1.0.0

# Log out from Docker Hub
docker logout
```

### Complete Build, Tag, Push Workflow

```bash
# 1. Build the image
docker build -t testapp:1.0 .

# 2. Tag the image for your repository
docker tag testapp:1.0 deypadma/testapp:1.0

# 3. Push the image
docker push deypadma/testapp:1.0
```

---

## 🌐 Using Different Registries

### Docker Hub (Default)

```bash
# Pulls from docker.io/library/nginx
docker run nginx
```

### Google Container Registry (GCR)

```bash
# Pull an image from GCR
docker pull gcr.io/kubernetes-e2e-test-images/dnsutils
```

### Private Registry

You can run your own private registry to store images.

```bash
# 1. Start a local registry container
docker run -d -p 5000:5000 --name registry registry:2

# 2. Tag an image to point to the local registry
docker tag nginx localhost:5000/nginx

# 3. Push the image to the local registry
docker push localhost:5000/nginx

# 4. Pull the image from the local registry
docker pull localhost:5000/nginx

# 5. Check the catalog of the local registry
curl localhost:5000/v2/_catalog
