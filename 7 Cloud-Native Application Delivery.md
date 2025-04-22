---
tags: cloud-native, GitOps, Kubernetes, Argo CD, Flux, CI/CD, automation
title: Cloud-Native Application Delivery
category: KCNA Notes
topics:
  - GitOps
  - Argo CD
  - Flux
  - CI/CD
  - Automation
  - Kubernetes
  - Cloud-Native Deployment
  - Infrastructure as Code
created: 2025-03-31
updated: 2025-03-31
author: reissada
status: draft
summary: >
  This note explores modern cloud-native application delivery practices, focusing on GitOps, Argo CD, and Flux. It covers declarative infrastructure, CI/CD pipelines, automation, and best practices for Kubernetes deployments.
---

# Cloud-Native Application Delivery

## Modern Application Deployment with Cloud-Native & GitOps

- GitOps refers to application delivery with Git as a single source of truth.
- The core principals are:
	- Everything — infrastructure, configuration, and application state — is described **declaratively** (e.g., using YAML, Helm, Kustomize).
	- These declarative definitions are stored in a **version-controlled** system like Git.
		- Every change is **tracked** and **auditable**.
	- A GitOps agent (like Argo CD, Flux) **continuously reconciles** the system state with what's in Git.
		- If Git changes, the cluster updates.
		- If the cluster drifts, the agent reverts it back to the Git-defined state.
	- There’s an ongoing loop comparing:
		- The system **self-heals** by correcting drift of "what is running" and "what should be running" — ensuring consistency.
- Argo CD is a cloud-native tool for application delivery
- **GitOps** + **Argo CD** = Automated, self-healing deployments based on declarative infrastructure
- Ideal for Kubernetes environments to maintain **consistency**, **scalability**, and **reliability**
- Great hands-on tool for learning modern deployment practices and preparing for certifications like the **CKA**

### 🌐 Cloud-Native Application Delivery

- Designed for **scalable**, **resilient**, and **rapid deployment** in cloud environments.
  - **Containerization** (e.g., Docker)
  - **Microservices**
  - **Dynamic orchestration** (e.g., Kubernetes)

---

### 🔄 GitOps: Infrastructure as Code

- Uses **Git** as the single source of truth for managing Kubernetes clusters and application deployments.
- Ensures **consistent**, **reliable**, and **scalable** deployment workflows.
- Simplifies infrastructure management by using version-controlled code changes.

---

### 📦 Argo CD: Cloud-Native GitOps Tool

- A powerful tool for **continuous delivery in Kubernetes environments**.
  - Declarative application setup
  - Automatic synchronization between Git state and live cluster
  - Supports self-healing and auto-pruning of applications

---

#### 🛠️ Argo CD Setup & Deployment Example

##### 1. **Installation:**

- Create a namespace for Argo CD:  
	`kubectl create namespace argocd`
- Install using YAML from Argo’s official repo:  
	`kubectl apply -n argocd -f [ARGO_YAML_URL]`

##### 2. **Access Setup:**

- Patch components for HTTP (for simplicity in labs)

```bash
kubectl -n argocd patch configmap argocd-cmd-params-cm --type merge -p '{"data":{"server.insegure":"true"}}'
kubectl -n argocd rollout restart deployment/argocd-server
```

