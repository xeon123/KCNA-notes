# Miscellaneous

## Questions

- In the realm of Kubernetes, which serverless framework stands out for its exceptional speed, developer-centric approach, and optimal performance, aiming to enhance developer productivity?
	- Fission
- In Kubernetes, which component was specifically introduced to supersede the outdated ReplicationControllers?
	- Deployments
- In Kubernetes, which component serves as the default Domain Name System (DNS) solution for service discovery and name resolution within the cluster?
	- CoreDNS
- Which mechanism in Kubernetes enables you to specify criteria for selecting information based on fields such as name, namespace, or status of a Kubernetes object?
	- Field Selectors
- What are the supported authorization mechanisms or modules in Kubernetes?
	- All of the options are correct
- Which CNCF project focuses on providing a unified framework for generating, managing, and analyzing logs, metrics, and traces through a comprehensive set of tools, APIs, and SDKs?
	- OpenTelemetry
- Which component in a Kubernetes cluster is responsible for managing the state of worker nodes and ensuring their proper functioning?
	- kubelet
- Which of the following container runtimes is a lightweight, low-level runtime designed specifically for running containers in Linux?
	- LXC
- Which Kubernetes component enables developers to schedule jobs based on user-defined time intervals?
	- CronJobs
- Within the collaborative landscape of Kubernetes, what defines Special Interest Groups (SIGs)?
	- Exclusive teams focusing on specific areas or domains of the project.
- In Kubernetes, which two types of resources are used to expose applications to external traffic, including options like ClusterIP, NodePort, and LoadBalancer?
	-  A Service provides a stable IP address and DNS name for accessing an application, while an Ingress resource acts as an entry point for HTTP requests, allowing you to define routing rules and SSL termination.
- Which cloud-native tools enable Kubernetes clusters to be automatically synchronised with Git repositories?
	- Argo CD and Flux are cloud-native tools that enable Kubernetes clusters to be automatically synchronized with Git repositories. Argo CD is a declarative continuous delivery tool for Kubernetes that automates the deployment of applications from Git repositories. Flux is an open-source tool that synchronizes Kubernetes resources with Git repositories, allowing for automated deployment and management of applications.
- Which product is built using the GitOps Toolkit, a set of composable APIs and specialised tools for building GitOps-based continuous delivery systems?
	- Flux is indeed built using the GitOps Toolkit, which provides a set of composable APIs and specialized tools for building GitOps-based continuous delivery systems. The GitOps Toolkit allows developers to create custom workflows and integrations with other tools, making it an ideal choice for building complex continuous delivery pipelines. 
- In Kubernetes, which component is primarily responsible for managing a Node?
	- Kubelet is an agent that runs on each Node in a Kubernetes cluster. Its primary responsibility is to manage the Node, ensuring that it is running the expected number of containers and maintaining the desired state. Kubelet communicates with the API Server to receive instructions and report the status of the Node.
- To achieve consistent DNS naming for pods managed by a StatefulSet in Kubernetes, what additional Kubernetes resource should you use?
	- Headless Service
- In Kubernetes, which tool is specifically designed as a comprehensive service mesh to control, secure, and observe the interactions between microservices, apart from facilitating advanced traffic management?
	- Istio is an open-source service mesh that provides a comprehensive set of features for controlling, securing, and observing interactions between microservices in Kubernetes. It offers advanced traffic management capabilities, including circuit breakers, retries, and fault injection, as well as security features like mutual TLS and authentication policies.
- For how long is the Kubernetes API guaranteed to be backward compatible following a release?
	- The Kubernetes API is guaranteed to be backward compatible for at least 1 year following a release. This means that if you write code against a specific version of the API, you can expect it to continue working without changes for at least 12 months after that version's release.
- What are CloudEvents in the context of cloud computing?
	- CloudEvents is indeed a set of standards for describing event data in a common way. It provides a standardized and interoperable format for expressing events, making it easier for different systems and applications to understand and process event data.
- In the realm of Kubernetes, which tool is specifically designed for orchestrating and managing complex parallel workflows and batch jobs?
	- Argo Workflows is an open-source tool specifically designed for orchestrating and managing complex parallel workflows and batch jobs on Kubernetes. It provides a flexible way to define workflows using YAML or JSON files and supports features like conditional logic, loops, and retries. 
- What is true about Pod-to-Pod communication within the same node in Kubernetes?
	- Pods use direct, NAT-less networking for communication on the same node'. This option is incorrect because Network Address Translation (NAT) is not required for Pod-to-Pod communication within the same node. In fact, one of the benefits of this type of communication is that it does not require NAT, making it more efficient and straightforward.
- What is the reference implementation of the Open Container Initiative (OCI) runtime specification?
	- `runc` is the reference implementation of the Open Container Initiative (OCI) runtime specification. It is a lightweight, portable, and highly performant runtime environment for containers that implements the OCI runtime specification. runc provides a minimalistic and flexible way to run containers, making it an ideal choice as the reference implementation.
