---
tags:
  - containers
  - Docker
  - Kubernetes
  - virtualization
title: Containers with Docker
category: KCNA Notes
topics:
  - Containers
  - Docker
  - Virtualization
  - Linux Namespaces
  - cgroups
  - Container Images
  - Container Networking
created: 2025-03-31
updated: 2025-03-31
author: reissada
status: draft
summary: This note explores the evolution of containers, Docker's role in containerization, key technologies like Linux namespaces and cgroups, container images, networking, and best practices for managing containers.
---

# Containers with Docker

## Introduction to containers

### **Transformative Impact of Containers**

- Containers revolutionized infrastructure, application design, development, and operations.
	- **Isolation**: Containers encapsulate dependencies, eliminating conflicts.
	- **Portability**: Applications run the same way on any system.
	- **Quick Setup**: Developers can start working with a single command (`docker run`).
- Changed how operating systems are used and applications are managed at scale, particularly with tools like Kubernetes.
- Enabled efficient management of thousands of containers, fostering scalability and interoperability.

### **Historical Foundations**

1. **Mainframe Virtualization (1960s)**
   - IBM CP/CMS allowed multiple users to access virtualized OS instances on a single mainframe.
   - Marked the beginning of virtual machine architectures.
   - Each user had its own version of OS virtualized in the machine
2. **Chroot (1979)**
   - Introduced in Unix Version 7, enabling process isolation by changing the root directory.
     - Changes the root directory for a process and its children.
       - **chroot is a process that allows you to change the root directory** for a running process and its children, effectively isolating them from the rest of the system. This is commonly used in containerization and virtualization technologies.
     - This enabled process and its children can only see the files and directories within the new root directory. Prevented to access the files from outside the root directory.
   - Limited by shared resources (e.g., hostname, IP addresses are still visible and accessible). Cannot run superuser processes, directories must be `root:root` owned.
3. **FreeBSD Jails (2000)**
   - Provided lightweight virtualization by isolating system resources within FreeBSD. Allow partitioning of FreeBSD into isolated environments with their own set of users, process, file system, and networking stack.
   - This is similar to Docker, but it was complex and lacked widespread adoption despite strong security features.
4. **Zones and Virtual Partitions (2000s)**
   - Solaris Zones and HP-UX virtual partitions aimed to isolate compute resources.
   - Solaris provided a virtualization technology called Zones that allowed the Solaris OS to be divided into multiple isolated environments.
   - HP-UX provided similar technology known as Virtual Partitions to segment a system into logical partitions for virtualization.
   - Highlighted the industry's focus on resource isolation.

### **Modern Ingredients for Containers**

Docker brought together two key ingredients: **Linux Namespaces and control groups (cgroups)**. Linux Namespaces provide a **way to isolate processes at the kernel level**, while cgroups allow for **resource allocation and isolation**. By combining these technologies, Docker created a lightweight containerization platform that provided strong isolation guarantees.

Docker was originally called **DotCloud**, which was a cloud platform-as-a-service (PaaS) company founded in 2008 by Solomon Hykes and Sebastien Pahl. The project that would eventually become Docker was initially developed as an internal tool for automating the deployment of applications on the DotCloud platform.

1. **Linux Namespaces (2002)**
   - In 2002, the Linux kernel was modified to include **6 new namespaces**.
   - Key for containerization, isolating processes, network stacks, mount points, hostnames, and inter-process communication.
   - The network namespace in Linux is used to p**rovide a complete, isolated networking stack for each namespace**. This means that each namespace can have its own IP addresses, routing tables, firewall rules, and other networking configurations, which are separate from those of other namespaces and the host system.
   - Examples include `PID`, `NET`, `MNT`, `UTS` (Unix Timesharing System), `IPC`, and `USER` namespaces.
     - `USER`: Isolate the user ID space allowing processes within a namespace to have their own unique set of ids seprarate from the rest of the system.
     - `PID`: allow processes in a namespace to have their own unique set of processes IDs without conflicting with each other.
     - `NET`: Processes within a namespace have their own independent networking stack, allowing communication between processes in the same namespace.
       - Provide an isolated networking stack with its own IP addresses, routing tables, firewall rules, and other networking configurations
     - `MNT`: an independent filesystem hierarchy providing processes with an independent view of the filesystem, able to mount and unmount filesystems within a namespace.
     - `UTS`: Unix Time Sharing for isolating the hostname and domain names.
     - `IPC`: Inter-process communication namespace, allowing for exchange of data between processes in forms of messages.
