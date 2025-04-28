# Cloud-Native Flashcards

Below are flashcards optimized for Obsidian, covering Kubernetes, cloud-native architecture, and related concepts. Each flashcard uses a consistent `Question :: Answer` format and includes relevant tags for filtering.

---

What are the origins of Cloud-Native Architecture? :: Evolved from challenges with legacy monolithic applications. #flashcard #cloudnative

What is the focus of Cloud-Native Architecture? :: Improves availability, cost management, efficiency, and reliability through design patterns. #flashcard #cloudnative

What are the benefits of Cloud-Native Architecture? :: Enables loosely coupled, resilient, manageable, observable systems with robust automation. #flashcard #cloudnative

In Kubernetes, which serverless framework stands out for speed and developer productivity? :: Fission #flashcard #kubernetes #serverless

In Kubernetes, which component replaced ReplicationControllers? :: Deployments #flashcard #kubernetes

What is the default DNS solution for service discovery in Kubernetes? :: CoreDNS #flashcard #kubernetes #networking

Which Kubernetes mechanism selects information based on fields like name or status? :: Field Selectors #flashcard #kubernetes

What are the supported authorization mechanisms in Kubernetes? :: All available options are correct. #flashcard #kubernetes #security

Which CNCF project provides a unified framework for logs, metrics, and traces? :: OpenTelemetry #flashcard #cncf #observability

Which Kubernetes component manages the state of worker nodes? :: kubelet #flashcard #kubernetes

Which Kubernetes component schedules jobs based on time intervals? :: CronJobs #flashcard #kubernetes #jobs

What defines Special Interest Groups (SIGs) in Kubernetes? :: Exclusive teams focusing on specific project areas. #flashcard #kubernetes #cncf

What are the two Kubernetes resources for exposing applications to external traffic? :: Service (stable IP/DNS) and Ingress (HTTP routing/SSL). #flashcard #kubernetes #networking

Which cloud-native tools synchronize Kubernetes clusters with Git repositories? :: Argo CD and Flux #flashcard #kubernetes #gitops

Which product is built using the GitOps Toolkit for continuous delivery? :: Flux #flashcard #kubernetes #gitops

Which Kubernetes component primarily manages a Node? :: Kubelet #flashcard #kubernetes

What Kubernetes resource ensures consistent DNS naming for StatefulSet pods? :: Headless Service #flashcard #kubernetes #statefulsets

Which tool is a comprehensive service mesh for Kubernetes microservices? :: Istio #flashcard #kubernetes #servicemesh

How long is the Kubernetes API guaranteed to be backward compatible? :: At least 1 year following a release. #flashcard #kubernetes #api

What are CloudEvents in cloud computing? :: Standards for describing event data in a common format. #flashcard #cloudnative

Which Kubernetes tool orchestrates complex parallel workflows? :: Argo Workflows #flashcard #kubernetes #jobs

What is true about Pod-to-Pod communication on the same node in Kubernetes? :: Uses direct, NAT-less networking. #flashcard #kubernetes #networking

What is the reference implementation of the OCI runtime specification? :: runc #flashcard #containers #openstandards

What are the three pillars of observability in a software system? :: Logs, Metrics, Traces #flashcard #observability

What is the significance of the 1.5MB size limit in etcd for Kubernetes? :: Recommended maximum size for individual values to maintain performance. #flashcard #kubernetes #etcd

In telemetry, what records the path of a request through services in a distributed system? :: Trace #flashcard #observability

What does OIDC stand for in authentication and authorization? :: OpenID Connect #flashcard #security

What is the primary function of Kubernetes Security Profiles like PSP and Kyverno? :: Enforce security settings at the pod level. #flashcard #kubernetes #security

Which software is used as a lightweight proxy for traffic management in Kubernetes? :: Envoy #flashcard #kubernetes #servicemesh

What is the traditional workflow of a CI/CD pipeline? :: Build, Testing, Release, Deployment #flashcard #devops

Which Kubernetes object manages jobs that run multiple times in a batch process? :: CronJob #flashcard #kubernetes #jobs

What technique reduces the size of Docker container images in multistage builds? :: Discarding unnecessary artifacts using multiple build stages. #flashcard #docker #containers

What is a Kubernetes service without a ClusterIP called? :: Headless Service #flashcard #kubernetes #networking

Which Kubernetes entities can use the immutable:true attribute? :: ConfigMaps and Secrets #flashcard #kubernetes #configmaps #secrets

How can data be shared between cronjobs in Kubernetes? :: Persistent Volume Claim (PVC) #flashcard #kubernetes #storage

Which Kubernetes component is part of the node infrastructure, not the control plane? :: kube-proxy #flashcard #kubernetes

What is the primary purpose of kube-state-metrics in Kubernetes? :: Generates and exposes cluster state data in Prometheus format. #flashcard #kubernetes #observability

