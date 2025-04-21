---
tags: cloud-native, Kubernetes, KCNA, certification
title: Cloud Native Concepts
category: KCNA Notes
topics:
  - Cloud Native
  - Kubernetes
  - CNCF
  - Linux Foundation
  - KCNA Exam
created: 2025-03-31
updated: 2025-03-31
author: reissada
status: draft
---

# Cloud Native Concepts

## What is Cloud Native?

### **Definition**

- Combines "cloud" (inclusive of public, private, and hybrid cloud environments) and "native" (designed for or built into a given system to enable scalability, resilience, and manageability). #cloud-native
- Represents a collection of philosophies rather than a binary state.

### **Philosophies**

- **Cloud Native Architecture**: Applications are built with best practices to run across diverse cloud environments. #architecture #cloud-native
- **Cloud Native Culture**: Promotes organizational changes and optimal practices for design and implementation. #devops #culture

### Benefits of Cloud Native

- Aligning with cloud-native practices ensures applications:
  - Utilize cloud infrastructure effectively. #cloud-infrastructure
  - Achieve resilience, scalability, automation, and security. #resilience #scalability #automation #security
  - Progress beyond basic cloud hosting.
  - Built using **Infrastructure-as-Code** tools like Terraform for vendor-agnostic management. #IaC #Terraform
  - Benefits:: Scalability, Resilience, Automation, Security, Infrastructure-as-Code (Terraform)

### Misconceptions About Cloud Native

- Running in the cloud doesn’t inherently make an application cloud native. #misconceptions
- Using containers is beneficial but doesn’t automatically qualify an application as cloud native unless best practices are followed. They are a positive step toward Cloud Native. #containers

### Key Practices in Cloud Native Development

- **Automation**: Setup and delivery processes are automated. #automation
- **Resilience**: Applications are designed to handle failures gracefully. #resilience
- **Scalability**: Workloads adapt automatically to operational demands. #scalability
- **Security by Default**: Applications ensure robust security measures. #security

### Challenges Addressed by Cloud Native

- Single system errors. #fault-tolerance
- Host resource limitations. #performance
- Issues like "noisy neighbors" in shared environments. #multitenancy

### Ecosystem and Governance

- **Linux Foundation**:
  - Established in 2000 to standardize Linux and promote its adoption. #LinuxFoundation
  - Sponsors and promotes significant open-source projects, including **Kubernetes**, the **Linux Kernel**, and CNCF. #opensource #Kubernetes #CNCF
  - Provides education for **CKA** and **CKAD**. #certification #CKA #CKAD
  - Drives the open cloud movement.

- **Cloud Native Computing Foundation (CNCF)**:
  - Founded in 2015 to promote cloud-native technologies. #CNCF
  - In February 2015, Kubernetes reached its first major milestone with release 1.0. #Kubernetes
  - Oversees hundreds of projects like **Kubernetes**.
  - Plays a critical role in the cloud-native ecosystem. #cloud-native

## Open Standards

![[Kubernetes-Open-Standards.png]]

- Kubernetes Open Standard interface