2. **Control Groups (cgroups, 2006)**
   - Developed by Google, merged into Linux in 2008.
   - Enabled
     - resource limits: how much of a resource can be used
     - prioritization: related to resource limits and the priority each process has
     - accounting: monitoring and reporting of resource limits
     - control: ability to control processes within a cgroup (started, stopped, frozen, restarted)
3. **Rise of Virtual Machines**
   - ![[Containers with Dockers VMs.png]]
   - Hypervisors (e.g., VMware ESXi) allowed isolated virtual machines to share physical hardware, providing an abstraction layer between physical resources and system guest.
     - Advantages: flexibility, advanced features (e.g., live migration of VMs between HW platforms, support for GPU, Cloud Computing and Virtual Desktop Infrastructure (VDI)).
     - Disadvantages: resource segmentation (the resources are segmented between different VMs), OS overhead, licensing costs.
   - Over the time, Hypervisor and the OS became a unified component, bringing an OS overhead.
   - Amazon EC2 is one of the biggest VMs offerings in the world.

### **Docker: A Game-Changer**

![[Containers with Dockers containarised deployments.png]]

- Launched in 2013, Docker combined namespaces and `cgroups` into a user-friendly platform.
- Simplified adoption of containerization, enabling scalable and efficient application management.
  - Docker containers are preferred over virtual machines because they are lightweight and efficient in terms of resource usage.
  - Containers share the same kernel as the host operating system, which means that they don't require a separate operating system instance like virtual machines do.
- Container runtime is on the top of the OS and maximizes the use of available resources.
  - We can run applications via the container runtime.
- Containers share the host OS kernel, reducing overhead compared to virtual machines.
  - There is no OS layer for each container, as the container shares the kernel of the main OS.
- Uses namespaces and `cgroups` to provide isolated environment within their own users, hostnames, IP addresses, mount points and resource allocations.
- The success of Docker can be attributed to the ease of use for adoption. Docker brought containerization to the market with simple CLI based offering.

### Key Takeaways

- Containerization evolved from mainframe virtualization, Unix advancements, and Linux innovations.
- Docker and Kubernetes made these technologies accessible and practical for modern applications.
- Containers leverage isolation (namespaces) and resource control (cgroups) for lightweight, scalable, and efficient deployment.

## Docker installation

### Traditional Docker Architecture

- **Layers**: Hardware → OS → Docker runtime (containerd and `runc`).
- **Minimum requirement**: Linux kernel version 3.1 or higher.
- **Use case**: CLI for building and running containers.

---

### Docker Desktop Architecture

- Provides a friendly UI for Mac, Windows, and Linux.
- **Runs a hidden VM/subsystem for Docker runtime isolation.**
- Supports Docker CLI, Kubernetes integration, and Docker Desktop Extensions (marketplace for bundled applications).
  - Docker Extensions in Docker Desktop allow **developers to bundle Docker-based applications** into a single package that can be easily installed and run by end-users. This feature enables developers to create self-contained applications that include all the necessary dependencies, making it easier for users to get started with their apps.
- Key feature: Flexibility to reset the Docker and Kubernetes environment with a click.

### Using Docker Desktop

One of the primary benefits of using Docker Desktop is that it provides a highly flexible and self-contained environment for running containers. If something goes wrong or you need to start from scratch, you can simply reset the Docker Desktop VM and start again with a clean slate. This makes it ideal for development, testing, and learning environments.

- **Running Containers**:
  Example: `docker run -it ubuntu bash` to interact with a Ubuntu container.
  - -i : keeps the container’s standard input open to enable interaction with the container
  - -t: allocates TTY for the container to simulate a terminal interface, making the container behave as if a terminal was connected.
- **Managing Resources**:
  Adjust resource limits (Mac), though limited on Windows (WSL).
- **Kubernetes Integration**:
  Enable Kubernetes in preferences to run and manage Kubernetes commands (`kubectl`).

## Container Images

A **container image** is a portable, self-contained bundle of software and its dependencies, enabling consistent software execution across environments. Often called "Docker images," they are more accurately referred to as **OCI-compliant container images**. These images can be used with Docker, Kubernetes, or any platform supporting OCI standards.

### Key Points

#### 1. **Container Image vs. Container:**