What is true about Ingress in Kubernetes for traffic routing? :: Routes external HTTP/HTTPS traffic to services based on rules. #flashcard #kubernetes #networking

Which tool assesses Kubernetes cluster security per NSA/CISA guidelines? :: KubeScape #flashcard #kubernetes #security

Which Kubernetes object is best for stateless applications? :: Deployment #flashcard #kubernetes #deployments

Which role builds and maintains platform infrastructure in a cloud-native organization? :: Platform Engineers #flashcard #cloudnative #personas

Which Kubernetes component routes traffic for services and manages IP rules? :: Kube-Proxy #flashcard #kubernetes #networking

Which tool monitors runtime security in Kubernetes containers and pods? :: Falco #flashcard #kubernetes #security

What is the primary function of Kubernetes Security Contexts? :: Define container or pod-level security settings. #flashcard #kubernetes #security

What is the relationship between Deployments and ReplicaSets in Kubernetes? :: Deployments create ReplicaSets to manage pod replicas. #flashcard #kubernetes #deployments

How do Security Contexts differ from Security Policies in Kubernetes? :: Security Contexts apply to single pods/containers; Security Policies enforce at cluster/namespace level. #flashcard #kubernetes #security

Which Kubernetes feature manages costs by categorizing resources? :: Labels #flashcard #kubernetes #costmanagement

Which persona optimizes cloud costs in cloud-native environments? :: FinOps #flashcard #cloudnative #personas #costmanagement

Which container runtime is designed for Kubernetes and conforms to CRI? :: CRI-O #flashcard #kubernetes #containers

Which container runtime is a lightweight, low-level runtime for Linux containers? :: LXC #flashcard #containers

What is the difference between LXC and CRI-O? :: LXC is for lightweight Linux containers; CRI-O is built for Kubernetes CRI. #flashcard #containers

Which approach manages security threats and optimizes costs in cloud computing? :: Cloud Anomaly Detection #flashcard #cloudnative #security #costmanagement

What default Service Account is used for a Pod without one specified in Kubernetes? :: default service account #flashcard #kubernetes #security

What does SRE stand for in IT operations? :: Site Reliability Engineering #flashcard #cloudnative #personas

How are container specifications similar in Kubernetes Deployments and StatefulSets? :: Both use identical pod template container specs. #flashcard #kubernetes #deployments #statefulsets

What is a key issue with tightly coupled components in monolithic apps? :: Changes in one area affect the entire app; library updates may break compatibility. #flashcard #monolithic

How were monolithic apps traditionally installed? :: Directly on the OS via package managers like APT/YUM, sharing resources. #flashcard #monolithic

Why is the architecture of monolithic apps fragile? :: Tight coupling of web server, business logic, and data interfaces complicates changes. #flashcard #monolithic

What is the Microservices Architecture in Cloud-Native? :: Breaks apps into smaller, standalone units for easier management and updates. #flashcard #cloudnative #microservices

How does encapsulation benefit Cloud-Native apps? :: Isolates microservice dependencies, reducing conflicts (e.g., /main vs /store). #flashcard #cloudnative #microservices

How does Cloud-Native improve networking? :: Isolated networking for microservices avoids port conflicts and simplifies maintenance. #flashcard #cloudnative #networking

What flexibility does the data layer gain in Cloud-Native? :: Microservices can independently interact with databases or use replication/sharding. #flashcard #cloudnative #microservices

What does Cloud-Native Architecture involve? :: Microservices, containerization, CI/CD for scalability and resilience. #flashcard #cloudnative

How does Cloud-Native ensure high availability? :: Uses redundancy, self-healing, and scalability to handle failures. #flashcard #cloudnative

What practical benefits does Cloud-Native offer? :: Easier maintenance, deployment, upgrades, and frequent changes via modularity. #flashcard #cloudnative

How does resiliency work in Cloud-Native apps? :: Withstands failures via redundancy, failover, and self-healing (e.g., Kubernetes pod replacement). #flashcard #cloudnative

What does agility mean in Cloud-Native? :: Rapid development, modification, and deployment using microservices and CI/CD. #flashcard #cloudnative

What is operability in Cloud-Native apps? :: Easy deployment and management with automation and IaC (e.g., Terraform). #flashcard #cloudnative

What is observability in Cloud-Native? :: Insight into system state via logs, metrics, and traces for diagnostics. #flashcard #cloudnative #observability

What’s a mnemonic for Cloud-Native characteristics? :: RAOO - "Racoons Are Often Observant" (Resiliency, Agility, Operability, Observability). #flashcard #cloudnative

How does resilience manifest in Cloud-Native? :: Apps anticipate failures with self-healing (e.g., Kubernetes restarts failed pods). #flashcard #cloudnative

