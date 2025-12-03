# Kubernetes Hands-On Exercises with k0s

## 1. Verify Cluster
```bash
kubectl get nodes
```

## 2. Work with a Pod

### Get Pods
```bash
k0s kubectl get pods
```

### Run a Pod
```bash
k0s kubectl run nginx --image nginx
```

### Pod Debugging
```bash
k0s kubectl describe pod nginx
k0s kubectl logs nginx
```

### Accessing Pod via IP
```bash
# Get the port
k0s kubectl describe pod nginx

# Get the IP
k0s kubectl get pod nginx -o wide

# Access using curl
curl pod-ip:port
```

### Accessing Pod via Service

#### Via Service IP
```bash
k0s kubectl expose pod nginx --port 80 --name nginx-svc
curl svc-ip:80
```

#### Via Service Hostname
```bash
curl nginx-svc.default.svc
```

#### Via Port Forwarding
```bash
k0s kubectl port-forward pod/nginx 8080:80
```
