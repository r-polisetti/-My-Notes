## Quick Reference Table

| Task                          | Command Example |
|-------------------------------|-----------------|
| Run container                 | `podman run -it ubuntu bash` |
| List containers               | `podman ps -a` |
| Stop container                | `podman stop mycontainer` |
| Remove container              | `podman rm mycontainer` |
| Build image                   | `podman build -t myapp .` |
| List images                   | `podman images` |
| Remove image                  | `podman rmi myapp` |
| Run in background             | `podman run -d -p 8080:80 nginx` |
| Inspect container             | `podman inspect mycontainer` |
| Logs                          | `podman logs mycontainer` |
| Exec into container           | `podman exec -it mycontainer bash` |
| Generate kube YAML            | `podman generate kube mycontainer` |
| Play kube YAML                | `podman play kube myapp.yaml` |

---

## Key Differences with Docker
- Podman is **daemonless** (no background service like Docker).  
- Runs in **rootless mode** → better security.  
- Podman CLI is **Docker-compatible** (`alias docker=podman`).  
- Supports **Pods** (group of containers sharing namespaces).  

---

## CLI Cheat Sheet

### Containers
```bash
podman run -it ubuntu bash
podman run -d -p 8080:80 nginx
podman ps
podman ps -a
podman stop mycontainer
podman rm mycontainer
````

### Images

```bash
podman build -t myapp .
podman images
podman rmi myapp
```

### Pods

```bash
podman pod create --name mypod -p 8080:80
podman run --pod mypod nginx
podman pod ps
podman pod stop mypod
```

### Volumes

```bash
podman volume create myvol
podman run -v myvol:/data busybox
podman volume ls
```

### Rootless Mode

Run Podman as a normal user (default). No need for `sudo`.

Check:

```bash
podman info | grep rootless
```

---

## Kubernetes Integration

* Convert running Podman containers into Kubernetes YAML:

  ```bash
  podman generate kube mycontainer > myapp.yaml
  ```

* Run workloads with Kubernetes YAML:

  ```bash
  podman play kube myapp.yaml
  ```

---

## Troubleshooting

* **Permission denied with volumes**
  Ensure correct ownership:

  ```bash
  sudo chown -R $USER:$USER ./data
  ```

* **Network not working**
  Restart Podman networking:

  ```bash
  podman system reset
  ```

* **Clean unused resources**

  ```bash
  podman system prune
  ```

---

## Best Practices

* Use **rootless Podman** for better security
* Use **Pods** to group related containers
* Use **systemd integration** (`podman generate systemd`) for auto-starting containers
* Use **Kubernetes YAML** for portability (`generate kube` / `play kube`)

---
