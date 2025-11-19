# Docker Networking & Network Sharing

## **1 Default Networking Behavior**

### **Bridge Network (Default)**
By default, Docker creates a `bridge` network named `bridge` when installed. If you do not specify a network, containers are attached to this default bridge.

**Characteristics:**
- Containers can communicate with each other using IP addresses.
- No built-in DNS resolution for container names.
- Requires manual linking or custom networks for name-based communication.

**Example:**
```bash
# Run two containers without specifying a network
docker run -d --name container1 nginx

docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_name_or_id>

docker run -it --name container2 alpine ping <container1_ip>
```
Here, you need to use IP addresses because the default bridge does not provide DNS resolution.

## **2 User-Defined Networks**

### **Overview**
User-defined networks provide better control and flexibility compared to the default `bridge` network. They allow custom IP ranges, improved isolation, and built-in DNS for container name resolution.

### **Types of User-Defined Networks**
- **Bridge**  
  Used for containers on a single host. Ideal for local development.
- **Overlay**  
  Enables communication between containers across multiple Docker hosts (requires Swarm or Kubernetes).
- **Macvlan**  
  Assigns MAC addresses to containers, making them appear as physical devices on the network.

### **Creating and Managing Networks**
```bash
# Create a custom bridge network
docker network create --driver bridge my-network

# Inspect network details
docker network inspect my-network

# Remove a network
docker network rm my-network
```

**Benefits:**
- Custom IP address ranges.
- Easier container discovery.
- Improved isolation compared to the default bridge.

---

## **3 Container-to-Container Communication**

### **Same Network Communication**
Containers on the same user-defined network can communicate using container names as DNS.

**Example:**
```bash
# Run two containers on the same network
docker run -d --network my-network --name app1 nginx
docker run -it --network my-network --name app2 alpine ping app1
```
Here, `app2` can ping `app1` using its name because Docker provides built-in DNS resolution.

### **Cross-Network Communication**
If containers are on different networks, you can connect them using:
```bash
docker network connect other-network app1
```

### **Service Discovery**
Docker automatically registers container names in its internal DNS, simplifying communication without hardcoding IP addresses.

---

## **4 Network Isolation and Port Management**

### **Isolation**
Containers on different networks cannot communicate unless explicitly connected. This prevents accidental data leaks and improves security.

**Scenario:**  
- **App1** on `frontend-network` cannot talk to **App2** on `backend-network` unless you connect them manually.

### **Port Mapping**
Expose container ports to the host using `-p` or `--publish`:
```bash
docker run -d -p 8080:80 nginx
```
This maps **port 80 inside the container** to **port 8080 on the host**.

---

## **5. Connecting Containers to Other Networks**

You can attach a container to multiple networks for cross-network communication.

**Example:**
```bash
# Create two networks
docker network create frontend-network
docker network create backend-network

# Run a container on the frontend network
docker run -d --network frontend-network --name webapp nginx

# Connect the same container to the backend network
docker network connect backend-network webapp
```
Now, `webapp` can communicate with containers on both networks.

**Use Case:**
- Multi-tier applications where a container needs access to both frontend and backend services.

---