What role does automation play in Cloud-Native? :: Reduces manual effort with tools like Ansible/Terraform for consistent deployments. #flashcard #cloudnative #devops

What’s the difference between CI, CDelivery, and CDeployment? :: CI: frequent commits/testing; CDelivery: automated release prep; CDeployment: automated production release. #flashcard #devops

What does "Secure by Default" mean? :: Systems designed with security from the start (e.g., Zero Trust, least privilege). #flashcard #cloudnative #security

How does Cloud-Native optimize speed, efficiency, and cost? :: Uses auto-scaling, serverless, and proactive design for dynamic workloads. #flashcard #cloudnative #costmanagement

What is service discovery in Cloud-Native? :: Automates service detection with tools like Kubernetes DNS. #flashcard #cloudnative #networking

What are the four pillars of Cloud-Native Architecture? :: Microservices, Containerisation, DevOps, Continuous Delivery #flashcard #cloudnative

How does Containerisation benefit Cloud-Native? :: Encapsulates apps and dependencies for consistency and isolation. #flashcard #cloudnative #containerization

What is DevOps in Cloud-Native? :: Combines Dev and Ops for automation, monitoring, and collaboration. #flashcard #cloudnative #devops

What’s a mnemonic for the Cloud-Native pillars? :: "Morning Coffee Delivers Caffeine Delight" (Microservices, Containerisation, DevOps, CD). #flashcard #cloudnative

What is Auto Scaling? :: Automatically adjusts resources based on workload for performance/cost efficiency. #flashcard #cloudnative #autoscaling

What are the three types of Auto Scaling? :: Reactive (threshold-based), Scheduled (predictable spikes), Predictive (AI-based). #flashcard #cloudnative #autoscaling

What’s the difference between Vertical and Horizontal Scaling? :: Vertical: increases server resources; Horizontal: adds/removes instances. #flashcard #cloudnative #autoscaling

What are Kubernetes’ autoscaling tools? :: Cluster Autoscaler, HPA, VPA, KEDA (event-driven). #flashcard #kubernetes #autoscaling

What’s a key consideration for autoscaling? :: Testing ensures performance; horizontal scaling increases data-sharing complexity. #flashcard #cloudnative #autoscaling

What is Serverless Computing? :: Providers manage servers; code runs in response to events, auto-scaling. #flashcard #cloudnative #serverless

What are the core features of Serverless? :: Event-driven, auto-scaling (to zero), abstracted infrastructure. #flashcard #cloudnative #serverless

What affects costs in Serverless? :: Code execution (executions/duration) and autoscaling (resources used). #flashcard #cloudnative #serverless #costmanagement

What is AWS Lambda? :: FaaS platform executing code in response to events with auto-scaling. #flashcard #cloudnative #serverless

What’s a challenge in Serverless? :: Vendor lock-in and cold start latency after inactivity. #flashcard #cloudnative #serverless

What are open-source Serverless options for Kubernetes? :: Knative and OpenFaaS #flashcard #kubernetes #serverless

What is the CNCF? :: Vendor-neutral org under Linux Foundation, hosting cloud-native projects. #flashcard #cncf

What are CNCF’s project maturity levels? :: Sandbox, Incubating, Graduated #flashcard #cncf

What does the Technical Oversight Committee (TOC) do? :: Evaluates project maturity based on adoption and quality. #flashcard #cncf

What’s the role of SIGs and TAGs in CNCF? :: SIGs focus on project areas; TAGs provide domain guidance. #flashcard #cncf

What does a DevOps Engineer focus on? :: Bridges Dev and Ops with automation and cloud-native practices. #flashcard #cloudnative #personas #devops

What’s the role of a Site Reliability Engineer (SRE)? :: Ensures reliability at scale with SLAs/SLOs and incident management. #flashcard #cloudnative #personas

What does a Cloud Architect do? :: Designs cloud solutions, optimizing cost and performance. #flashcard #cloudnative #personas

What’s unique about a FinOps Engineer? :: Balances cloud spending with speed and quality. #flashcard #cloudnative #personas #costmanagement

What are Open Standards? :: Freely adoptable frameworks for collaboration and interoperability. #flashcard #cloudnative #openstandards

What is the Open Container Initiative (OCI)? :: Defines container image and runtime specs (e.g., runc). #flashcard #cloudnative #openstandards

What does the Container Network Interface (CNI) do? :: Simplifies networking setup for Kubernetes with plugins. #flashcard #kubernetes #networking #openstandards

What’s the purpose of the Container Storage Interface (CSI)? :: Standardizes storage integration for container orchestration. #flashcard #kubernetes #storage #openstandards

What does "Cloud Native" mean? :: Combines cloud environments and native design for scalability and resilience. #flashcard #cloudnative

What are the two main philosophies of CNCF? :: Cloud Native Architecture (best practices) and Culture (organizational changes). #flashcard #cloudnative #cncf

