# Miscellaneous

## Commands

### Namespaces

```bash
kubectl get namespaces
kubectl create namespace new-namespace-name
```

### Pods

```bash
**kubectl create -f def-pod.yaml**
**$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80**
**$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \  
--dry-run=client -o yaml > nginx-pod.yaml**
**$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \  
--dry-run=client -o json > nginx-pod.json**
**$ kubectl create -f nginx-pod.yaml  
$ kubectl create -f nginx-pod.json**
**$ kubectl apply -f nginx-pod.yaml  
$ kubectl get pods  
$ kubectl get pod nginx-pod -o yaml  
$ kubectl get pod nginx-pod -o json  
$ kubectl describe pod nginx-pod  
$ kubectl delete pod nginx-pod**
```

### ReplicaSets

```bash
**$ kubectl create -f redis-rs.yaml**
**$ kubectl apply -f redis-rs.yaml  
$ kubectl get replicasets  
$ kubectl get rs  
$ kubectl scale rs frontend --replicas=4  
$ kubectl get rs frontend -o yaml  
$ kubectl get rs frontend -o json  
$ kubectl describe rs frontend  
$ kubectl delete rs frontend**
```

### DeploymentSets

```bash
**$ kubectl create -f def-deploy.yaml**
**$ kubectl create deployment nginx-deployment \  
--image=nginx:1.20.2 --port=80 --replicas=3**
**$ kubectl create deployment nginx-deployment \  
--image=nginx:1.20.2 --port=80 --replicas=3 \  
--dry-run=client -o yaml > nginx-deploy.yaml**
**$ kubectl create deployment nginx-deployment \  
--image=nginx:1.20.2 --port=80 --replicas=3 \  
--dry-run=client -o json > nginx-deploy.json**
**$ kubectl create -f nginx-deploy.yaml  
$ kubectl create -f nginx-deploy.json**
**$ kubectl apply -f nginx-deploy.yaml --record  
$ kubectl get deployments  
$ kubectl get deploy -o wide  
$ kubectl scale deploy nginx-deployment --replicas=4  
$ kubectl get deploy nginx-deployment -o yaml  
$ kubectl get deploy nginx-deployment -o json  
$ kubectl describe deploy nginx-deployment  
$ kubectl rollout status deploy nginx-deployment  
$ kubectl rollout history deploy nginx-deployment  
$ kubectl rollout history deploy nginx-deployment --revision=1  
$ kubectl set image deploy nginx-deployment nginx=nginx:1.21.5 --record  
$ kubectl rollout history deploy nginx-deployment --revision=2  
$ kubectl rollout undo deploy nginx-deployment --to-revision=1  
$ kubectl get all -l app=nginx -o wide  
$ kubectl delete deploy nginx-deployment  
$ kubectl get deploy,rs,po -l app=nginx**
```

### DaemonSets

```bash
**$ kubectl create -f fluentd-ds.yaml**
**$ kubectl apply -f fluentd-ds.yaml --record  
$ kubectl get daemonsets  
$ kubectl get ds -o wide  
$ kubectl get ds fluentd-agent -o yaml  
$ kubectl get ds fluentd-agent -o json  
$ kubectl describe ds fluentd-agent  
$ kubectl rollout status ds fluentd-agent  
$ kubectl rollout history ds fluentd-agent  
$ kubectl rollout history ds fluentd-agent --revision=1  
$ kubectl set image ds fluentd-agent fluentd=quay.io/fluentd_elasticsearch/fluentd:v4.6.2 --record  
$ kubectl rollout history ds fluentd-agent --revision=2  
$ kubectl rollout undo ds fluentd-agent --to-revision=1  
$ kubectl get all -l k8s-app=fluentd-agent -o wide  
$ kubectl delete ds fluentd-agent  
$ kubectl get ds,po -l k8s-app=fluentd-agent**
```
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
- Which open-source tool is specifically designed for assessing the security posture of Kubernetes clusters according to NSA and CISA guidelines?
	- KubeScape is an open-source tool that is specifically designed to assess the security posture of Kubernetes clusters based on guidelines from the National Security Agency (NSA) and Cybersecurity and Infrastructure Security Agency (CISA). It provides a comprehensive risk assessment and identifies potential vulnerabilities in the cluster configuration.
