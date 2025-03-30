# Flashcards

## Evolution of Cloud-Native Architecture

- **What are the origins of Cloud-Native Architecture?**  
?
  Evolved from challenges with legacy monolithic applications.  
  #flashcard

- **What is the focus of Cloud-Native Architecture?**  
?
  Improves availability, cost management, efficiency, and reliability through design patterns.  
  #flashcard

- **What are the benefits of Cloud-Native Architecture?**  
?
  Enables loosely coupled, resilient, manageable, observable systems with robust automation for high-impact changes.  
  #flashcard

---

## Monolithic Applications: Issues and Challenges

- **What is a key issue with tightly coupled components in monolithic apps?**  
?
  Changes in one area can affect the entire app, and library updates can break compatibility.  
  #flashcard

- **How were monolithic apps traditionally installed?**  
?
  Directly on the OS using package managers like APT or YUM, sharing resources and increasing complexity.  
  #flashcard

- **Why is the architecture of monolithic apps fragile?**  
?
  Tight coupling between web server, business logic, and data interfaces makes frequent changes difficult.  
  #flashcard

---

## Cloud-Native Approach: Advantages

- **What is the Microservices Architecture in Cloud-Native?**  
?
  Breaks apps into smaller and standalone units for easier management, deployment, and independent updates.  
  #flashcard

- How does encapsulation benefit Cloud-Native apps?  
?
  Each microservice includes its dependencies, isolating resources and reducing conflicts (e.g., /main vs /store).  
  #flashcard

- How does Cloud-Native improve networking?  
?
  Isolated networking for microservices avoids port conflicts and simplifies maintenance.  
  #flashcard

- **What flexibility does the data layer gain in Cloud-Native?**  
?
  Microservices can independently interact with databases or use replication, sharding, or distributed stores (e.g., etcd).  
  #flashcard

---

## Key Benefits of Cloud-Native Design

- **What does Cloud-Native Architecture involve?**  
?
  Designing apps with microservices, containerization, and CI/CD for scalability, resilience, and flexibility across cloud environments.  
  #flashcard

- **How does Cloud-Native ensure high availability?**  
?
  Builds systems with redundancy, self-healing, and scalability to handle component failures.  
  #flashcard

- **What practical benefits does Cloud-Native offer?**  
?
  Easier maintenance, deployment, upgrades, and frequent, predictable changes with modularity.  
  #flashcard

---

## Characteristics of Cloud-Native Applications

- **How does resiliency work in Cloud-Native apps?**  
?
  Designed to withstand failures with redundancy, failover, and self-healing (e.g., Kubernetes replaces failed pods).  
  #flashcard

- **What does agility mean in Cloud-Native?**  
?
  Rapid development, modification, and deployment using microservices, CI/CD, and automation.  
  #flashcard

- **What is operability in Cloud-Native apps?**  
?
  Focus on easy deployment and management with automation and IaC (e.g., Terraform).  
  #flashcard

- **What is observability in Cloud-Native?**  
?
  Insight into system state via logs, metrics, and traces for diagnosing issues and performance.  
  #flashcard

- **What’s a mnemonic for Cloud-Native characteristics?**  
?
  RAOO - "Racoons Are Often Observant" (Resiliency, Agility, Operability, Observability).  
  #flashcard

---

## Key Cloud-Native Practices

- **How does resilience manifest in Cloud-Native?**  
?
  Apps anticipate failures with self-healing (e.g., Kubernetes restarts failed pods in a replica set).  
  #flashcard

- **What role does automation play in Cloud-Native?**  
?
  Reduces manual effort with tools like Ansible and Terraform for rapid, consistent deployments.  
  #flashcard

- **What’s the difference between CI, CDelivery, and CDeployment?**  
?
  CI: frequent commits with testing. CDelivery: automated release prep. CDeployment: fully automated production release.  
  #flashcard

- **What does "Secure by Default" mean?**  
?
  Systems are designed with security from the start (e.g., Zero Trust, least privilege access).  
  #flashcard

- **How does Cloud-Native optimize speed, efficiency, and cost?**  
?
  Uses auto-scaling, serverless architectures, and proactive design for dynamic workloads.  
  #flashcard

- **What is service discovery in Cloud-Native?**  
?
  Automates service detection with tools like Kubernetes DNS and environment variables.  
  #flashcard

---

## Key Pillars of Cloud-Native Architecture

- **What are the four pillars of Cloud-Native Architecture?**  
?
  Microservices, Containerisation, DevOps, Continuous Delivery.  
  #flashcard

- **How does Containerisation benefit Cloud-Native?**  
?
  Encapsulates apps and dependencies for consistency, isolation, and easy deployment.  
  #flashcard

- **What is DevOps in Cloud-Native?**  
?
  Combines Dev and Ops for automation, monitoring, and collaboration.  
  #flashcard

- **What’s a mnemonic for the Cloud-Native pillars?**  
?
  "Morning Coffee Delivers Caffeine Delight" (Microservices, Containerisation, DevOps, CD).  
  #flashcard

