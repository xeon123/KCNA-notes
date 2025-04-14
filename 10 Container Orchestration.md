# Container Orchestration

## Cluster Networking

- **Node Configuration:**
    - Each **node** (master or worker) must have:
        - At least **one network interface** (e.g., `eth0`)
        - A **configured IP address**
        - A **unique hostname**
        - A **unique MAC address** (important when cloning VMs)

---

### 🌐 **Network & Ports Overview**

#### **Key Nodes:**

- `master-01`: `192.168.1.10`
- `worker-01`: `192.168.1.11`
- `worker-02`: `192.168.1.12`
#### **Key Ports to Open:**

There are some ports that need to be open, because they are going to be used by the Kubernetes components.

| Component               | Port        | Node Type       | Description                                  |
| ----------------------- | ----------- | --------------- | -------------------------------------------- |
| Kube API Server         | 6443        | Master          | Entry point for all API calls                |
| Kubelet                 | 10250       | Master & Worker | Manages containers on nodes                  |
| Kube Scheduler          | 10251       | Master          | Schedules pods                               |
| Kube Controller Manager | 10252       | Master          | Maintains cluster state                      |
| Services (NodePort)     | 30000–32767 | Workers         | External access to services                  |
| etcd                    | 2379        | Master          | Cluster data store                           |
| etcd                    | 2380        | Master          | To etcd clients communicate with each other. |

If there are multiple master nodes, it is also necessary to open port 2380.

## Pod Networking

**Summary of Kubernetes Pod Networking Setup**

After setting up Kubernetes master and worker nodes with proper networking and control plane components (kube-apiserver, etcd, kubelets, etc.), the focus shifts to Pod networking, a critical layer for cluster functionality. Kubernetes requires a networking solution to address Pod communication, but it does not provide a built-in solution. Instead, it defines clear requirements for Pod networking, which must be implemented using a Container Network Interface (CNI) plugin or custom solution.

### Kubernetes Pod Networking Requirements

1. **Unique IP Address**: Every Pod must have its own unique IP address.
2. **Intra-Node Communication**: Pods on the same node must communicate with each other using their IP addresses.
3. **Inter-Node Communication**: Pods on different nodes must communicate without Network Address Translation (NAT).

### Implementation Approach

To meet these requirements, a manual networking setup is described, which helps understand how CNI plugins work. The process is applied to a three-node cluster (Node1: 192.168.1.11, Node2: 192.168.1.12, Node3: 192.168.1.13) connected via a LAN (192.168.1.0/24).

#### Steps for Pod Networking

1. **Create Bridge Networks**:
    - On each node, a bridge network (v-net-0) is created to connect Pods.
    - Each bridge is assigned a unique subnet:
        - Node1: 10.244.1.0/24 (bridge IP: 10.244.1.1)
        - Node2: 10.244.2.0/24 (bridge IP: 10.244.2.1)
        - Node3: 10.244.3.0/24 (bridge IP: 10.244.3.1)
    - Commands:
        

    ```bash
    ip link add v-net-0 type bridge
	ip link set dev v-net-0 up
	ip addr add <bridge-ip>/24 dev v-net-0
	```

        
2. **Connect Pods to Bridge**:
    - For each Pod, Kubernetes creates a network namespace.
    - A virtual Ethernet (veth) pair is created to connect the Pod’s namespace to the bridge:
        - One end (veth-red) is placed in the Pod’s namespace.
        - The other end (veth-red-br) is attached to the bridge.
    - Assign a unique IP from the node’s subnet (e.g., 10.244.1.2 for a Pod on Node1).
    - Commands are scripted (net-script.sh) for automation:
    ```bash
	ip link add veth-red type veth peer name veth-red-br
	ip link set veth-red netns <pod-namespace>
	ip -n <pod-namespace> addr add <pod-ip> dev veth-red
	ip link set veth-red-br master v-net-0
	ip -n <pod-namespace> link set veth-red up
	ip -n <pod-namespace> route add default via <bridge-ip>
	```
	
3. **Enable Inter-Node Pod Communication**:
    - Pods on different nodes need routing to communicate.
    - Add routes on each node to direct traffic to other nodes’ subnets via their external IPs:

    ```bash
# On Node1
ip route add 10.244.2.0/24 via 192.168.1.12
ip route add 10.244.3.0/24 via 192.168.1.13
# On Node2
ip route add 10.244.1.0/24 via 192.168.1.11
ip route add 10.244.3.0/24 via 192.168.1.13
# On Node3
ip route add 10.244.1.0/24 via 192.168.1.11
ip route add 10.244.2.0/24 via 192.168.1.12
```
        
    - Alternatively, configure a router with routes for all subnets (e.g., 10.244.0.0/16) and set it as the default gateway for nodes to simplify management.
4. **Automate with CNI**:
    - Manual scripting is impractical for large clusters, so the script is adapted for CNI compatibility.
    - The script includes:
        - **ADD**: Creates veth pair, assigns IP, connects to bridge, and sets routes.
        - **DEL**: Removes veth pair and frees IP when a Pod is deleted.
    - Kubelet uses CNI configuration (/etc/cni/net.d) and binaries (/etc/cni/bin) to execute the script automatically when Pods are created or deleted:

        ```bash
        ./net-script.sh add <container> <namespace>
        ```
        

### Outcome

- Pods receive unique IPs (e.g., 10.244.1.2, 10.244.2.2) and can communicate within and across nodes without NAT.
- The setup ensures scalability by scripting repetitive tasks and integrating with CNI for automation.
- While this is a basic solution, production clusters typically use CNI plugins (e.g., Flannel, Calico) that handle these tasks more efficiently.
- 

## CNI in Kubernetes


## CNI weave


## DNS in Kubernetes


## Ingress