- In Kubernetes, which object is most suitable for deploying stateless applications?
	- A Deployment is an object that manages a set of replicas of a Pod, which makes it suitable for deploying stateless applications. Deployments provide features like rolling updates and self-healing, making them ideal for managing multiple instances of a stateless application.
- In a cloud-native organisation, which role is primarily responsible for building and maintaining the underlying platform infrastructure, enabling application developers to deploy and run their services efficiently?
	- Platform Engineers
- In a Kubernetes cluster, which component is responsible for routing traffic for services and managing IP rules?
	- Kube-Proxy is a network proxy that runs on each node in a Kubernetes cluster and is responsible for routing traffic for services and managing IP rules. It ensures that incoming requests are directed to the correct pod, even if the pod is running on a different node.
- In Kubernetes, which security-focused tool is commonly used for runtime security monitoring and detection of anomalous activities within containers and pods?
	- Falco is a cloud-native, open-source security tool that provides runtime security monitoring and detection of anomalous activities within containers and pods in Kubernetes environments. It uses system calls to monitor container activity and can detect unusual behavior such as privilege escalation or unauthorized access.
- Which function is primarily associated with Kubernetes Security Contexts?
	- Kubernetes Security Contexts are primarily associated with defining container or pod level security settings. A Security Context defines the security settings for a container or pod, such as running a container as a specific user, setting file system permissions, and configuring SELinux labels. These settings can be applied to a single container or to all containers in a pod.
- In Kubernetes, what is the relationship between Deployments and ReplicaSets?
	- In Kubernetes, when you create a Deployment, it automatically creates a ReplicaSet to manage the desired number of pod replicas. The ReplicaSet ensures that the specified number of replicas are running and available, while the Deployment manages the rollout of new versions of the application.
- In Kubernetes security, what differentiates Security Contexts from Security Policies (Like PodSecurityPolicies or Kyverno) in terms of their scope and focus? 
	- Security Contexts are settings defined inside a pod or container spec. Scope: single pod or container.
	- Security Policies are cluster-level or namespace-level enforcement tools. Scope: all pods in a cluster or namespace.
- In Kubernetes, which feature is effective for managing costs by allowing for the categorisation and organisation of resources?
	- Labels are key-value pairs attached to objects, such as pods or services, and provide a flexible way to categorize and organize resources. By using labels, you can select and manage groups of resources based on specific criteria, making it easier to manage costs by allocating resources to specific teams, projects, or environments.
- In the context of cloud native environments, which persona primarily focuses on optimizing cloud costs and financial management?
	- FinOps (Financial Operations) is a relatively new persona that has emerged in cloud native environments. They are primarily responsible for optimizing cloud costs, managing financial resources, and ensuring that the organization gets the best possible return on investment from its cloud spending. FinOps teams work closely with other stakeholders to identify areas of cost optimization, implement cost-saving measures, and develop strategies for financial management.
- Which of the following is a lightweight container runtime specifically designed for Kubernetes, conforming to the Container Runtime Interface (CRI)?
	- CRI-O is a lightweight container runtime that's specifically designed for Kubernetes and conforms to the Container Runtime Interface (CRI). It provides a minimalistic approach to running containers in a Kubernetes environment, making it an ideal choice for this ecosystem. It is not LXC, because LXC is a userspace interface for the Linux kernel containment features. While LXC does provide containerization capabilities, it's not specifically designed for Kubernetes or conforming to CRI.
- Which of the following container runtimes is a lightweight, low-level runtime designed specifically for running containers in Linux?
	- LXC
- What is the difference between LXC and CRI-O?
	- LXC is a lightweight virtualization technology for running multiple isolated Linux systems (containers) on a single control host. CRI-O is a container runtime specifically built for Kubernetes.
- In cloud computing, which approach is best suited for simultaneously managing security threats and optimising costs by identifying unusual activities?
	- Cloud Anomaly Detection is a approach that involves monitoring and analyzing cloud-based systems to identify unusual patterns of activity that could indicate potential security threats. By detecting anomalies, organizations can take proactive measures to mitigate risks and optimize costs associated with managing security threats.
- When a Pod is created in Kubernetes without specifying a Service Account, which default Service Account is used?
	- default service account