---

## Autoscaling

- **What is Auto Scaling?**  
?
  Automatically adjusts resources based on workload demand for performance and cost efficiency.  
  #flashcard

- **What are the three types of Auto Scaling?**  
?
  Reactive (threshold-based), Scheduled (predictable spikes), Predictive (AI-based on historical data).  
  #flashcard

- **What’s the difference between Vertical and Horizontal Scaling?**  
?
  Vertical: increases resources on a server. Horizontal: adds/removes instances (preferred in Cloud-Native).  
  #flashcard

- **What are Kubernetes’ autoscaling tools?**
?
  Cluster Autoscaler (nodes), HPA (pod replicas), VPA (pod resources), KEDA (event-driven).
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **What’s a key consideration for autoscaling?**  
?
  Testing ensures performance under load; horizontal scaling increases data-sharing complexity.  
  #flashcard

---

## Serverless Computing

- **What is Serverless Computing?**
?
  Providers manage servers; users deploy code that runs in response to events, scaling automatically.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **What are the core features of Serverless?**  
?
  Event-driven, auto-scaling (to zero), abstracted infrastructure management.  
  #flashcard

- **What affects costs in Serverless?**  
?
  Code execution (executions and duration) and autoscaling (resources used).  
  #flashcard

- **What is AWS Lambda?**  
?
  A FaaS platform that executes code in response to events, auto-scaling with concurrency management.  
  #flashcard

- **What’s a challenge in Serverless?**  
?
  Vendor lock-in due to proprietary APIs; cold starts cause latency after inactivity.  
  #flashcard

- **What are open-source Serverless options for Kubernetes?**  
?
  Knative and OpenFaaS provide FaaS with scaling from zero.  
  #flashcard

---

## Community and Governance

- **What is the CNCF?**  
?
  A vendor-neutral org under Linux Foundation, hosting cloud-native projects like Kubernetes.  
  #flashcard

- **What are CNCF’s project maturity levels?**  
?
  Sandbox (early-stage), Incubating (adoption), Graduated (mature, strict criteria).  
  #flashcard

- **What does the Technical Oversight Committee (TOC) do?**  
?
  Evaluates project maturity based on adoption, engagement, and quality.  
  #flashcard

- **What’s the role of SIGs and TAGs in CNCF?**  
?
  SIGs focus on project areas (e.g., Kubernetes security); TAGs provide domain guidance (e.g., storage).  
  #flashcard

---

## Cloud-Native Personas

- **What does a DevOps Engineer focus on?**  
?
  Bridges Dev and Ops with automation, scripting, and cloud-native practices.  
  #flashcard

- **What’s the role of a Site Reliability Engineer (SRE)?**  
?
  Ensures reliability at scale with SLAs, SLOs, and incident management.  
  #flashcard

- **What does a Cloud Architect do?**  
?
  Designs cloud solutions, optimizing cost, performance, and interoperability.  
  #flashcard

- **What’s unique about a FinOps Engineer?**  
?
  Balances cloud spending with speed and quality, collaborating across IT and finance.  
  #flashcard

---

## Open Standards

- **What are Open Standards?**  
?
  Freely adoptable frameworks fostering collaboration and interoperability, avoiding vendor lock-in.  
  #flashcard

- **What is the Open Container Initiative (OCI)?**  
?
  Established in 2015, defines container image and runtime specs (e.g., runc).  
  #flashcard

- **What does the Container Network Interface (CNI) do?**  
?
  Simplifies networking setup for Kubernetes with plugin compatibility.  
  #flashcard

- **What’s the purpose of the Container Storage Interface (CSI)?**  
?
  Standardizes storage integration for container orchestration (e.g., Rook).  
  #flashcard

---

## What is Cloud Native?

- **What does "Cloud Native" mean?**  
?
  Combines "cloud" (public, private, hybrid environments) and "native" (designed for scalability, resilience, manageability). It’s a set of philosophies, not a binary state.  
  #flashcard

- **What are the two main philosophies CNCF?**  
?
  1. Cloud Native Architecture (best practices for diverse cloud environments). 2. Cloud Native Culture (organizational changes for design/implementation).  
  #flashcard

---

## Benefits of Cloud Native

- **What are the key benefits of Cloud Native practices?**  
?
  Effective use of cloud infrastructure, resilience, scalability, automation, security, and progression beyond basic cloud hosting.  
  #flashcard

- **How does Infrastructure-as-Code (IaC) relate to Cloud Native?**  
?
  Tools like Terraform enable vendor-agnostic management, aligning with Cloud Native principles.  
  #flashcard

---

## Misconceptions About Cloud Native

- **Does running an app in the cloud make it Cloud Native?**  
?
  No, it requires best practices, not just cloud hosting.  
  #flashcard

- **Are containers enough to make an app Cloud Native?**  
?
  No, they’re a step toward it, but best practices must be followed.  
  #flashcard

