# Questions

## **Kubernetes Architecture and Components**

1. **Which component acts as the central management entity in a Kubernetes cluster, processing RESTful requests to manage resources?**
    - **Answer**: The **Kubernetes API Server** processes RESTful requests to manage and control cluster resources like pods, services, and deployments.
2. **What is the default setting for the --authorization-mode flag in the Kubernetes API server if --authorization-config is not used?**
    - **Answer**: **AlwaysAllow**.
3. **Which component on a node is responsible for running containers as specified in Pod definitions?**
    - **Answer**: The **Kubelet** runs on each node, managing containers as per Pod definitions by interacting with the container runtime.
4. **Which component is responsible for managing the state of worker nodes and ensuring their proper functioning?**
    - **Answer**: The **Kubelet** manages node state and ensures proper functioning.
5. **Which component in a Kubernetes cluster is responsible for routing traffic for services and managing IP rules?**
    - **Answer**: **Kube-Proxy** runs on each node, handling service traffic routing and IP rule management.
6. **Which component serves as the default Domain Name System (DNS) solution for service discovery in Kubernetes?**
    - **Answer**: **CoreDNS** provides service discovery and name resolution within the cluster.
7. **Which component is a part of the node infrastructure rather than the control plane?**
    - **Answer**: **Kube-Proxy** operates at the node level, managing network rules for pod communication.
8. **In a Kubernetes node, which component is directly responsible for running containers?**
    - **Answer**: The **Container Runtime** (e.g., Containerd, CRI-O) manages the lifecycle of containers.

---

## **Kubernetes Resources and Objects**

1. **Which Kubernetes component was introduced to supersede ReplicationControllers?**
    - **Answer**: **Deployments** manage pod replicas and updates, replacing ReplicationControllers.
2. **Which Kubernetes object is recommended for managing jobs that need to run multiple times according to a batch process?**
    - **Answer**: **CronJobs** schedule jobs based on user-defined time intervals.
3. **Which Kubernetes object is most suitable for deploying stateless applications?**
    - **Answer**: **Deployments** manage identical, stateless pod replicas and their lifecycle.
4. **Which Higher Level construct is primarily responsible for managing the desired state of identical, stateless pods?**
    - **Answer**: **Deployment**.
5. **To achieve consistent DNS naming for pods managed by a StatefulSet, what additional Kubernetes resource should you use?**
    - **Answer**: A **Headless Service** (with clusterIP: None) ensures consistent DNS naming.
6. **In Kubernetes, which entities can utilize the immutable: true attribute to ensure data cannot be modified after creation?**
    - **Answer**: **ConfigMaps** and **Secrets** can be set as immutable to prevent modifications post-creation.
7. **Which feature is crucial for managing distributed stateful applications to avoid conflicts like "split-brain" scenarios?**
    - **Answer**: **StatefulSets** ensure unique pod identities and stable state management.
8. **When the kubectl expose command is used, which component is created?**
    - **Answer**: A **Service** is created to provide network access to pods.
9. **Which Kubernetes component is responsible for exposing applications running on a set of Pods as a network service?**
    - **Answer**: A **Service** provides a stable IP and DNS name for pod access.
10. **In Kubernetes, what is a Service called when it is created without a ClusterIP?**
    - **Answer**: A **Headless Service** (set with clusterIP: None) allows direct pod access without load balancing.
11. **What is the relationship between Deployments and ReplicaSets in Kubernetes?**
    - **Answer**: A **Deployment** creates and manages a **ReplicaSet**, which ensures the desired number of pod replicas are running.
12. **Which two types of resources are used to expose applications to external traffic, including options like ClusterIP, NodePort, and LoadBalancer?**
    - **Answer**: **Services** (e.g., ClusterIP, NodePort, LoadBalancer) and **Ingress** (for HTTP/HTTPS routing).

---

## **Networking and Service Discovery**