| Acronym | Full Name                   | Purpose                                                                                        | Example Implementations                         |
| ------- | --------------------------- | ---------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| **CRI** | Container Runtime Interface | Allows Kubernetes to interact with **container runtimes**                                      | containerd, CRI-O, Docker (via shi              |
| **CSI** | Container Storage Interface | Enables dynamic provisioning and management of **storage volumes**                             | Portworx, Ceph, AWS EBS, GCE                    |
| **OCI** | Open Container Initiative   | Defines **container image & runtime standards**, ensuring compatibility                        | Docker, Podman, contai                          |
| **SMI** | Service Mesh Interface      | Provides a standard interface for **service meshes** to work with K8s                          | Istio, Linkerd, C                               |
| CNI     | Container Network Interface | **Specification and set of libraries** for configuring network interfaces in Linux contain Calico, Flannel, Cillium, Weave, Canal, Multus anal,  |
### 🔍 Details

#### 🧩 **CRI – Container Runtime Interface**

- Interface between Kubernetes **kubelet** and **container runtime**.
- Allows K8s to use multiple runtimes (e.g., containerd, CRI-O).
- Standardizes operations like **run, stop, remove** containers.

👉 **Spec**: [kubernetes/cri](https://github.com/kubernetes/cri-api)

---

#### 📦 **CSI – Container Storage Interface**

- Abstracts away volume plugin logic from Kubernetes core. 
- Enables third-party storage providers to **plug into K8s** without modifying the core.
- Supports **provisioning, attaching, mounting, and snapshotting** volumes.

👉 **Spec**: [container-storage-interface/spec](https://github.com/container-storage-interface/spec)

---

#### 📐 **OCI – Open Container Initiative**

- Set of standards for **container image formats** and **runtimes**. 
- Ensures tools like Docker, Podman, containerd, and CRI-O can work with the same images.
- Provides **interoperability** across tools and platforms.

👉 **Website**: [opencontainers.org](https://opencontainers.org)

---

#### 🔗 **SMI – Service Mesh Interface**

- Standard API for **observability, traffic management, and policy** in service meshes. 
- Simplifies working with different service mesh providers.
- Still a **young and evolving spec**, not part of Kubernetes core.

👉 **Website**: [servicemeshinterface.io](https://servicemeshinterface.io)

#### 🌐 **CNI** = Container Network Interface

- CNI (spec) is a **specification and set of libraries** for configuring network interfaces in Linux containers. 
- It allows Kubernetes to **plug in different networking solutions** to provide **network connectivity** to pods.
- CNI Plugin is the actual implementation that follows the CNI spec.

## KCNA Exam

### Overview

- **KCNA** is a gateway certification to the **Kubernetes** and **cloud-native** ecosystem. #KCNA #certification #Kubernetes
  - KCNA → CKA → CKAD
- It provides foundational knowledge that aligns with professional careers and acts as a precursor to other certifications like:
  - **CKA** (Certified Kubernetes Administrator) #CKA
  - **CKAD** (Certified Kubernetes Application Developer) #CKAD

### Highlights of KCNA

- Covers **Kubernetes** and diverse areas such as:
  - **Security** (leading to the Certified Kubernetes Security Specialist or CKS). #security #CKS
  - **Telemetry and Observability** (a foundation for Prometheus certification). #observability #Prometheus
  - Other areas being developed into new qualifications.
- Designed to create a strong foundation for more advanced certifications. #certification

### Exam Details

- **Format**: Multiple-choice. Entry-level certification exam with a theoretical multiple-choice format. #exam
- **Mode**:
  - In-person at test centers.
  - Remote with proctor monitoring via screen and cameras.

### Why Choose KCNA?

- Offers a diverse and comprehensive curriculum.
- Best way to check changes in the KCNA exam: [KCNA Curriculum GitHub](https://github.com/cncf/curriculum) #KCNA #certification
- The KCNA curriculum includes **GitOps** and **Prometheus**. #GitOps #Prometheus
- Prepares candidates for advanced Kubernetes certifications. #Kubernetes #CKA #CKAD
- Builds practical knowledge applicable to cloud-native professional roles.
- Can be pursued remotely, with a friendly and supportive exam administration team.

### Pro Tip

- Follow the **Linux Foundation** and **CNCF** on social media to stay informed about sales, events, and updates. #LinuxFoundation #CNCF
- **Exam Booking**: [Kubernetes Cloud Native Associate (KCNA)](https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate)
- **Discount Code**: DIVEINTO30
  - While I recommend keeping an eye out for occasional discounts from the Linux Foundation—sometimes up to 40-45%—this code is a reliable backup if needed.
- For the **KCNA** examination, focus on:
  - **Infrastructure as Code (IaC)** - Understand its meaning and connection to Cloud Native principles. #IaC
  - The roles of **Speed, Efficiency, and Cost** in Cloud Native environments. #performance #cost #efficiency
- To reduce exam costs, wait for sales, attend events like **KubeCon/CloudNativeCon**, and check for discount codes on social media. #KubeCon
- The GitHub repository under cncf/curriculum is the official source for the **KCNA** exam curriculum. It is regularly updated by the CNCF, making it the best place to find the latest information on exam topics, objectives, and changes. #KCNA

## Failed Questions

- When did Kubernetes join the Cloud Native Computing Foundation (CNCF)?
	- 2016
- How are SIG leaders elected in Kubernetes?
	- They are appointed by a nomination and voting process within the SIG.
- What is the role of Working Groups in the Kubernetes community?
	- Come together to work on specific issues of the project
- How do KEPs address the limitations of using GitHub issues for proposing changes?
	- By enabling efficient management of proposed changes, by promoting transparency and collaboration, by providing a structured and well-defined process.
- What is the purpose of a test plan in a KEP?
	- To ensure that the proposed change is thoroughly tested before implementation
- What is the purpose of a Kubernetes Enhancement Proposal (KEP)?
	- To provide a structured process for proposing, evaluating, and implementing changes in Kubernetes
- What is the Open Container Initiative (OCI) and what standards has it created for container images and runtimes?
	- It is a group that focuses on creating open standards for container images, runtimes, and distributions. It has created the image spec, the runtime specification, and the distribution spec.
- Which standard created by the OCI outlines how a file system bundle should be packaged into an image?
	- Image spec
- What is pod-based scaling in Kubernetes autoscaling?
	- Adjusting the number of pods running in a deployment based on demand
- Which of the following is NOT one of the autoscaling features offered by Kubernetes?
	- Node Autoscaler
- What is the purpose of Horizontal Pod Autoscaler in Kubernetes autoscaling?
	- To adjust the number of pods running in a deployment based on demand
- Which auto scaling feature of Kubernetes allows businesses to adjust the number of pods running in a deployment based on demand?
	- Horizontal Pod Autoscaler
- Which auto scaling feature of Kubernetes allows businesses to adjust the resource allocation of individual pods based on demand?
	- Vertical Pod Autoscaler
- Which auto scaling feature of Kubernetes allows businesses to automatically resize the entire cluster to meet the demands of the application?
	- Cluster Autoscaler
- Which section of a KEP outlines the motivation and goals of a proposed change or feature?
	- Goals and non-goals
- What is serverless computing?
	- A computing model where the server is managed by a cloud provider
- What is Function-as-a-Service (FaaS)?
	- A model where the cloud provider manages the server infrastructure
- What flexibility does the Container Runtime Interface (CRI) provide in Kubernetes?
	- It empowers users to select the optimal container runtime that suits their specific needs.
- Which cloud providers offer FaaS?
	- AWS, Microsoft Azure, Google Cloud, and IBM Cloud