What are the key benefits of Cloud Native practices? :: Resilience, scalability, automation, security, and effective cloud use. #flashcard #cloudnative

How does Infrastructure-as-Code (IaC) relate to Cloud Native? :: Enables vendor-agnostic management with tools like Terraform. #flashcard #cloudnative #devops

Does running an app in the cloud make it Cloud Native? :: No, it requires best practices, not just cloud hosting. #flashcard #cloudnative

Are containers enough to make an app Cloud Native? :: No, they’re a step, but best practices are needed. #flashcard #cloudnative #containerization

What are the four key practices in Cloud Native development? :: Automation, Resilience, Scalability, Security by Default #flashcard #cloudnative

What challenges does Cloud Native solve? :: Single system errors, resource limitations, and noisy neighbors. #flashcard #cloudnative

What is the Linux Foundation’s role in Cloud Native? :: Standardizes Linux, sponsors Kubernetes, drives open cloud movement. #flashcard #cloudnative #cncf

What is the Cloud Native Computing Foundation (CNCF)? :: Promotes cloud-native tech, oversees Kubernetes, shapes ecosystem. #flashcard #cloudnative #cncf

When was Kubernetes’ first major milestone? :: February 2015, release 1.0. #flashcard #kubernetes #cncf

What is the KCNA certification? :: Entry-level cert for Kubernetes and cloud-native ecosystems. #flashcard #kcna

What advanced certifications follow KCNA? :: CKA (Administrator) and CKAD (Application Developer). #flashcard #kcna

What topics does KCNA cover? :: Kubernetes, security, telemetry/observability (e.g., Prometheus). #flashcard #kcna

Why is KCNA a strong foundation? :: Prepares for advanced certs with practical knowledge. #flashcard #kcna

What is the KCNA exam format? :: Multiple-choice, entry-level, theoretical. #flashcard #kcna

How can you take the KCNA exam? :: In-person at test centers or remotely with proctoring. #flashcard #kcna

Why is KCNA a good choice? :: Diverse curriculum, prepares for advanced certs, remote-friendly. #flashcard #kcna

Where can you check KCNA curriculum updates? :: https://github.com/cncf/curriculum #flashcard #kcna

How can you stay updated on KCNA exam news? :: Follow Linux Foundation and CNCF on social media. #flashcard #kcna

Where do you book the KCNA exam? :: https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate #flashcard #kcna

How can you reduce KCNA exam costs? :: Wait for sales, attend KubeCon, or check social media for codes. #flashcard #kcna

What should you focus on for the KCNA exam? :: IaC, Speed, Efficiency, and Cost in Cloud Native contexts. #flashcard #kcna

What’s the official source for KCNA exam info? :: GitHub repo cncf/curriculum #flashcard #kcna

How did containers transform technology? :: Revolutionized infrastructure, app design, and operations with tools like Kubernetes. #flashcard #containers

What’s the historical foundation of mainframe virtualization (1960s)? :: IBM CP/CMS allowed multiple virtualized OS instances on a mainframe. #flashcard #containers

What is Chroot (1979) and its limitations? :: Unix feature isolating processes; limited by shared resources and no superuser processes. #flashcard #containers

What are FreeBSD Jails (2000)? :: Lightweight virtualization isolating resources in FreeBSD. #flashcard #containers

What are Solaris Zones and HP-UX Virtual Partitions (2000s)? :: Virtualization isolating compute resources in Solaris and HP-UX. #flashcard #containers

What modern ingredients did Docker combine? :: Linux Namespaces (isolation) and cgroups (resource allocation). #flashcard #docker #containers

What are Linux Namespaces (2002)? :: Isolate processes, networks, mounts, etc. (e.g., PID, NET, MNT). #flashcard #containers

What are Control Groups (cgroups, 2006)? :: Limit, prioritize, and control resources; developed by Google. #flashcard #containers

How do Virtual Machines compare to containers? :: VMs use hypervisors with OS overhead; containers share host kernel. #flashcard #containers

What made Docker a game-changer in 2013? :: Combined namespaces and cgroups into a user-friendly platform. #flashcard #docker #containers

Why are containers preferred over VMs? :: Lightweight, share host OS kernel, reduce overhead. #flashcard #containers

What’s the traditional Docker architecture? :: Hardware → OS → Docker runtime (containerd, runc). #flashcard #docker

What does Docker Desktop provide? :: UI for Mac/Windows/Linux with CLI, Kubernetes, and Extensions. #flashcard #docker

What’s a key benefit of Docker Desktop? :: Flexible, self-contained environment with easy reset. #flashcard #docker

How do you run a container with Docker Desktop? :: `docker run -it ubuntu bash` #flashcard #docker

How do you enable Kubernetes in Docker Desktop? :: Turn on in preferences to use `kubectl`. #flashcard #docker #kubernetes