- What are the three pillars of observability in a software system?
	- The three pillars of observability in a software system are indeed Logs, Metrics, and Traces. Observability refers to the ability to understand the internal state of a system by analyzing its outputs. These three pillars provide different types of insights into the system's behavior: logs provide detailed records of events, metrics offer quantitative measurements of performance and health, and traces allow for the analysis of complex interactions between components.
- In the context of etcd used in Kubernetes, what is the significance of the 1.5MB size limit for etcd?
	- In the context of etcd used in Kubernetes, the 1.5MB size limit represents the recommended maximum size for an individual value stored in etcd. This means that any value stored in etcd should be less than or equal to 1.5MB in size. This limit helps prevent large values from impacting the performance and stability of the etcd cluster.
- In telemetry, which entity is primarily used to record and analyse the path that a request takes through various services in a distributed system?
	- A Trace records the entire journey of a request as it travels through various services, APIs, or components in a distributed system.
- What does the acronym OIDC stand for in the context of authentication and authorization?
	- OpenID Connect. This is an authentication protocol built on top of OAuth 2.0.
- Kubernetes Security Profiles, such as PodSecurityPolicies (PSP) and Kyverno, are essential for enforcing security standards in a Kubernetes environment. Which of the following best describes the primary function of these Kubernetes Security Profiles?
	- Kubernetes Security Profiles such as PodSecurityPolicies (PSP) and Kyverno are primarily used to define and enforce security settings at the pod level, ensuring that pods comply with specific security standards and requirements. These profiles allow administrators to control various aspects of pod behaviour, including network policies, access controls, and resource allocation.
- In a Kubernetes environment, which software is widely used as a lightweight proxy to handle the traffic management between microservices?
	- Envoy is a widely used, open-source, lightweight proxy that is designed to handle traffic management between microservices in a Kubernetes environment. It provides features such as service discovery, load balancing, and circuit breaking, making it an ideal choice for managing communication between microservices.
- What option aligns to the 'traditional' workflow of a CI/CD pipeline, where "D" in CI/CD stands for Deployment?
- In a traditional CI/CD pipeline, the workflow typically follows this order:
	1. Build: The code is compiled and packaged into a deployable artifact.
	2. Testing: The built artifact is then tested to ensure it meets the required standards.
	3. Release: The tested artifact is prepared for deployment, which may involve creating a release candidate or packaging the artifact for distribution.
	4. Deployment: The released artifact is finally deployed to production, making it available to end-users.
- In Kubernetes, which object is recommended for managing jobs that need to run multiple times according to a batch process?
	- A CronJob is a Kubernetes object that is specifically designed to manage jobs that need to run multiple times according to a batch process, such as running a job every hour or daily. It allows you to specify a schedule using a cron expression and ensures that the job runs at the specified time.
- When creating containers using multistage builds in Docker, what technique can you employ to reduce the size of the final container image?
	- Multistage builds in Docker allow you to use multiple FROM statements in your Dockerfile. Each stage can be used to perform a specific task, such as building an application or installing dependencies. By utilizing multiple build stages, you can discard unnecessary build artifacts and dependencies at each stage, resulting in a smaller final container image.
- In Kubernetes, what is a service called when it is created without a ClusterIP?
	- In Kubernetes, a headless service is created by setting the clusterIP field to None. This type of service does not have a cluster IP address and does not perform load balancing or proxying. Instead, it allows pods to be accessed directly using their pod IPs.
- In Kubernetes, which entities can utilise the immutable:true attribute to ensure that their data cannot be modified after creation?
	- In Kubernetes, ConfigMaps and Secrets can utilise the immutable:true attribute to ensure that their data cannot be modified after creation. When this attribute is set to true, any attempts to update the ConfigMap or Secret will result in an error.
- In Kubernetes, how would you enable data sharing between different cronjobs running at various times?
	- Persistent Volume Claim (PVC) allows multiple pods to access a shared storage volume, enabling data sharing between different cronjobs running at various times. By using a PVC, you can mount a persistent volume to multiple pods, allowing them to read and write data to the same storage location.
- In Kubernetes, which component is a part of the node infrastructure rather than the control plane?
	- kube-proxy runs on each node in the cluster and is responsible for maintaining network rules for Pod communication, ensuring proper load balancing across pods. It operates at the node level and doesn't directly interact with the control plane in terms of scheduling or state management.
- What is the primary purpose of kube-state-metrics in a Kubernetes cluster?
	- Kube-state-metrics is a service that generates and exposes cluster state data in Prometheus format. It does this by scraping the Kubernetes API server to gather information about the current state of the cluster, including metrics on deployments, pods, nodes, and more.
- Which statement is true about Ingress in Kubernetes in relation to the routing of traffic?
	- Ingress in Kubernetes provides a way to route external HTTP and HTTPS traffic to services within the cluster. It acts as an entry point for incoming requests and can be configured to direct traffic based on various rules, including URL paths, hostnames, and more.