---

## Key Practices in Cloud Native Development

- **What are the four key practices in Cloud Native development?**  
?
  1. Automation (setup/delivery). 2. Resilience (handle failures). 3. Scalability (adapt to demand). 4. Security by Default (robust measures).  
  #flashcard

---

## Challenges Addressed by Cloud Native

- **What challenges does Cloud Native solve?**  
?
  Single system errors, host resource limitations, and "noisy neighbors" in shared environments.  
  #flashcard

---

## Ecosystem and Governance

- **What is the Linux Foundation’s role in Cloud Native?**  
?
  Founded in 2000, it standardizes Linux, sponsors projects like Kubernetes, and drives the open cloud movement.  
  #flashcard

- **What is the Cloud Native Computing Foundation (CNCF)?**  
?
  Founded in 2015, it promotes cloud native tech, oversees projects like Kubernetes, and shapes the ecosystem.  
  #flashcard

- **When was Kubernetes’ first major milestone?**  
?
  February 2015, with release 1.0.  
  #flashcard

---

## KCNA Exam Overview

- **What is the KCNA certification?**  
?
  An entry-level cert for Kubernetes and cloud-native ecosystems, a gateway to CKA and CKAD.  
  #flashcard

- **What advanced certifications follow KCNA?**  
?
  CKA (Certified Kubernetes Administrator) and CKAD (Certified Kubernetes Application Developer).  
  #flashcard

---

## Highlights of KCNA

- **What topics does KCNA cover?**  
?
  Kubernetes, security (leads to CKS), telemetry/observability (Prometheus), and more.  
  #flashcard

- **Why is KCNA a strong foundation?**  
?
  It prepares you for advanced certs with practical, career-aligned knowledge.  
  #flashcard

---

## KCNA Exam Details

- **What is the KCNA exam format?**  
?
  Multiple-choice, entry-level, theoretical.  
  #flashcard

- **How can you take the KCNA exam?**  
?
  In-person at test centers or remotely with proctor monitoring.  
  #flashcard

---

## Why Choose KCNA?

- **Why is KCNA a good choice?**  
?
  Diverse curriculum, prepares for advanced certs, practical for cloud-native roles, and remote-friendly.  
  #flashcard

- **Where can you check KCNA curriculum updates?**  
?
  <https://github.com/cncf/curriculum> (includes GitOps, Prometheus
  #flashcard

---

## Pro Tips for KCNA

- **How can you stay updated on KCNA exam news?**  
?
  Follow Linux Foundation and CNCF on social media for sales, events, and updates.  
  #flashcard

- **Where do you book the KCNA exam?**  
?
  [Kubernetes Cloud Native Associate (KCNA)](https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate). Use discount code: DIVEINTO30.  
  #flashcard

- **How can you reduce KCNA exam costs?**
?
  Wait for sales (up to 40-45% off), attend KubeCon/CloudNativeCon, or check social media for codes.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **What should you focus on for the KCNA exam?**  
?
  Infrastructure as Code (IaC), Speed, Efficiency, and Cost in Cloud Native contexts.  
  #flashcard

- **What’s the official source for KCNA exam info?**  
?
  The GitHub repo cncf/curriculum, updated by CNCF.  
  #flashcard

---

## Introduction to Containers

- **How did containers transform technology?**  
?
  Revolutionized infrastructure, app design, development, and operations, enabling efficient management of thousands of containers with tools like Kubernetes.  
  #flashcard

- **What’s the historical foundation of mainframe virtualization (1960s)?**  
?
  IBM CP/CMS allowed multiple users to run virtualized OS instances on a single mainframe, marking the start of VM architectures.  
  #flashcard

- **What is Chroot (1979) and its limitations?**  
?
  Unix feature isolating processes by changing the root directory; limited by shared resources (e.g., hostname, IP) and inability to run superuser processes.  
  #flashcard

- **What are FreeBSD Jails (2000)?**  
?
  Lightweight virtualization isolating system resources in FreeBSD, similar to Docker but complex and less adopted.  
  #flashcard

- **What are Solaris Zones and HP-UX Virtual Partitions (2000s)?**  
?
  Virtualization tech isolating compute resources; Solaris Zones divided OS, HP-UX segmented systems logically.  
  #flashcard

- **What are the modern ingredients Docker combined?**  
?
  Linux Namespaces (process isolation) and cgroups (resource allocation) for lightweight containerization.  
  #flashcard

- **What are Linux Namespaces (2002)?**  
?
  Isolate processes, networks, mounts, hostnames, etc., with types like PID, NET, MNT, UTS, IPC, USER.  
  #flashcard

- **What are Control Groups (cgroups, 2006)?**  
?
  Limit, prioritize, account, and control resources; developed by Google, merged into Linux in 2008.  
  #flashcard

- **How do Virtual Machines compare to containers?**  
?
  VMs (e.g., VMware ESXi) use hypervisors for isolation but have OS overhead and resource segmentation; containers share the host kernel.  
  #flashcard

