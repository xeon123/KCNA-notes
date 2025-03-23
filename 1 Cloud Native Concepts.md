# What is Cloud Native?

- **Definition**:
    - Combines "cloud" (inclusive of public, private, and hybrid cloud environments) and "native" (designed for or built into a given system to enable scalability, resilience, and manageability).
    - Represents a collection of philosophies rather than a binary state.
- **Philosophies**:
    - **Cloud Native Architecture**: Applications are built with best practices to run across diverse cloud environments.
    - **Cloud Native Culture**: Promotes organizational changes and optimal practices for design and implementation.

## Benefits of Cloud Native

- Aligning with cloud native practices ensures applications:
    - Utilize cloud infrastructure effectively.
    - Achieve resilience, scalability, automation, and security.
    - Progress beyond basic cloud hosting.
    - Built using Infrastructure-as-Code tools like Terraform for vendor-agnostic management.

## Misconceptions About Cloud Native

- Running in the cloud doesn’t inherently make an application cloud native.
- Using containers is beneficial but doesn’t automatically qualify an application as cloud native unless best practices are followed. They are a positive step towards Cloud Native.

## Key Practices in Cloud Native Development

- **Automation**: Setup and delivery processes are automated.
- **Resilience**: Applications are designed to handle failures gracefully.
- **Scalability**: Workloads adapt automatically to operational demands.
- **Security by Default**: Applications ensure robust security measures.

## Challenges Addressed by Cloud Native

- Single system errors.
- Host resource limitations.
- Issues like "noisy neighbors" in shared environments.

## Ecosystem and Governance

- **Linux Foundation**:
    - Established in 2000 to standardize Linux and promote its adoption. Merger between Open source Delevopment Labs and Free Standards Group.
    - Sponsors and promotes significant open-source projects, including Kubernetes or linux kernel, which underpins moderns OS, or CNCF.
    - Provides education to CKA and CKAD.
    - Drive open cloud movement.
- **Cloud Native Computing Foundation (CNCF)**:
    - Founded in 2015 to promote cloud native technologies.
        - In February 2015, k 8 s reached fist major milestone with release 1.0.
    - Oversees hundreds of projects like Kubernetes.
    - Plays a critical role in the cloud native ecosystem.

# KCNA Exam

#### Overview

- **KCNA** is a gateway certification to the Kubernetes and cloud-native ecosystem.
	- KCNA => CKA => CKAD
- It provides foundational knowledge that aligns with professional careers and acts as a precursor to other certifications like:
    - **CKA** (Certified Kubernetes Administrator)
    - **CKAD** (Certified Kubernetes Application Developer)

#### Highlights of CCNA

- Covers **Kubernetes** and diverse areas such as:
    - **Security** (leading to the Certified Kubernetes Security Specialist or CKS).
    - **Telemetry and Observability** (a foundation for Prometheus certification).
    - Other areas being developed into new qualifications.
- Designed to create a strong foundation for more advanced certifications.

#### Exam Details

- **Format**: Multiple-choice. Entry-lever certification exam with theoretical multiple-choice format.
- **Mode**:
    - In-person at test centers.
    - Remote with proctor monitoring via screen and cameras.

#### Why Choose CCNA?

- Offers a diverse and comprehensive curriculum.
	- Best way to check changes in the KCNA exam: https://github.com/cncf/curriculum
		- The KCNA curriculum include GitOps and Prometheus.
- Prepares candidates for advanced Kubernetes certifications.
- Builds practical knowledge applicable to cloud-native professional roles.
- Can be pursued remotely, with a friendly and supportive exam administration team.

#### Pro Tip

- Follow the **Linux Foundation** and **CNCF** on social media to stay informed about sales, events, and updates.
	- **Exam Booking**: [Kubernetes Cloud Native Associate (KCNA)](https://training.linuxfoundation.org/certification/kubernetes-cloud-native-associate)  
		**Discount Code**: DIVEINTO30
While I recommend keeping an eye out for occasional discounts from the Linux Foundation - sometimes up to 40-45% - this code is a reliable backup if needed.
- For the KCNA examination focus on:
	- IAC - Understand the meaning of Infrastructure as Code and its connection to Cloud Native principles.
	- The roles of Speed, Efficiency, and Cost in the context of Cloud Native environments
- To reduce the cost of the exams wait for sales, attend events like KubeCon/CloudNativeCon, and checking for discount codes on social media.
- The GitHub repository under cncf/curriculum is the official source for the KCNA exam curriculum. It is regularly updated by the CNCF, making it the best place to find the latest information on exam topics, objectives, and changes.