- A **container image** is the blueprint (software bundle of software and dependencies). The correct term is not Docker image, is an OCI Compliant Container image. OCI image can run on docker, kubernetes, AWS ECS (`https://opencontainers.org`)
- A **container** is a running instance of that image. For example, multiple containers (instances) can run from one `nginx` image.

#### 2. **Tags in Container Images:**

- Tags distinguish image versions or variants (e.g., `latest`, `v1.0`).
- The `latest` tag doesn’t always represent the most recent image; it’s a default tag in Docker when a tag is not specified.

#### 3. **Layers in Container Images:**

- ![[Containers with Dockers Docker Container Layers-1.png]]
- Images are built as stacks of layers, each representing a step in the build process.
- Layers are shared between containers, saving storage.
- If you have too many layers, can lead to several drawbacks

##### - 1. **Increased Build Time**

- Each layer adds an overhead during the build process, making it slower, especially when changes occur in early layers, causing subsequent layers to rebuild.

##### - 2. **Larger Image Size**

- More layers mean more metadata and storage overhead, potentially leading to bloated images that consume more disk space and take longer to transfer.

##### - 3. **Slower Caching & Performance Issues**

- Docker caches layers to speed up builds, but excessive layers can lead to inefficient caching, as small changes may invalidate many layers, forcing unnecessary rebuilds.

##### - 4. **Complexity in Layer Management**

- When too many layers exist, it becomes harder to track changes, optimize images, and troubleshoot issues like unnecessary files or dependencies.

##### - 5. **Storage and Filesystem Limitations**

- Some filesystems have limits on the number of layers that can be efficiently handled, causing performance degradation or failures in extreme cases.

#### 4. **Union File System:**

- Docker make use of a Union File System.
- Merges layers into a single view for the user.
- On the top of the layers there is a writable layer.
  - Changes during runtime are stored in the writable layer, leaving underlying layers unchanged. E.g., if we delete a file that is part of a container image, we only make a reference in the writable layer. The file would still exists, but will no longer be visible to us.
  - The advantages is that if we are using multiple containers, we are using space efficiently as the only layer that changes is the thin writable layer.

#### 5. **Digests and Image IDs:**

- A **digest** (SHA-256 hash) uniquely **identifies an image version in a registry.**
- **A checksum taken from a container registry**, which represents the contents of an image as it exists in the registry, used to verify the integrity of images when they are pulled or pushed between registries.
- An **image ID** is a local identifier based on the JSON configuration used by Docker.
- An image ID is a **checksum based on the local container image**, which **represents the actual bytes stored on disk**. Image IDs are typically used by Docker and other container runtimes to identify and manage images locally.
- It is possible to pull the image using the digest
  - ![[Containers with Dockers docker pull with digest.png]]
- The digest is the checksum taken from a container registry, and is created when an images is pushed to the registry, and it is used to verify the integrity and authenticity of the image.
  - The digest represents the hash of the image manifest, which contains critical information about the image.
- The image ID is a checksum derived from the image json configuration file used by Docker for this image when building the container. The json configuration file contais the metadata of the image, including its layers, tags, and other details.

#### 6. **Saving and Viewing Images:**

- Images can be saved locally with `docker save` for backup or transfer.
  - ![[Containers with Dockers docker save.png]]
  - If we extract, we can see 5 layers
    - ![[Containers with Dockers docker image layers.png]]
    - The image id file, is the internal config that docker is using for the image id.
      - ![[Containers with Dockers docker image internal config.png]]
        - The image id hash is the checksum of this file.
- JSON files in saved images contain metadata, and the image ID is derived from their hash.

##### **Best Practices to Optimize Layers**

- ✅ **Use multi-stage builds** to reduce final image size.
- ✅ **Minimize RUN commands** by chaining them together (`RUN apt-get update && apt-get install -y package`).
- ✅ **Remove unnecessary files** in the same layer they were created (`rm -rf /var/lib/apt/lists/*`).
- ✅ **Start from minimal base images** (e.g., `alpine`, `distroless`).
- ✅ Containers add a writable layer on top for changes during runtime.

## Container Orchestration

- Kubernetes is supported on all major public cloud providers: GCP, Azure, and AWS.
- Kubernetes is one of the top-ranked projects on GitHub.
- Container orchestration provides several advantages:
    - **High availability**: Applications remain up even during hardware failures because they run on multiple instances across different nodes.
    - **Load balancing**: User traffic is distributed across various containers.
    - **Seamless scaling**: When demand increases, additional application instances can be deployed quickly and at the service level.
    - **Dynamic resource scaling**: The number of nodes can be adjusted up or down without affecting the application's availability.
    - All of these operations can be managed easily using declarative configuration files.