1. **What is the primary purpose of a Service in Kubernetes?**
    - **Answer**: A **Service** exposes an application running on pods as a network service with a stable IP and DNS name.
2. **What are the primary modes of service discovery within a Kubernetes cluster?**
    - **Answer**: **Environment Variables** and **DNS** enable pods to locate services.
3. **In Kubernetes, what does Service Discovery refer to?**
    - **Answer**: **Service Discovery** is the process of services locating and communicating with each other using stable IPs or DNS names provided by Services.
4. **In a Pod, which Linux namespace do containers usually share?**
    - **Answer**: Containers in a pod share the **network namespace**, enabling localhost communication.
5. **What modification is made to a ClusterIP service to create a headless service?**
    - **Answer**: Set the clusterIP field to None.
6. **What is true about Pod-to-Pod communication within the same node in Kubernetes?**
    - **Answer**: Pods use **direct, NAT-less networking** for efficient communication on the same node.

---

## **Security**

1. **In Kubernetes security, what differentiates Security Contexts from Security Policies (e.g., PodSecurityPolicies or Kyverno)?**
    - **Answer**: **Security Contexts** define settings at the pod/container level (e.g., user, permissions) that control privileges and access controls. **Security Policies** enforce rules at the cluster/namespace level.
2. **Which function is primarily associated with Kubernetes Security Contexts?**
    - **Answer**: Defining **container or pod-level security settings** (e.g., user IDs, SELinux labels).
3. **What is the primary function of Kubernetes Security Profiles (e.g., PodSecurityPolicies, Kyverno)?**
    - **Answer**: They **define and enforce security settings at the pod level** to ensure compliance with standards.
4. **Which security-focused tool is commonly used for runtime security monitoring and detection of anomalous activities in Kubernetes?**
    - **Answer**: **Falco** monitors container and pod activity for anomalies.
5. **Which open-source tool is designed for assessing the security posture of Kubernetes clusters per NSA and CISA guidelines?**
    - **Answer**: **KubeScape** evaluates cluster security based on NSA/CISA guidelines.
6. **Which container runtimes are recognized for enhanced security features through virtualization?**
    - **Answer**: **Kata Containers** and **gVisor** provide stronger isolation via virtualization.

## 🔄 Summary Table

| Feature              | Security Context                      | Security Policy (PSP/Kyverno)              |
| -------------------- | ------------------------------------- | ------------------------------------------ |
| Who sets it?         | Pod author (developer/operator)       | Cluster admin or security team             |
| Where is it applied? | Pod/Container spec                    | Admission controller (before pod creation) |
| Purpose              | Define _how_ container runs           | Enforce _what is allowed_ in the cluster   |
| Enforced by?         | Kubelet (on node)                     | Admission Controller / Policy Engine       |
| Examples             | `runAsUser`, `readOnlyRootFilesystem` | "Disallow root", "Require seccomp profile" |

---

## **Storage and Data Management**

1. **Which software is widely used in Kubernetes for cloud-native storage orchestration?**
    - **Answer**: **Rook** automates storage systems like Ceph in Kubernetes.
2. **In the context of etcd used in Kubernetes, what is the significance of the 1.5MB size limit?**
    - **Answer**: The **1.5MB limit** is the recommended maximum size for individual values in etcd to maintain performance and stability.
3. **In Kubernetes, how would you enable data sharing between different CronJobs running at various times?**
    - **Answer**: Use a **Persistent Volume Claim (PVC)** to mount shared storage across pods.

---

## **Observability and Telemetry**

1. **What is the primary purpose of kube-state-metrics in a Kubernetes cluster?**
    - **Answer**: **Kube-state-metrics** generates and exposes cluster state data in Prometheus format by scraping the API server.
2. **In telemetry, which entity is primarily used to record and analyze the path of a request through services in a distributed system?**
    - **Answer**: **Traces** record the request’s journey across services.