- **What made Docker a game-changer in 2013?**  
?
  Combined namespaces and cgroups into a user-friendly platform, simplifying scalable containerization.  
  #flashcard

- **Why are containers preferred over VMs?**  
?
  Lightweight, share host OS kernel, reducing overhead and maximizing resource use.  
  #flashcard

---

## Docker Installation

- **What’s the traditional Docker architecture?**  
?
  Hardware → OS → Docker runtime (containerd and runc); requires Linux kernel 3.1+.  
  #flashcard

- **What does Docker Desktop provide?**  
?
  UI for Mac, Windows, Linux; runs a hidden VM/subsystem for Docker runtime, with CLI, Kubernetes, and Extensions.  
  #flashcard

- **What’s a key benefit of Docker Desktop?**  
?
  Flexible, self-contained environment with easy reset for development and testing.  
  #flashcard

- **How do you run a container with Docker Desktop?**  
?
  `docker run -it ubuntu bash` opens an interactive Ubuntu terminal.  
  #flashcard

- **How do you enable Kubernetes in Docker Desktop?**  
?
  Turn it on in preferences to use `kubectl` commands.  
  #flashcard

---

## Container Images

- **What is a container image?**
?
  A portable, OCI-compliant bundle of software and dependencies for consistent execution across environments.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **What’s the difference between a container image and a container?**  
?
  Image is the blueprint; container is a running instance (e.g., multiple nginx containers from one image).  
  #flashcard

- **What are tags in container images?**  
?
  Labels for versions/variants (e.g., latest, v1.0); `latest` is a default, not always the newest.  
  #flashcard

- **How are container images structured?**  
?
  Built as layers; each step in the build process creates a shared, reusable layer, topped with a writable layer.  
  #flashcard

- **What are drawbacks of too many image layers?**  
?
  Increased build time, larger size, slower caching, complexity, and filesystem limits.  
  #flashcard

- **What’s a Union File System in Docker?**  
?
  Merges image layers into a single view; changes are stored in a writable layer, optimizing space.  
  #flashcard

- **What’s the difference between a digest and an image ID?**  
?
  Digest (SHA-256) identifies an image in a registry; Image ID is a local hash of the JSON config.  
  #flashcard

- **How do you save a container image?**  
?
  `docker save` exports it locally, including layers and JSON metadata.  
  #flashcard

---

## Container Registry

- **What is a container registry?**  
?
  A service hosting and distributing container images with storage, versioning, and access control.  
  #flashcard

---

## Running Containers

- **How do you validate a Docker installation?**
?
  `docker version` shows client/server details; uses containerd and runc.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **What’s the Funbox image used for?**  
?
  `docker pull wernight/funbox` and `docker run -it --rm wernight/funbox` to explore features like Nyan Cat.  
  #flashcard

- **What do `-i`, `-t`, and `--rm` do in Docker run?**  
?
  `-i` keeps input open, `-t` allocates a terminal, `--rm` auto-removes the container on exit.  
  #flashcard

- **How do you view container states?**  
?
  `docker ps` for running containers; `docker ps -a` for all, including exited.  
  #flashcard

- **What’s a security best practice for running containers?**  
?
  Run as non-root users to reduce attack surface.  
  #flashcard

---

## Container Networking Service and Volumes

- **What’s the difference between `-P` and `-p` in Docker run?**  
?
  `-P` publishes all exposed ports to random host ports; `-p` maps specific host-to-container ports (e.g., `-p 12345:80`).  
  #flashcard

- **What does `-d` do in Docker run?**  
?
  Detaches the container to run in the background.  
  #flashcard

- **How do you explore a running container?**  
?
  `docker exec -it <container-id> bash` opens a terminal inside it.  
  #flashcard

- **How do you make persistent changes to a container?**
?
  Use volumes with `-v /host/path:/container/path` to mount a host directory (e.g., for Nginx content).
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

## Container Orchestration

- **What is container orchestration?**  
?
  Automates tasks like scaling, resource allocation, updates, monitoring, and ensuring high availability for containers.  
  #flashcard

- **What are key features of container orchestration?**  
?
  Provisioning, self-healing, service exposure, security, storage management, auto-scaling, and extended functionality (e.g., CRDs).  
  #flashcard

- **How does orchestration benefit complex app deployment?**  
?
  Standardizes deployment, integrates networking/storage/security, letting developers focus on code.  
  #flashcard

- **What are some container orchestrators?**  
?
  Nomad, OpenShift, Docker, Kubernetes (gold standard due to features and adoption).  
  #flashcard

- **What are CRDs in Kubernetes?**  
?
  Custom Resource Definitions extend Kubernetes beyond core features (e.g., MySQL management).  
  #flashcard

---

## Kubernetes Architecture

- **What are the two main areas of Kubernetes architecture?**  
?
  Control Plane (manages cluster) and Nodes (run workloads).  
  #flashcard

