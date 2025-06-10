# Project with Compose

# 🔐 Docker Security Best Practices

### 1. **Use Minimal Base Images**

* Prefer slim or distroless images (e.g., `alpine`, `ubuntu:20.04`)
* Reduces attack surface and vulnerabilities

---

### 2. **Avoid Running as Root**

* Specify a non-root user in the Dockerfile using:

  ```dockerfile
  USER appuser
  ```

---

### 3. **Use Multi-Stage Builds**

* Reduce final image size by separating build-time dependencies from runtime.

---

### 4. **Scan Images for Vulnerabilities**

* Use tools like:

  * `docker scan`
  * Trivy, Clair, or Anchore
* Example:

  ```bash
  docker scan my-image
  ```

---

### 5. **Keep Images Updated**

* Regularly rebuild images with updated base layers and patched packages.

---

### 6. **Limit Container Capabilities**

* Drop unnecessary Linux capabilities:

  ```bash
  docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE my-image
  ```

---

### 7. **Use Read-Only Filesystems**

* Prevent containers from writing to the file system:

  ```bash
  docker run --read-only ...
  ```

---

### 8. **Avoid Mounting Sensitive Host Paths**

* Don’t mount host’s `/var/run/docker.sock`, `/etc`, `/root`, or other critical directories unless necessary.

---

### 9. **Use Docker Secrets for Sensitive Data**

* Avoid passing secrets (like passwords, API keys) via ENV variables.

---

### 10. **Restrict Network Access**

* Use custom user-defined bridge networks.
* Isolate containers to prevent unnecessary exposure.

---

### 11. **Enable Resource Limits**

* Prevent DoS by limiting CPU and memory:

  ```bash
  docker run --memory=512m --cpus=1 ...
  ```

---

### 12. **Use Signed Images (Content Trust)**

* Enable Docker Content Trust (DCT) to verify image signatures:

  ```bash
  export DOCKER_CONTENT_TRUST=1
  ```

---

### 13. **Don’t Expose Unused Ports**

* Only expose necessary ports using `-p` or `EXPOSE`.
---


