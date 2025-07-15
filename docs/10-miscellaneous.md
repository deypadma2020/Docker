# 10. Miscellaneous Topics

This section contains other useful commands and concepts that are helpful when working with Docker.

---

## 💢 YAML Example

YAML is a human-readable data serialization standard that is often used for configuration files, including Docker Compose.

```yaml
employee:
  name: john
  gender: male
  age: 24
  address:
    city: 'edison'
    state: 'new jersey'
    country: 'united states'
  payslips:
    - month: june
      amount: 1400
    - month: july
      amount: 2400
    - month: august
      amount: 3400
```

---

## ✅ Quick Tips

*   Use `docker inspect <name>` to get detailed, low-level information about containers and images.
*   Regularly run `docker system prune -a` to clean up unused containers, networks, images, and build cache.
*   Prefer Docker Compose for managing multi-container applications to simplify orchestration.
*   Always tag your images with meaningful versions before pushing them to a registry.
*   Test networking with user-defined bridges (`docker network create`) for better isolation.