- **What’s the role of the container runtime?**  
?
  Low-level (e.g., runc) interacts with namespaces/cgroups; high-level (e.g., containerd) manages container lifecycle.  
  #flashcard

- **What does Kubelet do?**  
?
  Runs on all nodes, ensures pods are healthy, reads specs from API or static YAML files.  
  #flashcard

- **What is etcd?**  
?
  Distributed key-value store for cluster data, uses Raft consensus, requires backups.  
  #flashcard

- **What’s the Kube API Server?**  
?
  Main cluster access point, exposes RESTful API, stores data in etcd, runs as a static pod.  
  #flashcard

- **What does the Kube Scheduler do?**  
?
  Assigns pods to nodes based on resources and constraints, runs as a static pod.  
  #flashcard

- **What’s Kube Proxy’s role?**  
?
  Manages network connectivity (TCP/UDP/SCTP) on every node as a DaemonSet.  
  #flashcard

- **What is CoreDNS?**
?
  Provides cluster DNS resolution, runs as a Deployment.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **What does the Controller Manager handle?**  
?
  Runs control loops (e.g., Node and Deployment controllers) to enforce desired state.  
  #flashcard

- **What’s the Cloud Controller Manager (CCM)?**  
?
  Bridges Kubernetes with cloud providers for load balancers and routes.  
  #flashcard

- **How does a High Availability (HA) cluster work?**  
?
  Multiple control planes, clustered etcd with Raft, load-balanced API Server.  
  #flashcard

---

## Pod Basics

- **What is a Pod in Kubernetes?**  
?
  Smallest deployable unit, contains one or more containers sharing network/storage.  
  #flashcard

- **How do containers in a pod communicate?**  
?
  Via localhost; each pod gets a unique cluster IP.  
  #flashcard

- **How do you create an nginx pod?**  
?
  `kubectl run nginx --image=nginx`.  
  #flashcard

- **How do you access a pod externally?**  
?
  Use port forwarding: `kubectl port-forward nginx 8080:80`.  
  #flashcard

- **How do you test inter-pod communication?**  
?
  Run a curl pod: `kubectl run curl --image=curlimages/curl --rm -it --restart=Never -- curl <nginx-pod-IP>`.  
  #flashcard

---

## Using YAML to Create Pods

- **What’s the difference between `kubectl create` and `apply`?**  
?
  `create` fails if resource exists; `apply` updates existing resources.  
  #flashcard

- **How do you generate YAML for a pod?**  
?
  `kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx.yaml`.  
  #flashcard

- **What are the restartPolicy options?**  
?
  Always (default), Never, OnFailure.  
  #flashcard

- **How do you combine multiple YAMLs into one file?**  
?
  Use `---` separator: `{ cat nginx.yaml; echo "---"; cat ubuntu.yaml; } > combined.yaml`.  
  #flashcard

- **What’s a sidecar container?**  
?
  A secondary container in a pod enhancing the main container’s functionality.  
  #flashcard

---

## Namespaces

- **What are Kubernetes namespaces?**  
?
  Divide cluster resources for isolation (e.g., users, apps, projects).  
  #flashcard

- **Why use namespaces?**  
?
  Multi-tenancy, resource quotas, limit ranges, RBAC, organization.  
  #flashcard

- **What are common default namespaces?**  
?
  kube-system, kube-public, kube-node-lease.  
  #flashcard

- **How do you create a namespace?**  
?
  `kubectl create namespace my-namespace`.  
  #flashcard

- **How do you set a default namespace?**  
?
  `kubectl config set-context --current --namespace=my-namespace`.  
  #flashcard

---

## Deployments and ReplicaSets

- **What is a Kubernetes Deployment?**  
?
  Manages app updates declaratively with replication, rolling updates, and rollbacks.  
  #flashcard

- **How do you scale a deployment?**  
?
  `kubectl scale deployment/nginx --replicas=10`.  
  #flashcard

- **How do you update a deployment?**  
?
  `kubectl set image deployment/nginx nginx=nginx:stable`.  
  #flashcard

- **How do you rollback a deployment?**  
?
  `kubectl rollout undo deployment/nginx`.  
  #flashcard

- **What manages pod lifecycles in a deployment?**  
?
  ReplicaSets ensure the desired number of pods are running.  
  #flashcard

---

## Services

- **What are the four main Kubernetes service types?**  
?
  ClusterIP (default), NodePort, LoadBalancer, ExternalName.  
  #flashcard

- **What’s a ClusterIP service?**  
?
  Internal-only IP for cluster communication (e.g., nginx.default.svc.cluster.local).  
  #flashcard

- **What’s a NodePort service?**  
?
  Exposes a static port on each node (30000-32767) for external access.  
  #flashcard

- **What’s a LoadBalancer service?**  
?
  Uses a cloud provider’s load balancer for external access.  
  #flashcard

