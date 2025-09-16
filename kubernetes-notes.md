# ☸️ Kubernetes Notes

## Quick Reference Table

| Task                          | Command Example |
|-------------------------------|-----------------|
| Check cluster info            | `kubectl cluster-info` |
| View all resources            | `kubectl get all` |
| View nodes                    | `kubectl get nodes` |
| View pods                     | `kubectl get pods` |
| Create resource from file     | `kubectl apply -f app.yaml` |
| Delete resource               | `kubectl delete -f app.yaml` |
| Scale deployment              | `kubectl scale deployment myapp --replicas=3` |
| View pod logs                 | `kubectl logs mypod` |
| Exec into pod                 | `kubectl exec -it mypod -- bash` |
| Port-forward service          | `kubectl port-forward svc/myservice 8080:80` |
| Describe resource             | `kubectl describe pod mypod` |
| Get resource YAML             | `kubectl get pod mypod -o yaml` |
| Delete pod                    | `kubectl delete pod mypod` |

---

## Core Concepts

### Pods
Smallest deployable unit. Runs one or more containers.  

👉 Example pod (`pod.yaml`):
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mycontainer
      image: nginx
      ports:
        - containerPort: 80
````

```bash
kubectl apply -f pod.yaml
kubectl get pods
```

---

### Deployments

Manages ReplicaSets and Pods. Used for scaling and rolling updates.

👉 Example (`deployment.yaml`):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: mycontainer
          image: nginx:latest
          ports:
            - containerPort: 80
```

```bash
kubectl apply -f deployment.yaml
```

---

### Services

Expose Pods inside or outside the cluster.

* **ClusterIP (default)** – internal communication
* **NodePort** – expose on each node’s IP
* **LoadBalancer** – cloud provider LB

👉 Example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myservice
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30007
```

---

### ConfigMaps & Secrets

* **ConfigMap** → store configs (non-sensitive)
* **Secret** → store sensitive data (base64 encoded)

👉 ConfigMap example:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
```

👉 Secret example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  password: cGFzc3dvcmQ=
```

---

### Volumes

Mount persistent storage into Pods.

👉 Example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-volume
spec:
  containers:
    - name: busybox
      image: busybox
      command: [ "sleep", "3600" ]
      volumeMounts:
        - name: myvolume
          mountPath: /data
  volumes:
    - name: myvolume
      emptyDir: {}
```

---

### Namespaces

Logical partition of cluster resources.

```bash
kubectl get namespaces
kubectl create namespace dev
kubectl apply -f app.yaml -n dev
```

---

## Troubleshooting

* **View logs**

  ```bash
  kubectl logs mypod
  ```

* **Exec into pod**

  ```bash
  kubectl exec -it mypod -- bash
  ```

* **Describe pod (events, issues)**

  ```bash
  kubectl describe pod mypod
  ```

* **Debug with ephemeral container**

  ```bash
  kubectl debug mypod -it --image=busybox
  ```

---

## Best Practices

* Use **Deployments**, not standalone Pods
* Use **ConfigMaps & Secrets** for configuration
* Use **Namespaces** to separate environments (dev, staging, prod)
* Apply **RBAC policies** for security
* Use **liveness & readiness probes** for health checks
* Monitor with **kubectl top**, Prometheus, Grafana