- In the field of IT operations and software development, SRE stands for a discipline that emphasizes automation, continuous improvement, and a balanced approach to system reliability and features. What does SRE stand for?
	- Site reliability engineering
- In Kubernetes, how are the container specifications in Deployments and StatefulSets similar?
	- Both Deployments and StatefulSets use the same container specification within their pod templates. The key difference between these two resources is in how they manage the lifecycle and ordering of pods, but the container specification within the pod template remains consistent for all replicas in both cases. 
- What is the primary function of a PDB?
	- Maintaining a minimum number of available replicas during voluntary disruptions. Pod Disruption Budgets (PDBs) were introduced in Kubernetes to protect against voluntary disruptions, such as rolling updates or scaling. Their primary function is to maintain a minimum number of available replicas during these disruptions, ensuring that a certain level of service availability is always maintained.
- When the kubectl expose command is used in Kubernetes, which component is created?
	- Service. When you run kubectl expose, it creates a Service, which is a Kubernetes resource that provides a network identity and load balancing for accessing one or more pods. The expose command takes an existing pod or replication controller and creates a new Service that exposes the specified port.
- What is the default update strategy used by Kubernetes Deployments for rolling out updates?
	- RollingUpdate. RollingUpdate is the default update strategy used by Kubernetes Deployments for rolling out updates. It allows for a gradual rollout of new versions while maintaining availability and minimizing downtime. With RollingUpdate, new pods are created with the updated version, and old pods are terminated once the new ones are available. 
- Which Kubernetes component is responsible for exposing applications running on a set of Pods as a network service?
	- A Service in Kubernetes is a resource that provides a stable IP address and DNS name for accessing an application running on a set of Pods. It acts as a network proxy, routing traffic to the appropriate Pod, allowing applications to be exposed to the outside world.
- Which kubectl command is used to list all the API resources, such as Pods, Services, and Deployments, available in a Kubernetes cluster?
	- kubectl api-resources is used to list all the API resources, such as Pods, Services, and Deployments, available in a Kubernetes cluster. It provides a comprehensive list of all the resources that can be managed using the Kubernetes API.
- What is OPA?
	- The Open Policy Agent (OPA) is a policy engine that can be integrated with Kubernetes to validate requests and apply policies across the cluster. OPA provides a unified way to define and enforce policies across different layers of the stack, including network policies, admission control, and more. 
- In Kubernetes, what does Service Discovery refer to?
	-  Service Discovery refers to the process by which services can locate and communicate with each other on the network. This is achieved through the use of Services, which provide a stable IP address and DNS name for accessing pods that are running in the cluster. This allows services to be decoupled from specific pod instances and enables communication between services.
- In a Pod, which Linux namespace do containers usually share?
	- Containers in a Pod usually share the same network namespace, which allows them to communicate with each other using localhost or the pod's IP address. This sharing of the network namespace enables containers to work together seamlessly and provides a simplified networking model.
- In Kubernetes, what modification is made to a ClusterIP service to create a headless service?
	- To create a headless service in Kubernetes, you need to set the clusterIP field of the Service object to 'None'. This tells Kubernetes not to allocate an IP address for the Service, effectively making it headless. A headless service allows you to access pods directly without going through a load balancer or proxy.
- In Kubernetes, which feature is crucial for managing distributed stateful applications to avoid conflicts like "split-brain" scenarios?
	-  StatefulSets are a type of controller in Kubernetes that provides guarantees about the deployment and management of stateful applications. They ensure that each replica has a unique identity and that the application can maintain its state even in the event of failures or network partitions, thus avoiding "split-brain" scenarios.
- Which Linux feature is utilised by Kubernetes for container isolation, limits the resource usage of a process or a set of processes?
	- cgroups (control groups) is a Linux feature that provides a way to limit, account, and isolate the resource usage of a process or a set of processes running on a system. Kubernetes utilizes this feature for container isolation, ensuring that each container has its own isolated environment with limited resources.
- In a cloud-native environment, which persona is typically responsible for the creation and execution of an incident management procedure?
	- A Site Reliability Engineer (SRE) is typically responsible for ensuring the reliability and uptime of a cloud-native environment. As part of this role, they are often involved in creating and executing incident management procedures to minimize downtime and ensure rapid recovery in case of an outage or other incidents.