- **What’s a Headless Service?**  
?
  No cluster IP, resolves directly to pod IPs (clusterIP: None).  
  #flashcard

---

## Kubernetes Jobs

- **What is a Kubernetes Job?**  
?
  Manages batch tasks, runs pods to completion, retries on failure.  
  #flashcard

- **What’s an example of a Job use case?**  
?
  Calculating Pi: `perl -Mbignum=bpi -wle "print bpi(2000)"`.  
  #flashcard

- **What do `completions` and `parallelism` control?**  
?
  `completions`: total pods to run; `parallelism`: concurrent pods.  
  #flashcard

---

## Kubernetes CronJobs

- **What is a Kubernetes CronJob?**  
?
  Schedules Jobs at intervals (e.g., `* * * * *` for every minute).  
  #flashcard

- **What happens when a CronJob is deleted?**  
?
  Stops scheduling, but existing Jobs/pods persist unless manually deleted.  
  #flashcard

- **What’s the default job history retention?**  
?
  3 successful, 1 failed (successfulJobsHistoryLimit, failedJobsHistoryLimit).  
  #flashcard

---

## ConfigMaps

- **What are ConfigMaps?**  
?
  Store non-sensitive configuration data (e.g., env vars) for pods.  
  #flashcard

- **How do you create a ConfigMap?**  
?
  `kubectl create configmap color-configmap --from-literal=color=red`.  
  #flashcard

- **How do you use a ConfigMap in a pod?**  
?
  Add `envFrom: - configMapRef: name: color-configmap` in YAML.  
  #flashcard

- **What’s an immutable ConfigMap?**  
?
  Can’t be modified after creation (immutable: true, Kubernetes 1.21+).  
  #flashcard

---

## Secrets in Kubernetes

- **What are Kubernetes Secrets?**  
?
  Store sensitive data (e.g., passwords, keys) encoded in Base64.  
  #flashcard

- **How do Secrets differ from ConfigMaps?**  
?
  Secrets are for sensitive data; ConfigMaps are for non-sensitive config.  
  #flashcard

- **How do you create a Secret?**
?
  `kubectl create secret generic color-secret --from-literal=color=red`.
  #flashcard
<!--SR:!2025-03-29,3,250-->

- **Are Secrets encrypted?**
?
  No, only Base64-encoded; stored in etcd, requiring secure access.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

## Labels in Kubernetes

- **What are Kubernetes Labels?**  
?
  Key-value pairs to tag and organize resources for grouping/filtering.  
  #flashcard

- **Why use Labels?**  
?
  Organize resources, automate CI/CD, enable service discovery, define network policies.  
  #flashcard

- **How do you create a pod with labels?**  
?
  `kubectl run nginx --image=nginx --labels="app=web,env=prod"`.  
  #flashcard

- **How do you filter by labels?**  
?
  `kubectl get pods --selector app=web` or `-l env=prod`.  
  #flashcard

---

## Kubernetes API

- **What interacts with the Kubernetes API?**  
?
  Users/tools (kubectl, Helm), monitoring tools, internal components (scheduler, kubelet, controller manager).  
  #flashcard

- **What’s the API request processing flow?**  
?
  Request Arrival → Route Matching → Authentication → Authorization → Admission Controller → Validation → Handling → Response.  
  #flashcard

- **What does the Admission Controller do in the API flow?**  
?
  Enforces policies (e.g., quotas) and modifies resources before acceptance.  
  #flashcard

- **What are Custom Resource Definitions (CRDs)?**  
?
  Extend the API with new resource types (e.g., MySQL management).  
  #flashcard

- **How does `kubectl proxy` simplify API access?**  
?
  Allows local HTTP access (localhost:8001) without manual authentication.  
  #flashcard

- **What’s the OpenAPI specification in Kubernetes?**  
?
  Documents API endpoints, parameters, and responses (e.g., via localhost:8001/openapi/v2).  
  #flashcard

---

## Role-Based Access Control (RBAC)

- **What is RBAC in Kubernetes?**
?
  Manages access to resources using roles assigned to users/groups for fine-grained control.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **What’s in a kubeconfig file?**  
?
  Cluster details (API server URL, CA data) and user info (username, certificates).  
  #flashcard

- **How does Kubernetes authenticate users?**  
?
  Via external certificates (CN=User, O=Group) signed by a CA, not managed internally.  
  #flashcard

- **What’s a ClusterRole vs. ClusterRoleBinding?**  
?
  ClusterRole defines cluster-wide permissions; ClusterRoleBinding assigns them to users/groups.  
  #flashcard

- **How do you check permissions with kubectl?**  
?
  `kubectl auth can-i <verb> <resource>` (e.g., `kubectl auth can-i '*' '*'`).  
  #flashcard

- **How do you create a superuser group?**  
?
  Create ClusterRole (`--verb='*' --resource='*'`) and bind it to a group with ClusterRoleBinding.  
  #flashcard

---

## Scheduling Process