What is a container image? :: Portable, OCI-compliant bundle of software and dependencies. #flashcard #containers

What’s the difference between a container image and a container? :: Image is the blueprint; container is a running instance. #flashcard #containers

What are tags in container images? :: Labels for versions/variants (e.g., latest, v1.0). #flashcard #containers

How are container images structured? :: Built as layers; each step creates a reusable layer. #flashcard #containers

What are drawbacks of too many image layers? :: Increased build time, size, and complexity. #flashcard #containers

What’s a Union File System in Docker? :: Merges image layers into a single view with a writable layer. #flashcard #docker #containers

What’s the difference between a digest and an image ID? :: Digest identifies registry images; Image ID is a local config hash. #flashcard #containers

How do you save a container image? :: `docker save` exports it locally with layers and metadata. #flashcard #docker #containers

What is a container registry? :: Service hosting/distributing container images with versioning. #flashcard #containers

How do you validate a Docker installation? :: `docker version` shows client/server details. #flashcard #docker

What’s the Funbox image used for? :: Explore features like Nyan Cat with `docker run -it --rm wernight/funbox`. #flashcard #docker

What do `-i`, `-t`, and `--rm` do in Docker run? :: `-i`: keeps input open; `-t`: allocates terminal; `--rm`: auto-removes container. #flashcard #docker

How do you view container states? :: `docker ps` for running; `docker ps -a` for all. #flashcard #docker

What’s a security best practice for running containers? :: Run as non-root users to reduce attack surface. #flashcard #docker #security

What’s the difference between `-P` and `-p` in Docker run? :: `-P`: publishes all exposed ports randomly; `-p`: maps specific ports. #flashcard #docker #networking

What does `-d` do in Docker run? :: Detaches container to run in background. #flashcard #docker

How do you explore a running container? :: `docker exec -it <container-id> bash` #flashcard #docker

How do you make persistent changes to a container? :: Use volumes with `-v /host/path:/container/path`. #flashcard #docker #storage

What is container orchestration? :: Automates scaling, updates, and availability for containers. #flashcard #orchestration

What are key features of container orchestration? :: Provisioning, self-healing, security, auto-scaling. #flashcard #orchestration

How does orchestration benefit complex app deployment? :: Standardizes deployment, integrates networking/storage. #flashcard #orchestration

What are some container orchestrators? :: Nomad, OpenShift, Docker, Kubernetes #flashcard #orchestration

What are CRDs in Kubernetes? :: Custom Resource Definitions extend Kubernetes functionality. #flashcard #kubernetes #api

What are the two main areas of Kubernetes architecture? :: Control Plane (manages cluster) and Nodes (run workloads). #flashcard #kubernetes

What’s the role of the container runtime? :: Low-level (runc) interacts with namespaces; high-level (containerd) manages lifecycle. #flashcard #kubernetes #containers

What does Kubelet do? :: Runs on nodes, ensures pod health, reads specs from API/YAML. #flashcard #kubernetes

What is etcd? :: Distributed key-value store for cluster data with Raft consensus. #flashcard #kubernetes #etcd

What’s the Kube API Server? :: Main cluster access point, exposes RESTful API, stores data in etcd. #flashcard #kubernetes #api

What does the Kube Scheduler do? :: Assigns pods to nodes based on resources/constraints. #flashcard #kubernetes #scheduling

What’s Kube Proxy’s role? :: Manages network connectivity (TCP/UDP/SCTP) on nodes. #flashcard #kubernetes #networking

What is CoreDNS? :: Provides cluster DNS resolution, runs as a Deployment. #flashcard #kubernetes #networking

What does the Controller Manager handle? :: Runs control loops to enforce desired state. #flashcard #kubernetes

What’s the Cloud Controller Manager (CCM)? :: Bridges Kubernetes with cloud providers for load balancers. #flashcard #kubernetes

How does a High Availability (HA) cluster work? :: Multiple control planes, clustered etcd, load-balanced API Server. #flashcard #kubernetes

What is a Pod in Kubernetes? :: Smallest deployable unit with one or more containers. #flashcard #kubernetes #pods

How do containers in a pod communicate? :: Via localhost; each pod gets a unique cluster IP. #flashcard #kubernetes #pods #networking

How do you create an nginx pod? :: `kubectl run nginx --image=nginx` #flashcard #kubernetes #pods

How do you access a pod externally? :: `kubectl port-forward nginx 8080:80` #flashcard #kubernetes #pods #networking

How do you test inter-pod communication? :: `kubectl run curl --image=curlimages/curl --rm -it --restart=Never -- curl <nginx-pod-IP>` #flashcard #kubernetes #pods #networking

What’s the difference between `kubectl create` and `apply`? :: `create` fails if resource exists; `apply` updates existing resources. #flashcard #kubernetes #api

