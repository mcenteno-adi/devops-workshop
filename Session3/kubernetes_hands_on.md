# Kubernetes Hands-On Exercises with k0s

## 1. Verify Cluster and Context
```bash
kubectl get nodes
kubectl config current-context
kubectl config get-contexts
kubectl config --help
```

## 2. Work with a Pod

### Get Pods
```bash
kubectl get pods
```

### Run a Pod
```bash
kubectl run nginx --image nginx --port 80
```

### Pod Debugging
```bash
kubectl describe pod nginx
kubectl logs nginx
```

### Accessing Pod via IP
```bash
# Get the port
kubectl describe pod nginx

# Get the IP
kubectl get pod nginx -o wide

# Access using curl
curl pod-ip:port
```

### Accessing Pod via Service

#### Via Service IP
```bash
kubectl expose pod nginx --port 80 --name nginx-svc
curl svc-ip:80
```

#### Via Service Hostname
```bash
curl nginx-svc.default.svc
```

#### Via Port Forwarding
```bash
kubectl port-forward pod/nginx 8080:80
```