* **Container orchestration automates the operational tasks required to run containers.**
	  - Scaling containerized applications up or down
	  - Managing resource allocation (e.g., CPU, memory)
	  - Handling rolling updates and deployments
	  - Monitoring application health and performance
	  - Providing high availability and fault tolerance.
	  - Orchestration tools help ensure that containers are running smoothly, efficiently, and in a way that aligns with the needs of the application.

### Key features of container orchestration

1. **Provisioning and Deployment**: Simplifies the setup and deployment of containers.
2. **Self-Healing and Scheduling**: Ensures container availability and optimal use of compute resources.
3. **Service Exposure**: Makes containers network-accessible.
4. **Authorization and Security**: Implements secure access and operations.
5. **Storage Management**: Supports shared and persistent storage for workloads.
6. **Auto-Scaling**: Adjusts resources based on demand.
7. **Extended Functionality**: Custom Resource Definitions (CRDs) in Kubernetes allow expanding beyond core functionalities (e.g., a MySQL CRD for managing MySQL instances).

### Advantages of container orchestration

- Standardizes the deployment process and integrates with various components like networking, storage, security, and autoscaling.
- This enables developers to focus on writing code, rather than worrying about the underlying infrastructure.
- Orchestrators available are: Nomad, Openshift, Docker, and Kubernetes.
- Openshift provide additional functionality and a support model to Kubernetes.
- Kubernetes is widely regarded as the gold standard for Container Orchestration due to its robust feature set, large community support, and widespread adoption across industries and organizations. Originally developed by Google and later open-sourced, Kubernetes has become the de facto standard for container orchestration and management.
- CRDs (Custom Resource Definitions) are a means of expanding Kubernetes to have functionality outside of its core features.
- CRDs allow developers to define new resources and APIs that can be used to extend the Kubernetes platform.
- By creating custom resources, developers can add new features and functionality to their clusters without having to modify the underlying Kubernetes codebase.

## Container Registry

A container registry is indeed a service that hosts and distributes container images. Registries provide a centralized location where images can be stored, shared, and accessed by multiple users or systems. They typically offer features such as image storage, versioning, tagging, and access control.

## Container Runtimes

- Using container runtimes that support CRI provides better compatibility and standardization across container runtimes.
- **Validating Docker Installation**:
  - Check Docker version and configuration using `docker version`.
    - `docker version` provides detailed information about the Docker client and server versions, along with their configuration details.
    - Docker uses a client-server architecture, typically with both components running locally on Docker Desktop.
      - ![[Containers with Dockers Docker version.png]]
      - Docker is using `containerd`
        - It is a high-level abstraction constructed on the top of `runc`, and manage the lifecycle of the containers.
		- Provides image management, networking, and storage.
		- Uses internally `runc`
        - `containerd`was donated to CNCF and now is a graduated project.
      - Docker is using `runc`
        - `runc` is a container runtime designed to create, run, and manage container processes.
		- Used by higher-level container runtimes (e.g., `containerd`, `cri-o`)
        - Docker donated `runc` to OCI
	- Commonly user Container Runtime Interface (CRI) in K8s 
		- **CRI-O** – A lightweight container runtime optimized for Kubernetes (you are using this one).
		- **containerd** – A core component of Docker, widely used in Kubernetes.
		- **Docker (via `cri-dockerd`)** – Docker support was removed from Kubernetes v1.24, but can still be used through `cri-dockerd`.
- **Container Runtime Basics**:
	- Docker leverages containerd (donated to CNCF) and `runc` (donated to OCI) as foundational technologies.
- **Using the Funbox Image**:
	- Pull the image using `docker pull wernight/funbox`.
	- Run containers with commands like `docker run -it --rm wernight/funbox`, exploring options such as `-i` (interactive), `-t` (terminal), and `--rm` (auto-remove on exit).
	- Override the default command (`/menu.sh`) to explore specific features like `Nyan Cat` or `ASCII Aquarium`.
	    - `docker run -it --rm wernight/funbox nyancat`
- **Container States and Management**:
	- View running containers using `docker ps`. Add `-a` to see all containers, including exited ones.
	- Remove containers with `docker rm <container_id/name>`.
	- Run containers without `--rm` to keep them for later interaction or reuse.