How do you generate YAML for a pod? :: `kubectl run nginx --image=nginx --dry-run=client -o yaml > nginx.yaml` #flashcard #kubernetes #pods

What are the restartPolicy options? :: Always, Never, OnFailure #flashcard #kubernetes #pods

How do you combine multiple YAMLs into one file? :: Use `---` separator #flashcard #kubernetes #api

What’s a sidecar container? :: Secondary container enhancing main container functionality. #flashcard #kubernetes #pods

What are Kubernetes namespaces? :: Divide cluster resources for isolation. #flashcard #kubernetes #namespaces

Why use namespaces? :: Multi-tenancy, quotas, RBAC, organization. #flashcard #kubernetes #namespaces

What are common default namespaces? :: kube-system, kube-public, kube-node-lease #flashcard #kubernetes #namespaces

How do you create a namespace? :: `kubectl create namespace my-namespace` #flashcard #kubernetes #namespaces

How do you set a default namespace? :: `kubectl config set-context --current --namespace=my-namespace` #flashcard #kubernetes #namespaces

What is a Kubernetes Deployment? :: Manages app updates with replication and rollbacks. #flashcard #kubernetes #deployments

How do you scale a deployment? :: `kubectl scale deployment/nginx --replicas=10` #flashcard #kubernetes #deployments

How do you update a deployment? :: `kubectl set image deployment/nginx nginx=nginx:stable` #flashcard #kubernetes #deployments

How do you rollback a deployment? :: `kubectl rollout undo deployment/nginx` #flashcard #kubernetes #deployments

What manages pod lifecycles in a deployment? :: ReplicaSets #flashcard #kubernetes #deployments

What are the four main Kubernetes service types? :: ClusterIP, NodePort, LoadBalancer, ExternalName #flashcard #kubernetes #services

What’s a ClusterIP service? :: Internal-only IP for cluster communication. #flashcard #kubernetes #services #networking

What’s a NodePort service? :: Exposes a static port on each node for external access. #fallashcard #kubernetes #services #networking

What’s a LoadBalancer service? :: Uses a cloud provider’s load balancer for external access. #flashcard #kubernetes #services #networking

What’s a Headless Service? :: No cluster IP, resolves to pod IPs. #flashcard #kubernetes #services #networking

What is a Kubernetes Job? :: Manages batch tasks, runs pods to completion. #flashcard #kubernetes #jobs

What’s an example of a Job use case? :: Calculating Pi with `perl -Mbignum=bpi -wle "print bpi(2000)"`. #flashcard #kubernetes #jobs

What do `completions` and `parallelism` control? :: `completions`: total pods; `parallelism`: concurrent pods. #flashcard #kubernetes #jobs

What is a Kubernetes CronJob? :: Schedules Jobs at intervals (e.g., `* * * * *`). #flashcard #kubernetes #cronjobs

What happens when a CronJob is deleted? :: Stops scheduling; existing Jobs/pods persist. #flashcard #kubernetes #cronjobs

What’s the default job history retention? :: 3 successful, 1 failed #flashcard #kubernetes #cronjobs

What are ConfigMaps? :: Store non-sensitive configuration data for pods. #flashcard #kubernetes #configmaps

How do you create a ConfigMap? :: `kubectl create configmap color-configmap --from-literal=color=red` #flashcard #kubernetes #configmaps

How do you use a ConfigMap in a pod? :: `envFrom: - configMapRef: name: color-configmap` in YAML #flashcard #kubernetes #configmaps

What’s an immutable ConfigMap? :: Cannot be modified after creation (immutable: true). #flashcard #kubernetes #configmaps

What are Kubernetes Secrets? :: Store sensitive data (e.g., passwords) encoded in Base64. #flashcard #kubernetes #secrets

How do Secrets differ from ConfigMaps? :: Secrets for sensitive data; ConfigMaps for non-sensitive. #flashcard #kubernetes #secrets #configmaps

How do you create a Secret? :: `kubectl create secret generic color-secret --from-literal=color=red` #flashcard #kubernetes #secrets

Are Secrets encrypted? :: No, only Base64-encoded; stored in etcd. #flashcard #kubernetes #secrets #security

What are Kubernetes Labels? :: Key-value pairs to tag and organize resources. #flashcard #kubernetes #labels

Why use Labels? :: Organize resources, automate CI/CD, enable service discovery. #flashcard #kubernetes #labels

How do you create a pod with labels? :: `kubectl run nginx --image=nginx --labels="app=web,env=prod"` #flashcard #kubernetes #labels #pods

How do you filter by labels? :: `kubectl get pods --selector app=web` #flashcard #kubernetes #labels

What interacts with the Kubernetes API? :: Users/tools, monitoring, internal components (scheduler, kubelet). #flashcard #kubernetes #api