3. **In distributed tracing, what is the relationship between a trace and a span?**
    - **Answer**: A **Trace** consists of multiple **spans**, where a span represents an individual operation within the trace.
4. **What are the three pillars of observability in a software system?**
    - **Answer**: **Logs**, **Metrics**, and **Traces**.
5. **Which CNCF project provides a unified framework for generating, managing, and analyzing logs, metrics, and traces?**
    - **Answer**: **OpenTelemetry** offers tools, APIs, and SDKs for observability.

---

## **CI/CD and GitOps**

1. **Which product is built using the GitOps Toolkit for GitOps-based continuous delivery?**
    - **Answer**: **Flux** leverages the GitOps Toolkit for building GitOps pipelines.
2. **Which cloud-native tools enable Kubernetes clusters to synchronize with Git repositories?**
    - **Answer**: **Argo CD** and **Flux** automate synchronization with Git repositories. ArgoCD automates the deployment. Flux synchronizes Kubernetes resources (built with GitOps Toolkit).
3. **What option aligns with the traditional workflow of a CI/CD pipeline, where "D" stands for Deployment?**
    - **Answer**: The workflow follows **Build → Test → Release → Deployment**.

---

## **Service Mesh and Traffic Management**

1. **Which tool is designed as a comprehensive service mesh for controlling, securing, and observing microservices in Kubernetes?**
    - **Answer**: **Istio** provides traffic management, security, and observability.
2. **Which software is widely used as a lightweight proxy for traffic management between microservices in Kubernetes?**
    - **Answer**: **Envoy** handles service discovery, load balancing, and circuit breaking.
3. **What are common components found in a service mesh implementation?**
    - **Answer**: **Data Plane** (proxies managing traffic) and **Control Plane** (configuring the data plane).

---

## **Container Runtimes**

1. **Which lightweight container runtime is specifically designed for Kubernetes and conforms to the Container Runtime Interface (CRI)?**
    - **Answer**: **CRI-O** is optimized for Kubernetes.
2. **What is the reference implementation of the Open Container Initiative (OCI) runtime specification?**
    - **Answer**: **runc** is the OCI reference implementation.
3. **Which container runtime is a lightweight, low-level runtime designed for running containers in Linux?**
    - **Answer**: **LXC** provides lightweight virtualization.
4. **What is the difference between LXC and CRI-O?**
    - **Answer**: **LXC** is a general-purpose lightweight virtualization technology. **CRI-O** is a Kubernetes-specific runtime conforming to CRI.

---

## **Autoscaling**

1. **Which Kubernetes autoscaling solution can scale workloads down to zero pods?**
    - **Answer**: **KEDA (Kubernetes Event-Driven Autoscaling)** supports scaling to zero based on events.
2. **How does KEDA enable event-driven autoscaling, and what custom resource does it utilize?**
    - **Answer**: KEDA scales based on external metrics (e.g., queue lengths) using the **ScaledObjects** custom resource.
3. **Which two-layered approach does Kubernetes use for its autoscaling mechanism?**
    - **Answer**: **Pod-based scaling (HPA)** and **Cluster-based scaling (Node-level scaling)**.

---

## **Workflows and Jobs**

1. **Which tool is designed for orchestrating complex parallel workflows and batch jobs in Kubernetes?**
    - **Answer**: **Argo Workflows** manages workflows using YAML/JSON definitions.

---

## **Cost Management**

1. **Which feature is effective for managing costs by categorizing and organizing resources in Kubernetes?**
    - **Answer**: **Labels** enable resource categorization for cost allocation.
2. **In cloud-native environments, which persona focuses on optimizing cloud costs and financial management?**
    - **Answer**: **FinOps (Financial Operations)** optimizes cloud spending.

---

## **Roles and Responsibilities**

1. **In a cloud-native organization, which role is responsible for building and maintaining platform infrastructure for developers?**
    - **Answer**: **Platform Engineers** enable efficient application deployment.