- **What does the kube-scheduler do?**  
?
  Assigns pods to nodes based on resources, affinity, taints, and tolerations.  
  #flashcard

- **What are the three steps in scheduling?**  
?
  Filtering (remove unfit nodes), Scoring (rank nodes), Binding (assign pod to node).  
  #flashcard

- **How do you bypass the scheduler?**  
?
  Set `nodeName` in the pod spec (e.g., `nodeName: worker-2`).  
  #flashcard

- **What’s a custom scheduler?**  
?
  Specified via `schedulerName` in pod spec; can use custom logic (e.g., random node selection).  
  #flashcard

- **How do NodeSelectors work?**  
?
  Use labels (e.g., `kubernetes.io/hostname: worker-1`) for targeted pod placement.  
  #flashcard

---

## Storage

- **What is ephemeral storage in Kubernetes?**  
?
  Temporary storage (e.g., emptyDir) that doesn’t persist across pod restarts.  
  #flashcard

- **What’s an emptyDir volume?**  
?
  Created empty when a pod starts; data persists across container crashes but not pod deletion.  
  #flashcard

- **What’s persistent storage in Kubernetes?**  
?
  Storage that persists beyond pod lifecycle (e.g., PVs, PVCs).  
  #flashcard

- **What are Storage Classes, PVs, and PVCs?**  
?
  Storage Classes define storage types; PVs are provisioned storage; PVCs are user requests for storage.  
  #flashcard

- **What’s dynamic provisioning?**  
?
  PVC against a StorageClass auto-creates a PV (vs. manual PV/PVC creation).  
  #flashcard

---

## StatefulSets

- **What are StatefulSets used for?**  
?
  Manage stateful apps (e.g., databases) with stable identities and persistent storage.  
  #flashcard

- **How do StatefulSets differ from Deployments?**  
?
  StatefulSets provide unique pod identities and individual PVCs; Deployments are stateless.  
  #flashcard

- **What’s a headless service in StatefulSets?**  
?
  No cluster IP, provides stable DNS names (e.g., nginx-0.nginx.svc.cluster.local).  
  #flashcard

- **How are StatefulSet pods named?**  
?
  Sequentially with StatefulSet name prefix (e.g., nginx-0, nginx-1).  
  #flashcard

- **What happens to PVCs when a StatefulSet is deleted?**  
?
  PVCs and PVs persist, reusable when recreated.  
  #flashcard

---

## Network Policies

- **What do Network Policies do?**  
?
  Control pod communication by allowing/denying traffic based on labels, ports, etc.  
  #flashcard

- **What’s the role of the CNI plugin in Network Policies?**  
?
  Enforces policies by configuring network rules for traffic control.  
  #flashcard

- **What happens without Network Policies?**  
?
  Pods can send/receive traffic from any source by default.  
  #flashcard

- **How do multiple Network Policies combine?**  
?
  Effects are additive, creating cumulative traffic rules.  
  #flashcard

---

## Pod Disruption Budgets (PDB)

- **What’s the difference between PDB and Replicas?**  
?
  Replicas ensure running pods; PDBs limit disruptions to maintain availability.  
  #flashcard

- **What are voluntary disruptions?**  
?
  Planned actions (e.g., maintenance, upgrades) that may terminate pods.  
  #flashcard

- **How does a PDB work during node drain?**  
?
  Prevents eviction if it would drop below the minimum available pods (e.g., 2).  
  #flashcard

---

## Security

- **What are Security Contexts in Kubernetes?**  
?
  Define access control/privileges for pods/containers (e.g., runAsUser, runAsGroup).  
  #flashcard

- **What does `allowPrivilegeEscalation: false` do?**  
?
  Prevents containers from gaining elevated privileges.  
  #flashcard

- **What replaced Pod Security Policies in Kubernetes 1.25?**  
?
  Admission Controllers enforce security at the cluster level.  
  #flashcard

- **What are Kyverno and OPA Gatekeeper?**
?
  Tools for custom policy enforcement via admission controllers.
  #flashcard
<!--SR:!2025-03-27,1,230-->

- **What are the 4C’s of Cloud Native Security?**
?
  Cloud, Cluster, Container, Code—layers of security from broad to specific.
  #flashcard
<!--SR:!2025-03-29,3,250-->

---

## Helm and Helm Charts

- **What is Helm in Kubernetes?**  
?
  Simplifies app management with Helm Charts (pre-configured resource packages).  
  #flashcard

- **How do you create a Helm Chart?**  
?
  `helm create flappy-app`, customize Chart.yaml and values.yaml.  
  #flashcard

- **How do you deploy a Helm Chart?**  
?
  `helm install flappy-app <chart-file>.tgz`.  
  #flashcard

- **What’s the benefit of Helm Charts?**  
?
  Easy deployment, upgrades, and scaling, like APT/YUM for Kubernetes.  
  #flashcard

---

## Service Meshes

- **What’s the role of a service mesh?**  
?
  Manages secure, reliable microservice communication in complex architectures.  
  #flashcard