What’s the API request processing flow? :: Request → Route → Authn → Authz → Admission → Validation → Handling → Response #flashcard #kubernetes #api

What does the Admission Controller do in the API flow? :: Enforces policies and modifies resources. #flashcard #kubernetes #api

What are Custom Resource Definitions (CRDs)? :: Extend the API with new resource types. #flashcard #kubernetes #api

How does `kubectl proxy` simplify API access? :: Allows local HTTP access without manual auth. #flashcard #kubernetes #api

What’s the OpenAPI specification in Kubernetes? :: Documents API endpoints and parameters. #flashcard #kubernetes #api

What is RBAC in Kubernetes? :: Manages access using roles assigned to users/groups. #flashcard #kubernetes #rbac #security

What’s in a kubeconfig file? :: Cluster details and user info (e.g., certificates). #flashcard #kubernetes #rbac #security

How does Kubernetes authenticate users? :: Via external certificates signed by a CA. #flashcard #kubernetes #rbac #security

What’s a ClusterRole vs. ClusterRoleBinding? :: ClusterRole defines permissions; ClusterRoleBinding assigns them. #flashcard #kubernetes #rbac #security

How do you check permissions with kubectl? :: `kubectl auth can-i <verb> <resource>` #flashcard #kubernetes #rbac #security

How do you create a superuser group? :: Create ClusterRole with all permissions and bind to group. #flashcard #kubernetes #rbac #security

What does the kube-scheduler do? :: Assigns pods to nodes based on resources and constraints. #flashcard #kubernetes #scheduling

What are the three steps in scheduling? :: Filtering, Scoring, Binding #flashcard #kubernetes #scheduling

How do you bypass the scheduler? :: Set `nodeName` in pod spec. #flashcard #kubernetes #scheduling

What’s a custom scheduler? :: Uses custom logic via `schedulerName` in pod spec. #flashcard #kubernetes #scheduling

How do NodeSelectors work? :: Use labels for targeted pod placement. #flashcard #kubernetes #scheduling

What is ephemeral storage in Kubernetes? :: Temporary storage that doesn’t persist across pod restarts. #flashcard #kubernetes #storage

What’s an emptyDir volume? :: Created empty; persists across container crashes but not pod deletion. #flashcard #kubernetes #storage

What’s persistent storage in Kubernetes? :: Storage persisting beyond pod lifecycle (e.g., PVs, PVCs). #flashcard #kubernetes #storage

What are Storage Classes, PVs, and PVCs? :: Storage Classes define types; PVs are provisioned; PVCs request storage. #flashcard #kubernetes #storage

What’s dynamic provisioning? :: PVC auto-creates PV against a StorageClass. #flashcard #kubernetes #storage

What are StatefulSets used for? :: Manage stateful apps with stable identities and storage. #flashcard #kubernetes #statefulsets

How do StatefulSets differ from Deployments? :: StatefulSets provide unique pod identities and PVCs. #flashcard #kubernetes #statefulsets #deployments

What’s a headless service in StatefulSets? :: No cluster IP, provides stable DNS names. #flashcard #kubernetes #statefulsets #services

How are StatefulSet pods named? :: Sequentially with StatefulSet name prefix. #flashcard #kubernetes #statefulsets

What happens to PVCs when a StatefulSet is deleted? :: PVCs and PVs persist, reusable when recreated. #flashcard #kubernetes #statefulsets #storage

What do Network Policies do? :: Control pod communication by allowing/denying traffic. #flashcard #kubernetes #networking

What’s the role of the CNI plugin in Network Policies? :: Enforces network rules for traffic control. #flashcard #kubernetes #networking #openstandards

What happens without Network Policies? :: Pods can send/receive traffic from any source. #flashcard #kubernetes #networking

How do multiple Network Policies combine? :: Effects are additive, creating cumulative rules. #flashcard #kubernetes #networking

What’s the difference between PDB and Replicas? :: Replicas ensure running pods; PDBs limit disruptions. #flashcard #kubernetes #pdb

What are voluntary disruptions? :: Planned actions (e.g., maintenance) that may terminate pods. #flashcard #kubernetes #pdb

How does a PDB work during node drain? :: Prevents eviction below minimum available pods. #flashcard #kubernetes #pdb

What are Security Contexts in Kubernetes? :: Define access control/privileges for pods/containers. #flashcard #kubernetes #security

What does `allowPrivilegeEscalation: false` do? :: Prevents containers from gaining elevated privileges. #flashcard #kubernetes #security

What replaced Pod Security Policies in Kubernetes 1.25? :: Admission Controllers #flashcard #kubernetes #security

What are Kyverno and OPA Gatekeeper? :: Tools for custom policy enforcement via admission controllers. #flashcard #kubernetes #security

What are the 4C’s of Cloud Native Security? :: Cloud, Cluster, Container, Code #flashcard #cloudnative #security

