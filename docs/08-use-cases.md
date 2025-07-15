# 8. Practical Use Cases

This section provides examples of running common services and applications inside Docker containers.

---

## 🐘 MySQL in Docker

```bash
# Pull the latest MySQL image
docker pull mysql

# Run MySQL on the default port
docker run -p 3306:3306 mysql

# Run MySQL on a custom port with a root password
docker run -d -p 8306:3306 \
-e MYSQL_ROOT_PASSWORD=secret \
--name my-sql mysql

# Inspect the container for details like its IP address
docker inspect my-sql
```

### MySQL & Web App Stack

```bash
# Pull and run a MySQL database
docker pull mysql
docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=db_pass123 -d mysql

# Run a Redis container and a web app that connects to it
docker run -d --name redis redis:alpine
docker run -d --name clickcounter --link redis -p 8085:5000 kodekloud/click-counter
```

---

## 🌐 Web Apps with Docker

```bash
# Expose a web app on different host ports
docker run -p 80:5000 kodekloud/webapp
docker run -p 8000:5000 kodekloud/webapp
docker run -p 8001:5000 kodekloud/webapp

# Run a web app in detached mode
docker run -d -p 38282:8080 kodekloud/simple-webapp:blue
```

---

## 🐍 Flask App Inside Docker (Manual Setup)

This demonstrates how to set up and run a Flask application from within a bare Ubuntu container.

```bash
# 1. Start an interactive Ubuntu container
docker run -it ubuntu bash

# 2. Install Python and Flask inside the container
apt-get update && apt-get install -y python3 python3-pip
apt-get install -y python-is-python3
pip install flask --break-system-packages

# 3. Create the Flask application file
cat > /app.py
```

```python
# app.py
import os
from flask import Flask

app = Flask(__name__)

@app.route("/")
def main():
    return "Welcome to Klizos"

@app.route("/How-are-you-doing?")
def hello():
    return "I am good. What about you?"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8080)
```

```bash
# 4. Run the Flask server inside the container
FLASK_APP=app.py flask run --host=0.0.0.0

# 5. In another terminal on your host, test the app
docker exec -it <container-id> bash
apt update && apt install -y curl
curl http://localhost:5000
```

---

## ⚙️ Jenkins with Docker

```bash
# Run a basic Jenkins container
docker run jenkins/jenkins
# Access it at http://localhost:8080/

# Run Jenkins with a persistent volume to save data
mkdir my-jenkins-data
docker run -p 8080:8080 \
-v /d/padma/DOCKER/my-jenkins-data:/var/jenkins_home \
-u root jenkins/jenkins
```

---

## 🧪 Working with Linux Distros

```bash
# Run an interactive Ubuntu shell
docker run -it ubuntu bash

# Run an interactive CentOS 7 shell
docker run -it centos:7 bash

# Run a container in detached mode that sleeps for a long time
docker run -d centos:7 sleep 2000

# Check the OS version inside a container
cat /etc/os-release
