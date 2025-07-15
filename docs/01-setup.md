# 1. Setup and Configuration

This section covers the initial setup for Docker, including WSL (Windows Subsystem for Linux) configuration.

---

## ⚙️ WSL & Docker Setup

```bash
# Check WSL version
wsl --status

# Set WSL 1 as default (if needed)
wsl --set-default-version 1

# Check Docker version
docker version
```

## 🔁 Windows MySQL Service Management

If you are running MySQL locally on Windows, you might need to manage the service to avoid port conflicts with a MySQL container.

```powershell
# Stop the local MySQL service
net stop MYSQL80

# Start the local MySQL service
net start MYSQL80