What is Helm in Kubernetes? :: Simplifies app management with Helm Charts. #flashcard #kubernetes #helm

How do you create a Helm Chart? :: `helm create flappy-app` #flashcard #kubernetes #helm

How do you deploy a Helm Chart? :: `helm install flappy-app <chart-file>.tgz` #flashcard #kubernetes #helm

What’s the benefit of Helm Charts? :: Easy deployment, upgrades, and scaling. #flashcard #kubernetes #helm

What’s the role of a service mesh? :: Manages secure, reliable microservice communication. #flashcard #cloudnative #servicemesh

What are the two parts of a service mesh? :: Data Plane (proxies) and Control Plane (manages proxies). #flashcard #cloudnative #servicemesh

What’s a sidecar proxy vs. host node proxy? :: Sidecar attaches to services; host node runs at node level. #flashcard #cloudnative #servicemesh

What are key benefits of service meshes? :: Security, access control, observability, reliability. #flashcard #cloudnative #servicemesh

What’s the Service Mesh Interface (SMI)? :: Unified API for managing traffic and metrics. #flashcard #cloudnative #servicemesh #openstandards

What is observability in cloud-native systems? :: Determines system state via telemetry (logs, metrics, traces). #flashcard #cloudnative #observability

What are the three pillars of observability? :: Logs, Metrics, Traces #flashcard #cloudnative #observability

What do alerts do in observability? :: Provide early warnings of anomalies or failures. #flashcard #cloudnative #observability

What’s the role of OpenTracing and OpenTelemetry? :: Provide APIs/tools for tracing and telemetry. #flashcard #cloudnative #observability

How do logs help in observability? :: Record events for troubleshooting and analysis. #flashcard #cloudnative #observability

What are the types of metrics? :: Gauges, Counters, Meters, Histograms #flashcard #cloudnative #observability

What do traces provide in a distributed system? :: Visibility into request latency and bottlenecks. #flashcard #cloudnative #observability

What is Prometheus? :: Open-source CNCF tool for monitoring/alerting with PromQL. #flashcard #prometheus #observability

What are key features of Prometheus? :: PromQL, autonomous nodes, push-pull data collection. #flashcard #prometheus #observability

What is Grafana? :: Platform for visualizing metrics from Prometheus and other sources. #flashcard #grafana #observability

How do Prometheus and Grafana enhance Kubernetes observability? :: Offer monitoring, visualization, and alerting. #flashcard #kubernetes #prometheus #grafana #observability

What’s the kube-prometheus-stack? :: Helm chart for Prometheus/Grafana setup in Kubernetes. #flashcard #kubernetes #prometheus #helm

What components are installed with kube-prometheus-stack? :: Prometheus Operator, Alertmanager, Node Exporter, Kube-State-Metrics, Grafana #flashcard #kubernetes #prometheus

How do you access Prometheus UI after installation? :: `kubectl port-forward service/<prometheus-service> 9090:9090` #flashcard #kubernetes #prometheus

What’s an example PromQL query for pods per namespace? :: `sum by (namespace) (kube_pod_ips)` #flashcard #prometheus #observability

How do you import dashboards in Grafana? :: Use IDs (e.g., 15759) and select Prometheus data source. #flashcard #grafana #observability

What’s a key principle of cloud-native cost management? :: Flexibility to allocate resources across clouds. #flashcard #cloudnative #costmanagement

How can you optimize resource usage? :: Review consumption, remove unused resources, design for autoscaling. #flashcard #cloudnative #costmanagement

What are on-demand instances? :: Resources provisioned as needed; flexible but costly. #flashcard #cloudnative #costmanagement

What are reserved instances? :: Long-term commitments; cost-effective but less flexible. #flashcard #cloudnative #costmanagement

What are spot instances? :: Bid for unused resources; cheap but terminable. #flashcard #cloudnative #costmanagement

What’s the risk of lift-and-shift in cloud migration? :: Over-provisioning without reevaluating needs. #flashcard #cloudnative #costmanagement

How does autoscaling help with costs? :: Adjusts resources to demand, reducing idle costs. #flashcard #cloudnative #costmanagement #autosscaling

What does KubeCost do? :: Monitors and manages Kubernetes-specific costs. #flashcard #kubernetes #costmanagement

How does cloud anomaly detection aid cost management? :: Identifies unusual usage to prevent overruns. #flashcard #cloudnative #costmanagement

---

## Tags
#flashcard #cloudnative #monolithic #microservices #containerization #devops #continuousdelivery #autoscaling #serverless #cncf #kubernetes #observability #prometheus #grafana #costmanagement #docker #security #helm #servicemesh #kcna #networking #storage #rbac #scheduling #pods #deployments #services #jobs #cronjobs #configmaps #secrets #labels #api #orchestration #namespaces #pdb #statefulsets #openstandards #personas