- **What are the two parts of a service mesh?**  
?
  Data Plane (proxies for traffic) and Control Plane (manages proxies).  
  #flashcard

- **What’s a sidecar proxy vs. host node proxy?**  
?
  Sidecar attaches to each service; host node runs at the node level.  
  #flashcard

- **What are key benefits of service meshes?**  
?
  Security (mutual TLS), access control, observability, reliability (rate limiting).  
  #flashcard

- **What’s the Service Mesh Interface (SMI)?**  
?
  Unified API standard for managing traffic, access, and metrics across meshes.  
  #flashcard

---

## Observability

- **What is observability in cloud-native systems?**  
?
  Ability to determine system state via telemetry (logs, metrics, traces, alerts).  
  #flashcard

- **What are the three pillars of observability?**  
?
  Logs (event records), Metrics (quantitative measures), Traces (request tracking).  
  #flashcard

- **What do alerts do in observability?**  
?
  Provide early warnings of anomalies or failures for proactive resolution.  
  #flashcard

- **What’s the role of OpenTracing and OpenTelemetry?**  
?
  Operate at the app layer, providing APIs/tools for tracing and telemetry collection.  
  #flashcard

- **How do logs help in observability?**  
?
  Record events (errors, debug info) for troubleshooting and behavior analysis.  
  #flashcard

- **What are the types of metrics?**  
?
  Gauges (point-in-time), Counters (cumulative), Meters (event rates), Histograms (distributions).  
  #flashcard

- **What do traces provide in a distributed system?**  
?
  Visibility into request latency, component traversal, and bottlenecks.  
  #flashcard

---

## Prometheus and Grafana

**Q: What is Prometheus?**  
?
A: Open-source CNCF tool for monitoring/alerting with a multi-dimensional data model.
  #flashcard

**Q: What are key features of Prometheus?**  
?
A: PromQL queries, autonomous nodes, push-pull data collection, built-in alerting.
  #flashcard

**Q: What is Grafana?**  
?
A: Open-source platform for visualizing metrics from sources like Prometheus.
  #flashcard

**Q: How do Prometheus and Grafana enhance Kubernetes observability?**  
?
A: Offer monitoring, visualization, and alerting for uptime/performance goals.
  #flashcard

**Q: What’s the kube-prometheus-stack?**  
?
A: Helm chart simplifying Prometheus/Grafana setup in Kubernetes.
  #flashcard

**Q: What components are installed with kube-prometheus-stack?**  
?
A: Prometheus Operator, Alertmanager, Node Exporter, Kube-State-Metrics, Grafana.
  #flashcard

**Q: How do you access Prometheus UI after installation?**  
?
A: `kubectl port-forward service/<prometheus-service> 9090:9090`.
  #flashcard

**Q: What’s an example PromQL query for pods per namespace?**  
?
A: `sum by (namespace) (kube_pod_ips)`.
  #flashcard

**Q: How do you import dashboards in Grafana?**  
?
A: Use IDs (e.g., 15759 for Node Metrics) and select Prometheus as the data source.
  #flashcard

---

## Cost Management in Cloud-Native Solutions

**Q: What’s a key principle of cloud-native cost management?**  
?
A: Flexibility to allocate resources across public/hybrid/private clouds.
  #flashcard/cloudnative

**Q: How can you optimize resource usage?**  
?
A: Review consumption, remove unused resources, design for autoscaling.
  #flashcard/cloudnative

**Q: What are on-demand instances?**  
?
A: Resources provisioned as needed; flexible but costly if unmanaged.
  #flashcard/cloudnative

**Q: What are reserved instances?**  
?
A: Long-term resource commitments; cost-effective but less flexible.
  #flashcard/cloudnative

**Q: What are spot instances?**  
?
A: Bid for unused resources; cheap but can be terminated anytime.
  #flashcard/cloudnative

**Q: What’s the risk of lift-and-shift in cloud migration?**  
?
A: Over-provisioning resources without reevaluating needs.
  #flashcard/cloudnative

**Q: How does autoscaling help with costs?**  
?
A: Dynamically adjusts resources to demand, reducing idle costs.
  #flashcard/cloudnative

**Q: What does KubeCost do?**  
?
A: Monitors and manages Kubernetes-specific costs (open-source and commercial).
  #flashcard/cloudnative

**Q: How does cloud anomaly detection aid cost management?**  
?
A: Identifies unusual usage patterns to prevent cost overruns.
  #flashcard/cloudnative

---

## Tags

 #flashcards #cloudnative #monolithic #microservices #containerization #devops #continuousdelivery #autoscaling #serverless #cncf #kubernetes #observability #prometheus #grafana #costmanagement #docker #security #helm #servicemesh #kcna #networking #storage #rbac #scheduling #pods #deployments #services #jobs #cronjobs #configmaps #secrets #labels #api #orchestration #namespaces #pdb #statefulsets #openstandards #personas