- Retrieve the admin password:
	- The initial admin password in Argo CD can be retrieved by **querying the `argocd-initial-admin-secret using a kubectl`** get secret command. When Argo CD is installed, it generates an initial admin user with a random password, which is stored in a Kubernetes secret named `argocd-initial-admin-secret`.

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```

- Get the service IP and login via the web UI

```bash
kubectl get svc
```

##### 3. **Deploy a Sample App (e.g., WordPress):**

- Create a new app in Argo CD with auto-sync, prune, and self-heal enabled
- Point to a GitHub repo as the source of truth
- Observe automatic deployment and synchronization

##### 4. **GitOps in Action:**

- Delete the app namespace
- Argo CD automatically reconciles and redeploys the app based on the Git repository

## Argo CD vs. Flux: GitOps Tools for Kubernetes

### 📌 Argo CD Overview

- A **GitOps continuous delivery tool** for Kubernetes.
- Supports complex deployment workflows through integration with **Argo Workflows**.
  - **Argo Workflows** is a GitOps engine that allows users to define **workflows as code and execute them on Kubernetes**. It's specifically designed to orchestrate complex workflows, including **parallel jobs**, making it an ideal choice for managing large-scale deployments on Kubernetes.
- Key Features:
  - **Sequential & Parallel Execution:** Run tasks in order or simultaneously.
  - **Advanced Automation:** Combine **CI/CD pipelines** for complex deployments.
  - **Customizable Pipelines:** Add testing stages, approval processes, or external tool integrations.

### 🔍 Argo CD Workflows

- Enables orchestration of **multi-step deployment processes**.
- Integrates with Argo Workflows for advanced pipeline management.
- Ideal for **complex deployments** requiring custom automation sequences.
- More info: [Argo Workflows Docs](https://argoproj.github.io/workflows/)

---

### ⚡ Flux Overview

- A **GitOps continuous delivery tool** maintained by the CNCF.
- Uses a **pull-based synchronization** model by default (auto-sync changes from Git).
  - It is a GitOps tool designed to ensure that the **desired state of a Kubernetes cluster, as defined in a Git repository,** is continuously synchronized with the actual cluster state. Flux automatically applies changes to the cluster when it detects differences between the repository's configuration and the current state of the cluster.
- Strongly integrated with the **GitOps Toolkit** for custom workflows.
- GitOps Toolkit is a collection of specialized APIs and controllers built specifically to manage and automate the continuous delivery of Kubernetes resources in Flux.

### 🔧 GitOps Toolkit Components

- **Source Controller:** Manages repository access.
- **Kustomize Controller:** Customizes Kubernetes manifests using Kustomize.
- **Helm Controller:** Manages Helm chart releases declaratively.
- **Notification Controller:** Sends alerts and notifications for GitOps events.

---

### 🔥 Argo CD vs. Flux: Key Differences

|                           | **Argo CD**                              | **Flux**                                  |
| ------------------------- | ---------------------------------------- | ----------------------------------------- |
| **Architecture**          | Centralized control                      | Decentralized, scalable across clusters   |
| **Sync Approach**         | Manual or automatic sync                 | Always pull-based, continuous sync        |
| **Customization**         | Workflow-based pipelines with automation | Highly customizable via GitOps Toolkit    |
| **Community Integration** | Strong CNCF support                      | Deeper integration with CNCF projects     |
| **Best Use Case**         | Complex deployment workflows             | Continuous sync across large environments |

---

### ✅ Which Should You Use?

- **Choose Argo CD** if you need:
  - Complex workflows
  - Fine-grained control over deployment steps
  - Integration with approval processes and testing
- **Choose Flux** if you need:
  - Continuous, automatic synchronization from Git
  - Scalability across large systems with decentralized control
  - Flexibility through modular components (GitOps Toolkit)

Understanding both tools is beneficial for the **KCNA exam** and practical Kubernetes deployments. Knowing when to use Argo CD or Flux can help optimize your GitOps strategy. 🚀

---

## Deployment types
### Push-based Deployment

![[7 Cloud-Native Application Delivery Push deployment.png]]

- A CI/CD pipeline or a developer **pushes changes** directly to the cluster.
- The pipeline executes commands (e.g., `kubectl apply`) to update the environment.
- It is necessary to expose the cluster credentials
#### ✅ Pros:
- **Simple and familiar**: Easy to implement with Jenkins, GitLab CI, etc.
- **Immediate control**: You know exactly when and what is being deployed.

#### ❌ Cons:
- **Security risk**: CI/CD system needs direct access to the cluster.
- **Harder to track drift**: No continuous reconciliation.
- **Less GitOpsy**: Git isn't the sole source of truth — the actual state may drift.
### Pull-based Deployment

![[7 Cloud-Native Application Delivery Pull-based deployment.png]]

- A GitOps agent (like **Argo CD** or **Flux**) **runs inside the cluster**.
- It **pulls changes** from the Git repo and applies them to the cluster.
- The agent continuously reconciles the cluster state with the Git repo.

#### ✅ Pros:
- **Secure**: No need to expose the cluster externally.
- **True GitOps**: Git is the source of truth.
- **Automatic self-healing**: Detects and fixes drift.
- **Auditability**: Every change is traceable in Git.

#### ❌ Cons:
- **Initial setup complexity**.
- **Slight delay** in applying changes, depending on the sync frequency (usually seconds).

| Feature                | Push-Based               | Pull-Based (GitOps)    |
| ---------------------- | ------------------------ | ---------------------- |
| Trigger                | CI/CD pipeline           | GitOps agent           |
| Access to Cluster      | Required externally      | Internal only          |
| Git as Source of Truth | Not guaranteed           | Yes                    |
| Drift Detection        | Manual or none           | Automatic              |
| Security               | Weaker (cluster exposed) | Stronger (agent pulls) |
| Common Tools           | Jenkins, GitLab CI       | Argo CD, Flux          |

### CI/CD with GitOps

![[7 Cloud-Native Application Delivery Continuous monitoring.png]]
- In GitOps, we use 2 repositories:
	- Application code repository
	- Kubernetes manifests repository
- In the production cluster, the ArgoCD continuously monitors the Kubernetes repository looking for changes.

## Failed Questions

- What is the key advantage of using GitOps for managing infrastructure changes?
	- Greater efficiency and accuracy
- How does GitOps ensure that changes to Kubernetes objects are deployed to the live environment?
	- By using a GitOps operator/tool like Flux or Argo CD
- Which of the following GitOps tools is commonly used for automating the deployment process?
	- Flux
- What is the main advantage of using Git as the single source of truth for both infrastructure and application resources?
	- It tracks changes to infrastructure and application resources as code.
- How can a developer update the Kubernetes manifest repository after pushing a newly built image to the container registry?
	- Modify the YAML files referencing the image and commit the changes
- How can a pull-based approach support a multi-tenant model in Kubernetes?
	- By distributing tools across namespaces with distinct access rights and corresponding git repositories
- Which of the following is a benefit of a pull-based deployment approach?
	- Reduced risk of unauthorized access or malicious attacks
- Which GitOps tool covers the entire CI/CD process and is relatively simple to deploy?
	- Jenkins X
- What is FluxCD primarily focused on?
	- Continuous delivery to a Kubernetes cluster
- What is the main challenge faced by the software company in delivering new features to users before implementing a CI/CD process?
	- Slow and error-prone deployment