2. **Which persona is responsible for creating and executing incident management procedures in a cloud-native environment?**
    - **Answer**: **Site Reliability Engineers (SREs)** manage incident response.
3. **What roles and tasks are associated with a Site Reliability Engineer (SRE)?**
    - **Answer**: All tasks, including designing automated incident response, enabling developer access to logs, and mitigating service disruptions.
4. **As an SRE, which task is critical for ensuring system reliability?**
    - **Answer**: Implementing and fine-tuning **thresholds and alerts** for monitoring system health.
5. **In IT operations, what does SRE stand for?**
    - **Answer**: **Site Reliability Engineering**.

---

## **Miscellaneous**

1. **Which serverless framework in Kubernetes is known for speed and developer productivity?**
    - **Answer**: **Fission** enhances developer-centric serverless workflows.
2. **What tool is commonly used for installing and managing applications in a Kubernetes cluster?**
    - **Answer**: **Helm** simplifies application management with charts.
3. **What are the supported authorization mechanisms in Kubernetes?**
    - **Answer**: **All options are supported (e.g., RBAC, ABAC, Webhook)**.
4. **Which mechanism enables specifying criteria for selecting Kubernetes objects based on fields like name or namespace?**
    - **Answer**: **Field Selectors**.
5. **What defines Special Interest Groups (SIGs) in the Kubernetes community?**
    - **Definition**: **Exclusive teams** focusing on specific project domains.
6. **What are the default namespaces in a Kubernetes installation?**
    - **Answer**: **default, kube-system, kube-public, kube-node-lease**.
7. **For how long is the Kubernetes API guaranteed to be backward-compatible after a release?**
    - **Answer**: **At least 1 year**.
8. **What does OIDC stand for in authentication and authorization?**
    - **Answer**: **OpenID Connect**.
9. **What is OPA in Kubernetes?**
    - **Answer**: **Open Policy Agent** enforces policies across the cluster.
10. **What are CloudEvents in cloud computing?**
    - **Answer**: A **standardized format** for describing event data.
11. **Which Linux feature is used by Kubernetes for container isolation and resource limits?**
    - **Answer**: **cgroups** isolate and limit resource usage.
12. **What is the default Service Account used when none is specified for a Pod?**
    - **Answer**: The **default** service account.
13. **During container image downloading for a Pod, what state is the Pod typically in?**
    - **State**: **Pending**.
14. **What is the primary function of a Pod Disruption Budget (PDB)?**
    - **Answer**: **Maintaining a minimum number of available replicas** during voluntary disruptions.
15. **What is the default update strategy for Kubernetes Deployments?**
    - **Answer**: **Deployment**: **RollingUpdate**.
16. **Which kubectl command lists all API resources in a Kubernetes cluster?**
    - **Answer**: **Command**: **kubectl api-resources**.
17. **In cloud computing, which approach manages security threats and optimizes costs by identifying unusual activities?**
    - **Answer**: **Cloud Anomaly Detection** identifies potential threats.
18. **What ensures resiliency in cloud-native microservices architecture?**
    - **Answer**: **Resilience** through independent service operation.
19. **How are container specifications in Deployments and StatefulSets similar?**
    - **Answer**: Both use the **same container specification** in pod templates.
20. **When using kubectl apply, what is a potential drawback related to resource state tracking?**
    - **Answer**: Removed resources in the manifest may be **deleted** from the cluster unintentionally.
21. **Which Kubernetes component interacts with the Container Runtime Interface (CRI)?**
    - **Answer**: The **Kubelet** manages container operations via CRI.
22. **In multistage Docker builds, what technique reduces the final container image size?**
    - **Answer**: Use **multiple build stages** to discard unnecessary artifacts.

## Links

<https://glossary.cncf.io/>
3. **In multistage Docker builds, what technique reduces the final container image size?**
    - **Answer**: Use **multiple build stages** to discard unnecessary artifacts.

## Links

<https://glossary.cncf.io/>
