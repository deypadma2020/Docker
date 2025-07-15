# 3. Writing Dockerfiles

This section provides best practices and examples for creating your own Docker images using a `Dockerfile`.

---

## 📄 Dockerfile (Ubuntu + Python + Flask)

This example sets up a basic Flask web application on an Ubuntu base image.

```Dockerfile
FROM ubuntu

# Install Python and Flask
RUN apt-get update && \
    apt-get install -y python3 python3-pip && \
    apt-get install -y python-is-python3 && \
    pip install flask --break-system-packages

# Copy the application code into the image
COPY app.py /app.py

# Set the command to run when the container starts
ENTRYPOINT FLASK_APP=/app.py flask run --host=0.0.0.0
```

### 🐍 app.py

```python
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

### 🔧 Build & Run

```bash
# Build the Docker image from the Dockerfile
docker build -t my-simple-webapp .

# Run the container in detached mode, mapping port 8080 on the host to 5000 in the container
docker run -d -p 8080:5000 my-simple-webapp
```

---

## 🐍 Alternate Build (Python Slim Image)

Using a specialized base image like `python:3.6-slim` results in a smaller, more efficient image.

### Dockerfile

```Dockerfile
FROM python:3.6-slim

# Set the working directory inside the container
WORKDIR /app

# Copy application files
COPY . .

# Install dependencies from requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Expose the port the app runs on
EXPOSE 5000

# Define the command to run the application
CMD ["python", "app.py"]
```

### requirements.txt

```
flask
```

### Build & Run

```bash
docker build -t webapp-color:lite .
docker run -p 8383:8080 webapp-color:lite
```

---

## 🛠️ Dockerfile (with MySQL Client)

This example shows how to include additional system packages, like the MySQL client.

```Dockerfile
FROM python:3.9.12-slim
WORKDIR /app

COPY requirements.txt .
COPY app.py .

# Install MySQL client and clean up apt cache to keep image small
RUN apt-get update && \
    apt-get install -y default-mysql-client --no-install-recommends && \
    pip install --no-cache-dir -r requirements.txt && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

CMD ["python", "app.py"]