- What tool is commonly used for installing and managing applications in a Kubernetes cluster?
	- Helm is a package manager for Kubernetes that simplifies the process of installing and managing applications in a Kubernetes cluster. It provides a way to easily install, upgrade, and manage applications using pre-configured packages called charts. It is not kubectl. Kubectl is a command-line tool for interacting with a Kubernetes cluster, but it's not specifically designed for installing and managing applications. While it can be used to deploy applications using YAML or JSON files, it requires more manual effort and doesn't provide the same level of package management as Helm.
- During the phase when container images are being downloaded for a Pod in Kubernetes, what state is the Pod typically in?
	- Pending State. When a Pod is in the Pending state, it means that the Pod has been created but at least one of its containers is still waiting for resources to be allocated or is still downloading its image. This is typically the case when container images are being downloaded for a Pod.
- Which Kubernetes autoscaling solution has the capability to scale workloads down to zero pods?
	- Kubernetes Event-Driven Autoscaling (KEDA) is an autoscaling solution that allows you to scale your workloads down to zero pods based on events such as message queue lengths or HTTP requests. KEDA uses a pull-based approach, where it periodically checks for new events and scales accordingly.
- How does KEDA enable event-driven autoscaling in Kubernetes, and what custom resource does it utilise for this purpose?
	- KEDA enables event-driven autoscaling in Kubernetes by scaling applications based on external metrics, such as message queue lengths or HTTP requests. It utilizes a custom resource called ScaledObjects for this purpose. ScaledObjects define how to scale an application based on these external metrics.
- When using kubectl apply in Kubernetes, what is a potential drawback related to tracking the state of resources?
	- If you remove resources from the applied manifest, kubectl apply may not track them and can inadvertently delete those resources. If you remove resources from the applied manifest and reapply it, kubectl apply may interpret the missing resources as no longer necessary and will delete them from the cluster. This behavior is particularly important in situations where a manifest was pruned or modified without intending to delete resources from the cluster.
- Which Kubernetes component interacts with the Container Runtime Interface (CRI)?
	- The Kubelet interacts with the Container Runtime Interface (CRI) to manage container runtime operations, such as creating and deleting containers. The CRI provides a standardized interface between the Kubernetes node agent (Kubelet) and the container runtime.
- Which of the following container runtimes are recognized for providing enhanced security features, such as stronger isolation through virtualization?
	- Kata Containers and gVisor are recognized for providing enhanced security features through virtualization. They use a combination of containerization and virtualization to provide stronger isolation between containers. Kata Containers uses a lightweight virtual machine (VM) to run each container, while gVisor uses a user-space kernel to provide a sandboxed environment for containers.
- What are common components found in a service mesh implementation?
	- In a service mesh implementation, there are two primary components: the data plane and the control plane. The data plane refers to the proxies that intercept and manage traffic between services, while the control plane manages the configuration and behaviour of the data plane. Examples of service meshes include Istio, Linkerd, and Envoy.
- In Kubernetes, what is a Service primarily used for?
	- In Kubernetes, a Service is primarily used for exposing an application running on a set of Pods as a network service. It provides a stable network identity and load balancing for accessing the application, allowing clients to access the application without needing to know the individual IP addresses of the Pods.
- What are the primary modes of service discovery within a Kubernetes cluster?
	- Environment Variables and DNS are the primary modes of service discovery within a Kubernetes cluster. When a pod is created, Kubernetes automatically configures environment variables for the pod's container that point to other services running in the same namespace. Additionally, Kubernetes provides a built-in DNS service that allows pods to discover and communicate with each other using DNS names.
- What are the default namespaces in a Kubernetes installation?
	- default, kube-system, kube-public, kube-node-lease. In a Kubernetes installation, there are four default namespaces that are created during the cluster setup process. These namespaces are 'default', 'kube-system', 'kube-public', and 'kube-node-lease'. The 'default' namespace is where user-deployed applications typically run. The 'kube-system' namespace contains objects created by the Kubernetes system, such as the API server, controller manager, and scheduler. The 'kube-public' namespace is used for cluster-wide information that can be safely exposed publicly without authentication. The 'kube-node-lease' namespace is used for node lease objects.
## Links

https://glossary.cncf.io/