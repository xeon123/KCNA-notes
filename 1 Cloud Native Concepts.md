---
tags: cloud-native
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