- **Best Practices**:
  - ![[Containers with Dockers docker inspect.png]]
  - Run containers as non-root users for security.
    - In this example, the user is john is specified in one of the layer
      - Running a container as a non-root user reduces the attack surface and prevents malicious code from gaining elevated privileges.
  - Use `--rm` for transient containers to ensure they are cleaned up automatically.
  - Use `bash` to access inside the container
    - `docker run -it --rm wernight/funbox bash`

### Container Networking Service and Volumes

- **Publishing Ports**:
  - **`-P` (uppercase)**: **Publishes all exposed ports** from the container image to random host ports. In this case, the container is exposed in all IP addresses on port `55000`.

```bash
docker run -d --rm -P nginx
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                   NAMES

e241e9c6325d   nginx     "/docker-entrypoint.…"   39 seconds ago   Up 38 seconds   0.0.0.0:55000->80/tcp   sweet_haibt
```

- **`-p` (lowercase)**: **Maps specific host ports** to container ports (e.g., `-p 12345:80` maps host port `12345` on the host to container port `80`).

- **`-d` (detached mode)**: Detaches the container from the terminal, allowing it to run in the background.

```bash
docker run -d --rm -p 12345:80 nginx
```

- **Exploring a Running Container**:
  - `docker exec` enables you to execute a process inside the container.
  - Use `docker exec -it <container-id> bash` to access a running container and inspect or modify its contents.
    - Example: Locate and edit the `index.html` file (`find / -name index.html`), then update it (e.g., `echo Hello > index.html`).
- **Persistent Changes with Volumes**:
  - Modify the Nginx content by mounting a host directory as a volume:
    - Create a directory on the host and add content (`echo "Hello from James" > index.html`).
    - Mount the directory using `-v /host/path:/container/path` in the `docker run` command.

```bash
mkdir my_web_page
echo "hello" > my_web_page/index.html
docker run -d --rm -p 12345:80 -v ./my_web_page/:/usr/share/nginx/html nginx
```

- Changes to the host file (e.g., appending text) are immediately reflected in the container.

### containerd

- K8s introduced dockershim as a compatibility layer between k8s and docker
	- Kubernetes v1.24 **removed `dockershim`** because maintaining it added complexity.
	- Allow docker to communicate with k8s without using the CRI
	- Docker itself was using `containerd` internally, so Kubernetes could directly interact with `containerd`, eliminating the need for `dockershim`.
- `ctr` is bundled with containerd and should be used for debugging containerd
	- not user friendly
	- `ctr images pull docker.io/library/redis:alpine`
	- `ctr run docker.io/library/redis:alpine redis`
- `nerdctl` provides a docker-like cli and supports docker compose
- `nerdctl` supports
	- encrypted container images
	- lazy pulling
	- p2p image distribution
	- image signing and verifying
	- namespaces 
- `crictl` is used to interact with CRI compatible runtimes
	- It is necessary to set the endpoint to connect to CRI
	```bash
	crictl --runtime-endpoint
	export CONTAINER_RUNTIME_ENDPOINT
	```
	

| Feature                    | `crictl` 🛠️                                                                        | `nerdctl` 🐳                                                                             |
| -------------------------- | ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| **Purpose**                | Designed to interact with CRI-compatible runtimes (like `CRI-O` and `containerd`).  | A Docker-compatible CLI for `containerd`, offering a similar experience to `docker` CLI. |
| **API Used**               | Uses the **Container Runtime Interface (CRI)**.                                     | Uses the **containerd API** directly.                                                    |
| **Usage**                  | Primarily used in Kubernetes environments to debug and manage CRI-based containers. | Useful as a lightweight Docker alternative when using `containerd` without Docker.       |
| **Supported Runtimes**     | Works with CRI-compatible runtimes like `CRI-O` and `containerd`.                   | Works only with `containerd`.                                                            |
| **Commands**               | Similar to `kubectl` for containers (e.g., `crictl ps`, `crictl images`).           | Similar to `docker` CLI (e.g., `nerdctl run`, `nerdctl build`).                          |
| **Kubernetes Integration** | Yes, commonly used for debugging Kubernetes pods and containers.                    | No direct Kubernetes integration; more focused on standalone containerd usage.           |
| **Purpose**                | Debugging Purposes                                                                  | General purpose                                                                          |