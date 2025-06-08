---
tags: Kubernetes, API, RBAC, CRDs, Helm, StatefulSets, Service Mesh
title: Kubernetes Deep Dive
category: KCNA Notes
topics:
  - Kubernetes API
  - RBAC
  - CRDs
  - Helm
  - StatefulSets
  - Service Mesh
  - Network Policies
  - Pod Disruption Budgets
  - Security
  - Storage
created: 2025-03-31
updated: 2025-03-31
author: reissada
status: draft
summary: >
  This note provides an in-depth exploration of Kubernetes, covering the API, RBAC, CRDs, Helm, StatefulSets, Service Meshes, Network Policies, Pod Disruption Budgets, Security, and Storage. It also includes practical examples and best practices for managing Kubernetes clusters.
---

# Kubernetes API

![[Kubernetes API Architecture3.png]]

## Key Interactions with the Kubernetes API

1. **Users & Tools**: Users interact via `kubectl`, Helm, and client libraries.
2. **Monitoring**: Tools query the API to track cluster health.
3. **Internal Components in Kubernetes**:
   - **Kube-scheduler**: Uses the API to schedule Pods on Nodes.
   - **Kubelet**: Reports Node and Pod statuses and receives instructions.
   - **Controller Manager**: Maintains the cluster’s desired state.
   - **Admission Controller**: Kube API can use admission controller to enforces policies (e.g., quotas, validation) and modify resources.
   - set quotas, set defaults, validate configurations, and perform other tasks.
   - useful to set namespace to not exceed a given threshold of memory
   - can accept or reject requests accordingly.

## API Request Processing Flow

```mermaid
flowchart LR
    classDef smallText font-size:10px, padding:1px;
    linkStyle default stroke-width:1px;
    request["Request Arrival"]:::smallText --> route["Route Matching"]:::smallText
    route --> authn["Authentication"]:::smallText
    authn --> authz["Authorization"]:::smallText
    authz --> admission["Admission Controller"]:::smallText
    admission --> validation["Validation"]:::smallText
    validation --> handling["Request Handling"]:::smallText
    handling --> response["Response Generation & Sending"]:::smallText
```

1. **Request Arrival**: API receives HTTPS requests (typically on port `6443`).
2. **Route Matching**: API server determines request type (GET, POST, PUT, DELETE, etc.).
3. **Authentication**: Verifies identity via API keys or tokens to validate the requests.
4. **Authorization**: Checks permissions based on RBAC, Webhooks, etc. Are these permissions allowed (relates to the `--authorization-mode api-server` parameter). If the parameter is not set, it will default to `AlwaysAllow` - **allow all requests without any restrictions**.
5. **Admission Controller**: Applies policies to validate user requests (e.g., resource limits).
   * Enforce policies and validate requests before they are accepted by the API server.
   * In this case, enforcing a policy that a certain namespace doesn't exceed a given memory threshold is a potential use case for an Admission Controller. This would involve writing a custom admission controller that checks the total memory usage of all pods in a namespace against a predefined limit.
1. **Validation**: Ensures request data is well-formed.
2. **Request Handling**: Executes the operation (e.g., persisting data in `etcd`).
3. **Response Generation & Sending**: Returns HTTP status and response body.

The path of API routing is

1. Authentication: Such as an API key and token
2. Authorization: Are these parameters allowed
3. Admission Controller

### Authentication

Kubernetes does not store user objects or usernames internally, but it still supports **authentication** using usernames. There are two types of users:

- **Normal Users**: Managed externally (e.g., via certificates, password files, or OAuth).
- **Service Accounts**: In-cluster identities for processes, tied to namespaces, with credentials stored in Secrets.

Kubernetes supports **anonymous access**, **user impersonation**, and several **authentication methods**, including:

1. **X.509 Client Certificates** – Verified via CA file.
2. **Static Token File** – Predefined tokens passed via a file.
3. **Bootstrap Tokens** – For setting up new clusters.
4. **Service Account Tokens** – Automatically managed, used by pods.
5. **OpenID Connect Tokens** – Integrates with external OAuth2 providers.
6. **Webhook Token Authentication** – Auth offloaded to a remote service.
7. **Authenticating Proxy** – Adds custom authentication logic.

#### Create new user

Create **lfs158** namespace:

```bash
$ kubectl create namespace lfs158
```

Create the **rbac** directory and **cd** into it:

```bash
$ mkdir rbac
$ cd rbac/
```

Create a new user **bob** on your workstation, and set **bob**'s password as well (the system will prompt you to enter the password twice) :

```bash
~/rbac$ sudo useradd -s /bin/bash bob
~/rbac$ sudo passwd bob
```

Create a **private key** for the new user **bob** with the **openssl** tool, then create a **certificate signing request** for **bob** with the same **openssl** tool:

```bash
~/rbac$ openssl genrsa -out bob.key 2048

Generating RSA private key, 2048 bit long modulus (2 primes)
.................................................+++++
.........................+++++
e is 65537 (0x010001)

~/rbac$ openssl req -new -key bob.key \  
  -out bob.csr -subj "/CN=bob/O=learner"
```

Create a YAML definition manifest for a **certificate signing request** object, and save it with a blank value for the **request** field: 

```bash
~/rbac$ vim signing-request.yaml

apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: bob-csr
spec:
  groups:
  - system:authenticated
  request: <assign encoded value from next cat command>
  signerName: kubernetes.io/kube-apiserver-client  
  **usages:
  - digital signature
  - key encipherment
  - client auth
```

View the **certificate**, encode it in **base64**, and assign it to the **request** field in the **signing-request.yaml** file:

```bash
~/rbac$ cat bob.csr | base64 | tr -d '\n','%'
LS0tLS1CRUd...1QtLS0tLQo=

~/rbac$ vim signing-request.yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: bob-csr
spec:
  groups:
  - system:authenticated
  request:** **LS0tLS1CRUd...1QtLS0tLQo=
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - digital signature
  - key encipherment
  - client auth
```

Create the **certificate signing request** object, then list the certificate signing request objects. It shows a **pending** state:

```bash
~/rbac$ kubectl create -f signing-request.yaml
certificatesigningrequest.certificates.k8s.io/bob-csr created

~/rbac$ kubectl get csr
NAME      AGE   SIGNERNAME                            REQUESTOR       CONDITION
bob-csr   12s   kubernetes.io/kube-apiserver-client   minikube-user   Pending
```

Approve the **certificate signing request** object, then list the certificate signing request objects again. It shows both **approved** and **issued** states:

```bash
~/rbac$ kubectl certificate approve bob-csr
certificatesigningrequest.certificates.k8s.io/bob-csr approved
~/rbac$ kubectl get csr
NAME      AGE   SIGNERNAME                            REQUESTOR       CONDITION
bob-csr   57s   kubernetes.io/kube-apiserver-client   minikube-user   Approved,Issued
```

Extract the approved certificate from the certificate signing request, decode it with base64 and save it as a certificate file. Then view the certificate in the newly created certificate file:

```bash
~/rbac$ kubectl get csr bob-csr \
  -o jsonpath='{.status.certificate}' | \
  base64 -d > bob.crt

~/rbac$ cat bob.crt
-----BEGIN CERTIFICATE-----
MIIDGzCCA...
...
...NOZRRZBVunTjK7A==
-----END CERTIFICATE-----
```

Configure the kubectl client's configuration manifest with user bob's credentials by assigning his key and certificate: 

```bash
~/rbac$ kubectl config set-credentials bob \
  --client-certificate=bob.crt --client-key=bob.key
```

User "bob" set.

Create a new context entry in the kubectl client's configuration manifest for user bob, associated with the lfs158 namespace in the minikube cluster:

```bash
~/rbac$ kubectl config set-context bob-context \
  --cluster=minikube --namespace=lfs158 --user=bob
Context "bob-context" created.
```

View the contents of the kubectl client's configuration manifest again, observing the new context entry bob-context, and the new user entry bob (the output is redacted for readability):

```bash
~/rbac$ kubectl config view

apiVersion: v1
clusters:
- cluster:
    certificate-authority: /home/student/.minikube/ca.crt
    ...
    server: ht‌tps://192.168.99.100:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    ...
    user: minikube
  name: minikube
- context:
    cluster: minikube
    namespace: lfs158
    user: bob
  name: bob-context
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /home/student/.minikube/profiles/minikube/client.crt
    client-key: /home/student/.minikube/profiles/minikube/client.key
- name: bob
  user:
    client-certificate: /home/student/rbac/bob.crt
    client-key: /home/student/rbac/bob.key
```

### Authorization

After **successful authentication**, Kubernetes uses **authorization modules** to determine whether API requests are allowed or denied. It evaluates attributes such as:

- **User**
- **Group**
- **Resource**
- **Namespace**
- **API Group**

Authorization modes: Node, ABAC, Webhooks, and RBAC

#### RBAC Example

While in the default minikube context, create a new deployment in the lfs158 namespace:

```bash
~/rbac$ kubectl -n lfs158 create deployment nginx --image=nginx:alpine
deployment.apps/nginx created
```

From the new context bob-context try to list pods. The attempt fails because user bob has no permissions configured for the bob-context:

```bash
~/rbac$ kubectl --context=bob-context get pods
```

Error from server (Forbidden): pods is forbidden: User "bob" cannot list resource "pods" in API group "" in the namespace "lfs158"

The following steps will assign a limited set of permissions to user bob in the bob-context. 

Create a YAML configuration manifest for a pod-reader Role object, which allows only get, watch, list actions/verbs in the lfs158 namespace against pod resources. Then create the role object and list it from the default minikube context, but from the lfs158 namespace:

```bash
~/rbac$ vim role.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: lfs158
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

~/rbac$ kubectl create -f role.yaml
role.rbac.authorization.k8s.io/pod-reader created

~/rbac$ kubectl -n lfs158 get roles
NAME         CREATED AT
pod-reader   2022-04-11T03:47:45Z
```

Create a YAML configuration manifest for a rolebinding object, which assigns the permissions of the pod-reader Role to user bob. Then create the rolebinding object and list it from the default minikube context, but from the lfs158 namespace:

```bash
~/rbac$ vim rolebinding.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-read-access
  namespace: lfs158
subjects:
- kind: User
  name: bob
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io

~/rbac$ kubectl create -f rolebinding.yaml 
rolebinding.rbac.authorization.k8s.io/pod-read-access created

~/rbac$ kubectl -n lfs158 get rolebindings
NAME              ROLE              AGE
pod-read-access   Role/pod-reader   28s
```

Now that we have assigned permissions to bob, we can successfully list pods from the new context bob-context.

```bash
~/rbac$ kubectl --context=bob-context get pods

NAME                     READY   STATUS    RESTARTS   AGE
nginx-565785f75c-kl25r   1/1     Running   0          7m41s
```

### Admission Controllers

- Admission controllers in Kubernetes enforce fine-grained access policies after authentication and authorization, handling tasks like resource quota checks and enforcing default settings.
- To use admission controls, we must start the Kubernetes API server with the **--enable-admission-plugins**, which takes a comma-delimited, ordered list of controller names, such as:

```bash
--enable-admission-plugins=NamespaceLifecycle,ResourceQuota,PodSecurity,DefaultStorageClass
```

## Extending the API with CRDs

- **Custom Resource Definitions (CRDs)** allow defining new resource types.
- E.g., CRD could be used in Kubernetes to deploy and manage MySQL instances.
- Example: A company offering "Database as a Service" can create a CRD to manage databases similarly to built-in resources.

## Exploring the Kubernetes API with `kubectl`

![[kubectl-verbosity.png]]

Using `kubectl`, every request made interacts with the **Kubernetes API**, making it a convenient wrapper. For example, running `kubectl get nodes` retrieves node information, and increasing verbosity (`--v=6`) reveals the actual API request.

In this example, the request is being sent to `https://127.0.0.1:6443`. When the request is made, it is perfoming the authentication using the information in `/root/.kube/config`

### Understanding API Requests

- `kubectl` requests are sent to the **API server** (default: `https://127.0.0.1:6443/api/v1/nodes?limit=500`).
- Authentication is managed via the **kubeconfig** file.
- Directly querying the API without authentication fails.

### Proxying the API

![[kubectl-proxying request.png]]

- Running `kubectl proxy` allows local access over **HTTP** (`http://localhost:8001`) without additional authentication.
- **Proxy handles authentication with the API server** by using existing authentication configuration (e.g., `kubeconfig` or service account tokens).

### Using OpenAPI Specification

- The **OpenAPI specification (formerly Swagger)** provides structured documentation for the Kubernetes API.
- It details available endpoints, expected parameters, authentication, and response formats.
- OpenAPI specs can be retrieved via `kubectl proxy` or found in the **Kubernetes GitHub repo**
- `curl localhost:8001/openapi/v2` or `curl localhost:8001/openapi/v3`
- `https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.25/`
- `https://github.com/kubernetes/kubernetes/blob/release-1.25/api/openapi-spec/swagger.json`

#### Postman for API Exploration

- **Postman** helps visualize and test API interactions.
- Import the **Swagger JSON URL** to generate API requests.
- Set `BASE_URL` to `http://localhost:8001` for API calls through `kubectl proxy`.
- Modify authentication settings (`No Auth`) and headers (e.g., `Content-Type: application/json`).
- Convert API calls into `curl`, `Python`, or `Go` requests.
- Equivalent API request to get nodes

```bash
curl --location 'http://localhost:8001/api/v1/nodes' --header 'Accept: application/json'
```

#### Using `kubectl` to Generate API Payloads

- Running `kubectl run nginx --image=nginx -v9` reveals:
  - The **JSON request body** used to create a Pod.
  - The **POST request** structure and required headers.
- Copy this JSON into **Postman** to quickly recreate API calls.

##### Performing API Operations

- **Creating a Pod**: Use `POST /api/v1/namespaces/default/pods` with a valid JSON body.
- **Deleting a Pod**: Use `DELETE /api/v1/namespaces/default/pods/{pod_name}`.

### Key Takeaways

- **kubectl is a wrapper** for the Kubernetes API.
- **kubectl proxy** enables API access without authentication.
- This is possible because kubectl proxy command in Kubernetes automatically handles authentication with the API server by using the existing authentication configuration, such as the `kubeconfig` file or service account tokens.
- This allows users to access the API server without having to manually authenticate each time.
- **Postman + OpenAPI** simplifies API exploration.
- **Use `kubectl -v9`** to extract API request details and craft direct API calls.
- ”**Rule #7:** Deprecated behaviors must function for no less than 1 year after their announced deprecation.”
- [https://kubernetes.io/docs/reference/using-api/deprecation-policy/#deprecating-a-feature-or-behavior](https://kubernetes.io/docs/reference/using-api/deprecation-policy/#deprecating-a-feature-or-behavior)

## Role-Based Access Control (RBAC) in Kubernetes

- RBAC is a method for managing access to Kubernetes resources, ensuring only authorized users and services can perform specific actions.
- By defining roles and assigning them to users or groups, RBAC enables fine-grained access control and helps prevent unauthorized access to sensitive resources.

### Kubeconfig and CA

- The `kubeconfig` file contains both **cluster details**, such as the API server URL, **certificate authority data, authentication credentials, and user information**, including the username, client certificate, and private key.
- This allows users to authenticate with multiple clusters and switch between them seamlessly.
- **Certificate Authority (CA)** is responsible for **creating and verifying certificates within the cluster**.
- The CA issues certificates to components (pods, services, and nodes), allowing them to securely communicate with each other.
- The CA also verifies the identity of these components by checking their certificates.
- **Users and Groups** in Kubernetes are typically **managed externally** through mechanisms such as **client certificates signed by a Certificate Authority (CA) or tokens** that can be obtained from an external source.
- These external sources **provide the credentials to authenticate users and authorize access to cluster resources**.

### ClusterRole and ClusterRolebinding

- **ClusterRole** defines a **set of permissions that can be applied to resources across the entire cluster**, rather than being limited to a specific namespace. This allows for more flexible and powerful role-based access control (RBAC) configurations.
- **Permissions** are **assigned to users through their membership in a group or by being explicitly mentioned in a RoleBinding or ClusterRoleBinding**.
- A **ClusterRoleBinding grants the permissions** defined in a **ClusterRole to a user or group for all namespaces in the cluster**.
- When a new user, such as "batman", is associated with a group in Kubernetes RBAC, it is done through the "O" field in the certificate subject. The "O" field specifies the organization or group that the user belongs to. This information is used by the Kubernetes API server to determine the user's group membership.

### Understanding Kubernetes Access

![[5 Kubernetes Deep Dive Controlling Access API.png]]

#### ✅ **Kubernetes user types:**

1. **Normal Users** → managed **outside** the cluster (e.g., via certificates, password files, Google accounts).
2. **Service Accounts** → managed **inside** the cluster, used by in-cluster processes to interact with the API server.
    - Tied to a **specific Namespace**
    - Credentials are mounted into Pods as **Secrets**
    - Created automatically or manually
- The authentication sequence is: Authentication -> Authorization -> Admission Control

#### ✅ **Kubernetes authentication methods:**

- Supports **Normal Users, Service Accounts, anonymous requests**, and **user impersonation** (for admin troubleshooting).
- Authentication modules:
    1. **X.509 Client Certificates** – validated by certificate authorities passed to API server.
    2. **Static Token File** – predefined bearer tokens in a file (requires API server restart to change).
    3. **Bootstrap Tokens** – used for cluster bootstrapping.
    4. **Service Account Tokens** – signed tokens automatically attached to Pods for API server communication.
    5. **OpenID Connect Tokens** – integrates external OAuth2 providers (e.g., Google, Microsoft Entra ID).
    6. **Webhook Token Authentication** – delegates token verification to a remote service.
    7. **Authenticating Proxy** – enables custom authentication logic.
- To ensure successful user authentication, we should enable at least two methods: the service account tokens authenticator and one of the user authenticators.

##### Authentication mechanisms

Kubernetes supports several ways to authenticate users accessing the API server:

- Static Password File
  - A CSV file (user-details.csv) contains credentials.
  - Format: password, username, userID.
  - Specified in the API server using the flag: `--basic-auth-file=user-details.csv`
  - Requires a restart of the kube-apiserver.
- Static Token File
  - Similar to the static password file, but uses tokens instead.
  - Used for service accounts or user authentication via tokens.
  - Tokens generated by Kubernetes are audience bound, time bound, and object bound.
- Client Certificates
  - Each user has their own certificate.
  - The API server validates these against a certificate authority (CA).
- External Authentication Providers
  - Integration with external systems:
    - LDAP
    - Kerberos
    - OIDC (OpenID Connect)
    - Webhook token authentication


###### Example: kube-apiserver Configuration (Partial)

```bash
ExecStart=/usr/local/bin/kube-apiserver \
  --advertise-address=${INTERNAL_IP} \
  --basic-auth-file=user-details.csv \
  --authorization-mode=Node,RBAC \
  --etcd-servers=https://127.0.0.1:2379 \
  --service-cluster-ip-range=10.32.0.0/24 \
  --service-node-port-range=30000-32767
```

- If you setup your cluster using the kubeadm tool, then you must modify the kube apiserver POD definition file.

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  name: kube apiserver
  namespace: kube system
spec:
  containers:
    - command:
        - kube apiserver
        - --authorization mode= Node,RBAC
        - --advertise address=172.17.0.107
        - --allow privileged=true
        - --enable admission plugins= NodeRestriction
        - --enable bootstrap token auth=true
      image: k8s.gcr.io/kube apiserver amd64:v1.11.3
      name: kube apiserver
```

- To authenticate using the basic credentials while accessing the API server, specify the user and password in a curl command.

```bash
curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123"
```

###### kubeconfig

- The kubeconfig file is how kubectl (or any Kubernetes client) knows how to connect to a Kubernetes cluster and authenticate the user. Think of it as a configuration file that tells your CLI:
  - Which cluster to connect to (address of the API server)
  - Who you are (your user credentials)
  - What namespace to use by default
  - Which context to operate under (a combo of user + cluster + namespace)

![[Kubernetes Deep Dive config view.png]]

- Access to a Kubernetes cluster is typically managed via a **kubeconfig** file, which specifies authentication details.
- The **kubeconfig only tells `kubectl` how to connect and authenticate to the cluster; it doesn't create users or permissions in Kubernetes itself**.
- The `kubectl config view` command shows the current configuration, including cluster details, users, and certificates.
- `context` entry references a single cluster and user also named **default**.
- When the cluster was created, a CA was set up. There is an entry for the CA in base64
- `cluster` entry has a reference to the API server and port.
- A **Certificate Authority (CA)** ensures secure communication with the API server, preventing man-in-the-middle attacks.
- `kubeconfig` file reference the public key of the CA.
- Open Policy Agent (OPA) is a policy engine that allows you to enforce fine-grained, context-aware policies in Kubernetes and other systems.
  - It helps with admission control and authorization by making API calls to decide whether a user should be permitted access based on their access requirements.

**Anatomy of kubeconfig (~/.kube/config)**

```yaml
apiVersion: v1
kind: Config
clusters:
  - name: my-cluster
    cluster:
      server: https://192.168.1.100:6443
      certificate-authority: /path/to/ca.crt
users:
  - name: my-user
    user:
      client-certificate: /path/to/client.crt
      client-key: /path/to/client.key
contexts:
  - name: my-context
    context:
      cluster: my-cluster
      user: my-user
      namespace: default
current-context: my-context
```

- kubeconfig contains these sections
  - Clusters: Define the Kubernetes clusters you can connect to (e.g., Dev, Test, Prod, across different clouds or orgs).
    - name, server url, and the CA cert to verify the server
  - Users: Represent the identities you can use to connect (e.g., admin, dev user), each with their own permissions.
    - name, a client certificate, and key for authentication
  - Contexts: Link a specific user to a specific cluster. Example: Admin@Production uses the admin user to access the Prod cluster.
    - Each context links a user to a cluster. e.g., my-user@my-cluster

#### ✅ **Kubernetes authorization methods:**

- After authentication, users can send API requests to Kubernetes.
- Kubernetes authorizes these requests using different authorization modules.
- Authorization evaluates attributes like:
    - User
    - Group
    - Resource
    - Namespace
    - API group
- These attributes are checked against policies to allow or deny the request.
- Kubernetes supports multiple authorization modules (authorizers) per cluster.
- Authorizers are checked in sequence.
- The first authorizer to allow or deny the request returns the decision immediately.

##### Authorization modes:

###### ✅ **Node Authorization**

- Special-purpose authorization for **kubelet API requests**.
- Authorizes:
    - **Read**: services, endpoints, nodes.
    - **Write**: nodes, pods, events.
- Enabled automatically for kubelets.

###### ✅ **Attribute-Based Access Control (ABAC)**

- Grants access based on **policies + attributes** (user, namespace, resource, etc.).
- Example: user `bob` can only **read pods** in namespace `lfs158`.
```yaml
{
  "apiVersion": "abac.authorization.kubernetes.io/v1beta1",
  "kind": "Policy",
  "spec": {
    "user": "bob",
    "namespace": "lfs158",
    "resource": "pods",
    "readonly": true
  }
}
```
- Enabled by starting API server with:
    - `--authorization-mode=ABAC`
    - `--authorization-policy-file=PolicyFile.json`

###### ✅ **Webhook Authorization**

- Delegates authorization to an **external service** via HTTP.
- External service returns **true (allow)** or **false (deny)**.
- Enabled by starting API server with:
    - `--authorization-webhook-config-file=SOME_FILENAME`

###### ✅ **Role-Based Access Control (RBAC)**

- Grants access based on **roles assigned to subjects** (users, service accounts).
- Roles define **allowed verbs (get, list, create, etc.)** for resources.
- Two types of roles:
    - **Role**: namespace-scoped.
    - **ClusterRole**: cluster-wide scope.
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: lfs158
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group**  
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```
- `pod-reader` allows `get`, `watch`, `list` pods in namespace `lfs158`.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin
rules:
- apiGroups:
  - '*' # All API groups
  resources:
  - '*' # All resources
  verbs:
  - '*' # All operations
- nonResourceURLs:
  - '*' # All non resource URLs, such as "/healthz"
  verbs:
  - '*' # All operations
```
- ClusterRole `cluster-admin` allows **all actions on all resources**.
- When role is created, we can bind it to users with `RoleBinding` object.

- Roles are bound to users via:
    - **RoleBinding**: binds Role (or ClusterRole) to user **within a namespace**.
    - **ClusterRoleBinding**: binds ClusterRole to user **cluster-wide**.
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-read-access
  namespace: lfs158
subjects:
- kind: User
  name: bob
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```
- RoleBinding binds `bob` to `pod-reader` in namespace `lfs158`.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: Group
  name: system:admins
```
- The manifest defines a bind between the **cluster-admin** ClusterRole and all users of the group **system:admins**.
- To enable the RBAC mode, we start the API server with the `--authorization-mode=RBAC` option, allowing us to dynamically configure policies. For more details, please review the [RBAC mode](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).

#### Authentication and Authorization in Minikube

-  View the content of the **kubectl** client's configuration manifest, observing the only context **minikube** and the only user **minikube**, created by default (the output has been redacted for readability):
```yaml
$ kubectl config view

apiVersion: v1
clusters:
- cluster:
    certificate-authority: /home/student/.minikube/ca.crt
    server: ht‌tps://192.168.99.100:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /home/student/.minikube/profiles/minikube/client.crt
    client-key: /home/student/.minikube/profiles/minikube/client.key
```

Create **lfs158** namespace:

```bash
$ kubectl create namespace lfs158

namespace/lfs158 created
```

Create the **rbac** directory and **cd** into it:

```yaml
$ mkdir rbac
$ cd rbac/
```

Create a new user **bob** on your workstation, and set **bob**'s password as well (the system will prompt you to enter the password twice) :

```yaml
~/rbac$ sudo useradd -s /bin/bash bob
~/rbac$ sudo passwd bob
```

Create a **private key** for the new user **bob** with the **openssl** tool, then create a **certificate signing request** for **bob** with the same **openssl** tool:

```yaml
~/rbac$ openssl genrsa -out bob.key 2048

Generating RSA private key, 2048 bit long modulus (2 primes)
.................................................+++++
.........................+++++
e is 65537 (0x010001)

~/rbac$ openssl req -new -key bob.key \
  -out bob.csr -subj "/CN=bob/O=learner"
```

Create a YAML definition manifest for a **certificate signing request** object, and save it with a blank value for the **request** field: 

```yaml
~/rbac$ vim signing-request.yaml

apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: bob-csr
spec:
  groups:
  - system:authenticated
  request: <assign encoded value from next cat command>
  signerName: kubernetes.io/kube-apiserver-client  
  usages:
  - digital signature
  - key encipherment
  - client auth
```

View the **certificate**, encode it in **base64**, and assign it to the **request** field in the **signing-request.yaml** file:

```yaml
~/rbac$ cat bob.csr | base64 | tr -d '\n','%'
LS0tLS1CRUd...1QtLS0tLQo=

~/rbac$ vim signing-request.yaml

apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: bob-csr
spec:
  groups:
  - system:authenticated
  request: LS0tLS1CRUd...1QtLS0tLQo=
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - digital signature
  - key encipherment
  - client auth
```


Create the **certificate signing request** object, then list the certificate signing request objects. It shows a **pending** state:

```yaml
~/rbac$ kubectl create -f signing-request.yaml

certificatesigningrequest.certificates.k8s.io/bob-csr created

~/rbac$ kubectl get csr

NAME      AGE   SIGNERNAME                            REQUESTOR       CONDITION
bob-csr   12s   kubernetes.io/kube-apiserver-client   minikube-user   Pending
```

Approve the **certificate signing request** object, then list the certificate signing request objects again. It shows both **approved** and **issued** states:

```yaml
~/rbac$ kubectl certificate approve bob-csr

certificatesigningrequest.certificates.k8s.io/bob-csr approved

~/rbac$ kubectl get csr

NAME      AGE   SIGNERNAME                            REQUESTOR       CONDITION
bob-csr   57s   kubernetes.io/kube-apiserver-client   minikube-user   Approved,Issued
```

Extract the approved certificate from the certificate signing request, decode it with base64 and save it as a certificate file. Then view the certificate in the newly created certificate file:

```yaml
~/rbac$ kubectl get csr bob-csr \
  -o jsonpath='{.status.certificate}' | \
  base64 -d > bob.crt

~/rbac$ cat bob.crt

-----BEGIN CERTIFICATE-----
MIIDGzCCA...
...
...NOZRRZBVunTjK7A==
-----END CERTIFICATE-----
```

Configure the kubectl client's configuration manifest with user bob's credentials by assigning his key and certificate: 

```yaml
~/rbac$ kubectl config set-credentials bob \
  --client-certificate=bob.crt --client-key=bob.key
```

User "bob" set.

Create a new context entry in the kubectl client's configuration manifest for user bob, associated with the lfs158 namespace in the minikube cluster:

```yaml
~/rbac$ kubectl config set-context bob-context \
  --cluster=minikube --namespace=lfs158 --user=bob
```

Context "bob-context" created.

View the contents of the kubectl client's configuration manifest again, observing the new context entry bob-context, and the new user entry bob (the output is redacted for readability):

```yaml
~/rbac$ kubectl config view

apiVersion: v1
clusters:
- cluster:
    certificate-authority: /home/student/.minikube/ca.crt
    ...
    server: ht‌tps://192.168.99.100:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    ...
    user: minikube
  name: minikube
- context:
    cluster: minikube
    namespace: lfs158
    user: bob
  name: bob-context
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /home/student/.minikube/profiles/minikube/client.crt
    client-key: /home/student/.minikube/profiles/minikube/client.key
- name: bob
  user:
    client-certificate: /home/student/rbac/bob.crt
    client-key: /home/student/rbac/bob.key
```

While in the default **minikube context**, create a new **deployment** in the **lfs158** namespace:

```yaml
~/rbac$ kubectl -n lfs158 create deployment nginx --image=nginx:alpine

deployment.apps/nginx created
```

From the new **context bob-context** try to list pods. The attempt fails because user **bob** has no permissions configured for the **bob-context**:

```yaml
~/rbac$ kubectl --context=bob-context get pods

Error from server (Forbidden): pods is forbidden: User "bob" cannot list resource "pods" in API group "" in the namespace "lfs158"
```

The following steps will assign a limited set of permissions to user **bob** in the **bob-context**. 

Create a YAML configuration manifest for a **pod-reader** Role object, which allows only **get**, **watch**, **list** actions/verbs in the **lfs158** namespace against **pod** resources. Then create the **role** object and list it from the default **minikube context**, but from the **lfs158** namespace:

```yaml
~/rbac$ vim role.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: lfs158
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

~/rbac$ kubectl create -f role.yaml

role.rbac.authorization.k8s.io/pod-reader created

~/rbac$ kubectl -n lfs158 get roles

NAME         CREATED AT
pod-reader   2022-04-11T03:47:45Z
```

#### Authorization mechanisms

- Node
- ABAC
- RBAC
- Webhook

##### Node authorized

- The Node Authorizer is a specialized authorization mechanism that is used specifically for nodes in a Kubernetes cluster. It verifies whether a node is authorized to access resources within the cluster.
- How it works: It checks the identity of the node (usually via service account credentials) to determine if the node has permission to perform actions, such as pulling images or managing its pods.
- The Node Authorizer checks if the service account associated with the node has permission to perform the requested action. The service account’s permissions are usually defined using RBAC rules.
- RBAC and Node Authorizer deals with authorization, but they operate in different contexts. While RBAC provides fine-grained control over access to cluster resources based on roles and bindings, the Node Authorizer is a more specialized authorization mechanism specifically for nodes in the cluster.

###### Example of the Interaction

- A kubelet (node) tries to get the list of pods in the cluster.
  - The kubelet sends an API request to the Kubernetes API server with its associated service account token.
- The Node Authorizer receives this request and checks whether the kubelet’s service account is authorized to access the pods resource.
  - The Node Authorizer determines the service account's permissions based on its RBAC configuration.
  - For instance, if the node's service account is associated with a ClusterRole that allows it to get pods (pods: get), then the request will be authorized.
    - If the service account does not have the required permissions in the RBAC configuration, the Node Authorizer will deny the request.

##### ABAC

- Authorization mechanism that grants access based on a user's attributes, such as roles, namespaces, or the specific resource they are trying to interact with.

###### Structure of ABAC Policy File

The ABAC policy file in Kubernetes is typically in JSON format. It consists of a series of rules, where each rule specifies what attributes (such as users, namespaces, or resources) are required for a user to be authorized to perform a particular action. Each rule is essentially a JSON object containing:

- user: The identity of the user.
- group: The group the user belongs to (optional).
- verb: The operation the user is attempting (e.g., get, create, delete).
- namespace: The namespace in which the action is being performed (optional).
- resource: The resource being accessed (e.g., pods, deployments).
- resourceNames: Specific resource names (optional).
- apigroup: Since pods belong to the core API group, it is set to empty (""). Deployments belong to the apps group.
- effect: The action to take (allow or deny).

```json
[
  {
    "user": "alice",
    "verb": "get",
    "resource": "pods",
    "namespace": "default",
    "apigroup": "",
    "effect": "allow"
  },
  {
    "group": "dev-users",
    "verb": "get",
    "resource": "deployments",
    "namespace": "default",
    "apigroup": "apps",
    "effect": "allow"
  }
]
```

##### RBAC Components

- More standard approach to manage access to kubernetes cluster.
  - We create a role with all the permissions, and then associate the users to the role.
- RBAC Components
  - **Users**: External identities interacting with Kubernetes. Users are not managed by Kubernetes.
  - **Groups**: Collections of users with shared permissions. They are manage outside the Kubernetes. When permissions are given to a group, all users that are part of that group receives those permissions.
  - **ServiceAccounts**: Managed by Kubernetes, used by applications inside the cluster. They are tied to a namespace and permissions are scoped to the namespaces.
  - ServiceAccounts are used to give permissions to the pod to interact with Kube API.
- RBAC permissions are defined via **RoleBindings** and **ClusterRoleBindings**, granting users, groups, or service accounts access to specific resources.

![[Kubernetes Deep Dive clusterrolebinding.png]]

- **Users in a group inherit that group’s permissions**.

  - Example: The user `system:admin` belongs to `system:masters`, which is bound to the **cluster-admin** role, granting full cluster-wide access.
    - All users that are part of `system:masters` group receives the same permission.
    - The user does not need to be listed on the `clusterrolebinding` output
      - It is referenced in the signed CA certificate as a Common Name (CN). In this case, `CN: system:admin`
    - The clusterrolebinding of `cluster-admin` binds:
      - clusterrole `cluster-admin`
      - Group `system:masters`

- **Viewing Permissions**

  - Run `kubectl api-resources --sort-by=name -o wide | more` to list resources and available verbs.
    - Example: `nodes` is a non-namespaced resource with specific verbs assigned.

###### Role and RoleBinding

- This role allows access to pods, services, and configmaps in the namespace default:
  - apiGroups: [""] refers to the core group (no version prefix in API path, e.g., /api/v1/pods).
  - For core resources, keep apiGroups as an empty string.
  - Other groups, like apps, use values like apiGroups: ["apps"] for resources like deployments.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: core-resources-reader
  namespace: default
rules:
  - apiGroups: [""]
    resources:
      - pods
      - services
      - configmaps
    verbs:
      - get
      - list
      - watch
```

- Bind the role to a user/service account/group:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: core-resources-reader-binding
  namespace: default
subjects:
  - kind: User
    name: johndoe # or kind: ServiceAccount / Group
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: core-resources-reader
  apiGroup: rbac.authorization.k8s.io
```

- The subject is where we specify the user details.
- The RoleRef is where we specify the details of the role
- Check the access:

```bash
kubectl auth can-i create deployments
yes
```

- You can impersonate another user:

```bash
kubectl auth can-i create deployments --as dev-user
no
```

###### ClusterRole and ClusterRoleBinding

![[Kubernetes Deep Dive clusterrole clusterrolebinding connection.png]]

- Non-namespaced resources: nodes, PV, clusterroles, clusterrolesbindings, certificatesigningrequests, namespaces.
  - `kubectl api-resources --namespaced=true` or `kubectl api-resources --namespaced=false`
- A **ClusterRole** is **non-namespaced** and applies to all resources across all namespaces.
  - The **cluster-admin** role grants full permissions to the full cluster (`*` on all resources and verbs).
- A **ClusterRoleBinding** links a ClusterRole to users, groups, or service accounts.
- **Example: Creating a New Superuser Group**

  - A new **ClusterRole** called `cluster-superhero` is created with full permissions.
    - `kubectl create clusterrole cluster-superhero --verb='*' --resource='*'`
  - A **ClusterRoleBinding** associates this role with the **cluster-superheroes** group.

    ```bash
    kubectl create clusterrole cluster-superhero --verb="*" --resource="*"
    kubectl create clusterrolebinding cluster-superhero --clusterrole=cluster-superhero --group=cluster-superheroes
    kubectl auth can-i '*' '*'
    ```

  - All users that authenticate with the group cluster-superheroes (e.g., Batman, Superman, Wonder Woman) will now have cluster-wide admin privileges.

- **Validating Permissions with `kubectl auth can-i`**

  - The `kubectl auth can-i` command checks if a user, group, or service account has specific permissions.
  - Running `kubectl auth can-i '*' '*'` confirms superuser access for any member of the `cluster-superheroes` group.

    ```bash
    kubectl auth can-i '*' '*' --as-group="cluster-superheroes" --as="batman"
    kubectl auth can-i '*' '*' --as-group="cluster-superheroes" --as="superman"
    kubectl auth can-i '*' '*' --as-group="cluster-superheroes" --as="wonder-woman"
    ```

###### Service Accounts

- A ServiceAccount provides an identity for processes running in a Pod. Kubernetes automatically creates and attaches a default service account to every pod unless you specify otherwise.
  - A service account is used by an application to interact with Kubernetes.
  - There are service accounts for the user and services.
    - When a service account is created, it is create a service account object, and then a token that it is going to be used by external applications
      - In Kubernetes v1.24, when a service account in created, it no longer creates a secret and a token access secret.
        - To create a toke execute: `kubectl create token <service-account-name>`
  - The token is stored as a secret object. (execute `kubectl describe serviceaccount <name>`)
- We are a binding a ServiceAccount to a ClusterRoleBinding, allowing to grant specific permissions to the ServiceAccount at the cluster level, meaning the ServiceAccount can perform actions on cluster-wide resources (across all namespaces) or all namespaces if specified.

- Create the ServiceAccount

  ```bash
   kubectl create serviceaccount superhero-sa --namespace=default
  ```

- Create a ClusterRole (if not already created)

  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: ClusterRole
  metadata:
    name: cluster-superhero
  rules:
    - apiGroups: ["*"]
      resources: ["*"]
      verbs: ["*"]
  ```

- Bind the serviceaccount to the clusterrole

```yaml
# cluster-superhero-binding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-superhero-binding
subjects:
  - kind: ServiceAccount
    name: superhero-sa
    namespace: default
roleRef:
  kind: ClusterRole
  name: cluster-superhero
  apiGroup: rbac.authorization.k8s.io
```

- Verify the permissions

```bash
kubectl auth can-i '*' '*' --as=system:serviceaccount:default:superhero-sa
yes
```

- You can go inside the pod and see the tokens assigned to the pod. You cannot edit an assigned serviceccount. If you want to modify the serviceaccount, you need to delete and create a new one.

```bash
kubectl exec -it my-kubernetes-dashboard --ls /var/run/secrets/kubernetes.io/serviceaccount
ca.crt
namespace
token
```

- To not mount a serviceaccount token to a kubernetes pods, set in the pod spec `automountServiceAccountToken=false`.

##### Webhooks

- Webhooks are used to extend and customize the behavior of the Kubernetes API server. There are two main types of webhooks in Kubernetes:
  - Admission Webhooks: These are used to intercept requests to the Kubernetes API server and allow you to modify or validate the requests before they are persisted in the Kubernetes cluster. They can be classified into two types:
    - Mutating Admission Webhooks: These webhooks can modify the incoming request, allowing you to change the object being created, updated, or deleted before it is persisted.
    - Validating Admission Webhooks: These webhooks validate the incoming request and can reject it based on custom logic.
  - Audit Webhooks: These webhooks allow you to collect logs or data about API requests made to the Kubernetes API server, typically for monitoring, auditing, or compliance purposes.

##### AlwaysAllow

- Allows everything without requiring authorization checks.
- Mode set in the kube-api server.
- In your kube-apiserver startup command or manifest, include the following:

```bash
--authorization-mode=AlwaysAllow
--authentication-token-webhook=false
--anonymous-auth=true
```

- Or, update the `/etc/kubernetes/manifests/kube-apiserver.yaml` file:

```yaml
spec:
  containers:
    - name: kube-apiserver
      command:
        - kube-apiserver
        - --authorization-mode=AlwaysAllow
        - --authentication-token-webhook=false
        - --anonymous-auth=true
      # ... other existing flags
```

- `--authorization-mode=AlwaysAllow`: disables all authorization checks.
- `--authentication-token-webhook=false`: disables remote authentication webhooks.
- `--anonymous-auth=true`: allows requests from unauthenticated users.

##### AlwaysDeny

Always deny.

##### Multiple modes

- It is possible to have multiple modes configured. The authorization happens in the order of the modes specified.
  - In this example, first it is handled by the node authorizer.
  - When a module denies a request, it is forwarded to the next mode. When a module accepts a request, no more verification is done.

```ỳaml
ExecStart=/usr/local/bin/kube-server \\
  --advertise-address=${INTERNAL_IP} \\
  --allow-privileged=true \\
  --apiserver-count=3 \\
  --authorization-mode=Node,RBAC,Webhook \\
  --bind-address=0.0.0.0 \\
  # ... other existing flags
```

#### Setup Kubernetes authentication and authorization using RBAC

![[Kubernetes Deep Dive certificates generation path.png]]

- **User & Key Creation**:

  - Created an RSA private key (`Batman.key`).
    - `openssl genrsa -out batman.key 4096`
  - Generated a Certificate Signing Request (`Batman.csr`) with `CN=Batman` and `O=cluster-superheroes`.

  ```bash
  openssl req -new -key batman.key -out batman.csr -subj "/CN=batman/O=cluster-superheroes" -sha256
  ```

  - `O` specifies the organization or the group the user belongs to.

    - Submitted the CSR to Kubernetes, got it approved, and extracted the signed certificate (`Batman.crt`).

      ```bash
      CSR_DATA=$(base64 batman.csr | tr -d '\n')
      CSR_USER=batman
      ```

  - Let's create a template for a Kubernetes certificate signing request

```bash
  cat << EOF > batman-csr-request.yaml
  apiVersion: certificates.k8s.io/v1
  metadata:
 name: ${CSR_USER}
  spec:
 request: ${CSR_DATA}
 signerName: kubernetes.io/kube-apiserver-client
 usages:
 - client auth
  EOF
```

- Apply and approve this certificate

```bash
kubectl apply -f batman-csr-request.yaml
kubectl certificate approve batman
```

- We can see the data under status
- ![[Kubernetes Deep Dive certificates status.png]]
- We can decode the certificate which base64, and then we see the real certificate.
- ![[Kubernetes Deep Dive certificate.png]]
- We can decode the certificate, and see that the certificate is approved with our CN of batman
- `openssl x509 -in batman.crt -text -noout`
- ![[Kubernetes Deep Dive decoded certificate.png]]

- **Kubeconfig Setup**:

  - Created a new kubeconfig file (`Batman-Cluster-Superheroes.config`).
    `cp /root/.kube/config batman-cluster-superheroes.config`
  - Removed unnecessary configurations.

    ```bash
    KUBECONFIG=batman-cluster-superheroes.config
    kubectl config delete-context default
    kubectl unset current-context
    ```

  - Added Batman’s credentials and embedded the certificate and key.

    ```bash
    KUBECONFIG=batman-cluster-superheroes.config
    kubectl config set-credentials batman --client-certificate=batman.crt --client-key=batman.key --embed-cert=true
    kubectl config set-context default --cluster=default --user=batman
    kubectl config use-context default
    ```

  - Verified access by running `kubectl get nodes` and deploying a pod.

- **Automated User Management**:
  - Used a script to automate the process for new users (e.g., Superman, Wonder Woman).
    - <https://github.com/spurin/kubeconfig-creator>
    - `./kubeconfig_creator.sh -u superman -g cluster-superheroes`
  - Ensured these users could access the cluster using their kubeconfig files.
- **RBAC (Role-Based Access Control)**:
  ![[Kubernetes Deep Dive role vs cluster-role.png]]

  - **Read-Only User (Uatu)**: Created a `cluster-watcher` role with `list`, `get`, and `watch` permissions.

    ```bash
    kubectl create clusterrole cluster-watcher --verb=list,get,watch --resource='*'
    kubectl create clusterrolebinding cluster-watcher --clusterrole=cluster-watcher --group=cluster-watchers
    kubectl auth can-i 'list' '*' --as-group="cluster-watchers" --as="uatu"
    ```

  - **Pod Manager (Deadpool)**: Created a `pod-manager` role allowing only `list`, `get`, `create`, and `delete` actions on pods.

    ```bash
    kubectl create clusterrole cluster-pod-manager --verb=list,get,create,delete --resource='pods'
    kubectl create clusterrolebinding cluster-pod-manager --clusterrole=cluster-pod-manager --user=deadpool
    kubectl auth can-i 'list' 'pods ' --as-group="cluster-pod-managers" --as="deadpool"
    ```

  - **Namespace-Scoped Role (Gryffindor)**: Created a role with permissions limited to the `Gryffindor` namespace.

    ```bash
    kubectl -n gryffindor create role gryffindor-admin --verb='*' --resource='*'
    kubectl -n gryffindor create rolebinding gryffindor-admin --role=gryffindor-admin --group=gryffindor-admins
    kubectl -n gryffindor auth can-i '*' '*' --as-group="gryffindor-admins" --as=harry
    ```

#### Authentication vs Kubeconfig

Even when Kubernetes authenticates users via external certificates, the **kubeconfig** file is still essential because it serves as a configuration file that stores authentication and cluster access details.

1. **Client Configuration**: The `kubeconfig` file contains the information needed for `kubectl` or any Kubernetes client to connect to the cluster, including:
   - The API server endpoint (`server`).
   - The authentication method (`certificate-authority`, `client-certificate`, `client-key`, `token`, or an external authentication provider).
   - The cluster name and context.
2. **Certificate Storage and Reference**: Even if Kubernetes is using an external Certificate Authority (CA) to authenticate users, the `kubeconfig` file typically the certificates (or other credentials like bearer tokens) to authenticate requests.
3. **Multi-Cluster and Multi-User Management**: A `kubeconfig` file can store multiple clusters and user credentials, allowing users to switch between different environments (e.g., development, testing, production) easily.
4. **Delegating Authentication to External Providers**: If Kubernetes uses an **OIDC provider, LDAP, or another external system**, the `kubeconfig` may contain a token or instructions for retrieving credentials dynamically.
5. **Context Switching**: The `kubeconfig` file allows users to manage multiple Kubernetes environments efficiently by defining different contexts.

#### User Authentication and RBAC

![[Kubernetes Deep Dive CA authentication.png]]

- Kubernetes does not manage users directly but relies on certificates issued by a CA to prevent man-in-the-middle attacks.
  - This data is encoded in base64
- We can see the certificate from the certificate-authority-data
- ![[Kubernetes Deep Dive decode certificate.png]]
- Users and groups are identified by **Common Name (CN)** and **Organization (O)** fields in certificates.
- Example: A user "James" in the "whales" group would need a certificate signed with `CN=James, O=whales`.
- In users we find client certificate data (user data that has been signed by the CA) and client key data (user's private key).
  - ![[Kubernetes Deep Dive users data.png]]
  - The client-key-data is sensitive and it is used to sign requests make via kubectl to the API server. This is the gateway to the kubernetes cluster.
  - ![[Kubernetes Deep Dive client private key decode.png]]
  - The client-certificate-data show certificates data.
  - ![[Kubernetes Deep Dive client certificates data.png]]
  - ![[Kubernetes Deep Dive client certificates data decoded.png]]
  - We can see that the public key was issued by kubernetes CA
  - `O = system:masters` is the group where the username is assigned.
  - `CN=system:admin` references to the username.
- In kubernetes, we don't create users or groups. We have certificates that relates users to the groups, and then we permission those users and groups with RBAC.
- ![[Kubernetes Deep Dive rbac certificates.png]]

#### Admission Controller

✅ **Admission controllers** in Kubernetes enforce policies **after authentication and authorization**, but **before objects are persisted** in etcd.
- They control actions like **restricting privileged containers**, **enforcing resource quotas**, and **setting default storage classes**.
- Admission controllers can be:
    - **Mutating** → modify incoming requests
    - **Validating** → reject or allow requests
    - (some are both)

👉 To use them, the API server must be started with `--enable-admission-plugins` listing the desired controllers.

##### 🔐 Built-in Admission Controllers

Below is a list of commonly used admission controllers in Kubernetes:

- **NamespaceLifecycle**: Ensures that operations are not performed on namespaces that are undergoing termination.
- **LimitRanger**: Enforces constraints on resource usage per namespace, such as CPU and memory limits.
- **ServiceAccount**: Automates the assignment of service accounts to pods.
- **DefaultStorageClass**: Assigns a default storage class to PersistentVolumeClaims that do not specify one.
- **DefaultTolerationSeconds**: Adds default tolerations to pods, allowing them to tolerate node taints for a specified duration.
- **MutatingAdmissionWebhook**: Invokes external webhooks to modify (mutate) incoming requests before they are persisted.
- **ValidatingAdmissionWebhook**: Invokes external webhooks to validate incoming requests without modifying them.
- **Priority**: Assigns priority classes to pods, influencing the order in which pods are scheduled and evicted.
- **ResourceQuota**: Enforces resource quotas defined per namespace, preventing resource overconsumption.
- **PodSecurity**: Enforces Pod Security Standards at the namespace level, replacing the deprecated PodSecurityPolicy.

These controllers are executed in a specific order: mutating admission controllers run first, followed by validating admission controllers. If any controller rejects a request, the entire request is denied immediately.

##### 🧩 Dynamic Admission Control

In addition to the built-in controllers, Kubernetes supports dynamic admission control through external webhooks:
- **MutatingAdmissionWebhook**: Allows custom logic to modify requests before they are persisted.
- **ValidatingAdmissionWebhook**: Allows custom logic to validate requests without modifying them.

#### API Groups

### 📦 Kubernetes API Groups Summary

Kubernetes APIs are divided into **two main groups**:

---

##### 🔹 Core Group (`/api/v1`)

- Contains fundamental Kubernetes resources:
  - **Pods**
  - **Namespaces**
  - **Nodes**
  - **Services**
  - **Secrets**
  - **ConfigMaps**
  - **PersistentVolumes (PV)**
  - **PersistentVolumeClaims (PVC)**
  - **ReplicationControllers (RC)**
  - **Endpoints**
  - **Events**
  - **Bindings**

---

##### 🔸 Named Groups (`/apis/<group>/<version>`)

- More modular and organized.
  - Used for **newer and advanced features**.
- Examples of named groups:
  - `apps/v1`: Deployments, ReplicaSets, StatefulSets
  - `extensions/v1beta1`
  - `networking.k8s.io/v1`: NetworkPolicies
  - `certificates.k8s.io/v1`: CertificateSigningRequests
  - `authentication.k8s.io/v1`
  - `storage.k8s.io/v1`

---

##### 🛠 Resources and Verbs

- Each group contains **resources** (like deployments, replicasets, statefulsets, pods, etc.). - You can view the group with

  ```bash
  curl http://localhost:6443 -k
  {
    "paths": [
      "/api",
      "/healthz",
      ...]
  ```

  or

```bash
curl http://localhost:6443 –k --key admin.key --cert admin.crt --cacert ca.crt
```

- An alternate option is to start a kubectl proxy client.

```bash
kubectl proxy
curl http://localhost:8001 -k
```

- You interact with resources using **verbs** (operations): list, get, create, update, delete, watch, etc...
- kube proxy is not kubectl proxy
  - kube proxy is used to enable connectivity between pods and services across different nodes in the cluster.
  - kubectl proxy is an HTTP proxy service created by kubectl utility to access the kube api server.

## Image Security

- By default, docker image is defined in the docker registry

  - `docker.io/library/nginx`
    - docker.io: docker registry (gcr.io for google)
    - library: user account
    - nginx: image

- To configure a private image repository and use it within Kubernetes, follow these steps:

### Create a Private Registry

- Use a cloud provider’s private registry (e.g., AWS ECR, Google Container Registry, Azure Container Registry).
- Set up your own registry using tools like Docker Registry or Harbor.

### Push Images to the Private Registry

- Before using the private repository in Kubernetes, you need to push your Docker images to it.
- Steps to push an image to a private registry:
  - Login to the private registry (for example, Docker Hub, AWS ECR, or any private registry):

```bash
docker login <registry-url>
```

- Tag your local image to match the private registry’s format:

```bash
docker tag <your-image>:<tag> <registry-url>/<your-image>:<tag>
```

Push the image to the private registry:

```bash
docker push <registry-url>/<your-image>:<tag> 3. Create a Docker Registry Secret in Kubernetes
```

### Create a Docker Registry Secrets in Kubernetes

- To pull images from a private registry in Kubernetes, you must create a Docker registry secret that contains your credentials (username, password, and registry URL).
  - Docker registry is a built-in secret type used to store credentials
- Create the secret with the following command:

```bash
kubectl create secret docker-registry regcred \
 --docker-server=<private-registry-url> \
 --docker-username=<your-username> \
 --docker-password=<your-password> \
 --docker-email=<your-email>
```

Replace `<private-registry-url>`, `<your-username>`, `<your-password>`, and `<your-email>` with your actual registry details.

### Reference the Secret in Your Pod Definition

- To use the image from your private registry in a Kubernetes pod, reference the created secret in the pod's YAML file.
- Here’s an example of how to configure the pod to use the private registry image:

Example Pod Definition (nginx-pod.yaml):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: <private-registry-url>/nginx:latest
  imagePullSecrets:
    - name: regcred
```

- Replace `<private-registry-url>/nginx:latest` with the path to your image in the private registry.
- The imagePullSecrets section tells Kubernetes to use the regcred secret to pull the image.

### Deploy the Pod

Finally, apply the YAML file to create the pod:

```bash
kubectl apply -f nginx-pod.yaml
```

### Accessing and Managing Private Repositories

If you're using a service like AWS ECR or Google GCR, there are additional steps to configure credentials or use IAM roles. These services provide commands and tools like AWS CLI or gcloud for authentication.

For example, in AWS ECR, you authenticate using:

```bash
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws-account-id>.dkr.ecr.<region>.amazonaws.com
```

This login command is required to authenticate your Docker client with the AWS ECR registry.

### Optional: Using Helm Charts

If you're using Helm to deploy applications, you can add the imagePullSecrets to your Helm chart values file, like so:

```yaml
imagePullSecrets:
  - name: regcred
```

This will ensure your Helm deployments can pull images from the private registry.

## Security Context

- Security settings, like the user ID for running containers and the Linux capabilities, can be configured at both the container level and the pod level.
  - If set at the pod level, the settings apply to all containers within the pod.
  - If both the pod level and container level are configured, the container-level settings will override the pod-level ones.

```bash
docker run --user=1001 ubuntu sleep 3600
docker run --cap-add MAC_ADMIN ubuntu
```

- Pod-Level Configuration:
  - To set the security context for the pod, you define the securityContext field under the spec section of the pod, using the runAsUser option to set the user ID for the pod.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  securityContext:
    runAsUser: 1000 # Set user ID for the pod
    fsGroup: 2000 # Set the group ID for the pod's filesystem
    runAsGroup: 1000 # Set group ID for the pod
    seLinuxOptions:
      level: "s0:c123,c456" # Set SELinux options for the pod
    capabilities:
      add:
        - NET_ADMIN # Add specific capabilities to the pod

  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
```

- Container-Level Configuration:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: nginx
      securityContext:
        runAsUser: 1000 # Set user ID for this container
        runAsGroup: 1000 # Set group ID for this container
        fsGroup: 2000 # Set the group ID for the container's filesystem
        seLinuxOptions:
          level: "s0:c123,c456" # Set SELinux options for this container
        capabilities:
          add:
            - NET_ADMIN # Add specific capabilities to the container
      ports:
        - containerPort: 80
```

- To apply security settings at the container level, you can move the security context section directly under the container specification.
- Capabilities are only accessible at the container level.

You can also use the capabilities option to add specific Linux capabilities to the pod or container.

## Network Policy

- NetworkPolicy controls the flow of network traffic between pods, namespaces, or external endpoints. Without network policies, all pods can talk with each other, across namespaces.
  - Restrict traffic between pods to enhance security (default in Kubernetes is "allow all").
  - Allow only specific communication paths (e.g., only allow the API pod to talk to the DB pod).
  - Prevent unauthorized access by blocking all other connections unless explicitly allowed.
  - Enforce least privilege networking between services.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
    ingress:
  - from:
    - podSelector:
        matchLabels:
          role: api
    ports:
    - protocol: TCP
      port: 3306
```

- Applies to pods with role: db
- Blocks all incoming traffic by default
- Allows only traffic from pods labeled role: api
- Only on TCP port 3306

## Scheduling process

The **kube-scheduler** is responsible for assigning pods to nodes in a Kubernetes cluster. The scheduling process consists of several key steps:

- **Pod Creation**
  - If a pod is created without a specified node, the `kube-scheduler` determines the best node for it.
    - It evaluates the pod's resource requirements (CPU, memory) against available nodes.
  - Other factors such as **node affinity, anti-affinity, taints and tolerations** are also considered.
- **Queue Phase**
  - Pods are placed in the scheduling queue.
  - Sorted by priority, determined by the `priorityClassName`.
  - A PriorityClass must be defined (e.g., high-priority with a value of 1,000,000).
  - Plugin: `PrioritySort`
- **Scheduling Process** The scheduler follows three main operations:
  - **Filtering:** Nodes that don't meet resource or constraint requirements are removed.
    - Plugin: `NodeResourcesFit`, `NodeName`, `NodeUnschedulable`, etc.
  - **Scoring:** The remaining nodes are scored based on various functions to determine the best fit. The nodes with the highest score is chosen.
    - Remaining nodes are scored.
      - Score based on how much CPU will remain after placing the pod:
        - E.g., Node A → 2 CPUs left, Node B → 6 CPUs left → Node B scores higher.
    - Plugin: `NodeResourcesFit`, `ImageLocality`, etc.
  - **Binding:** The pod is assigned to the node with the highest score, and the kubelet starts the pod. The name of the node is stored in the node name field of the pod.
    - Plugin: `DefaultBinder`

### Custom Scheduler

- Kubernetes allows running a custom scheduler by specifying the `schedulerName` field in the pod spec.

```yaml
apiVersion: v1
kind: Pod
metadata:
creationTimestamp: null
labels:
run: nginx
name: nginx
spec:
schedulerName: my-scheduler
containers:
  - image: nginx
name: nginx
resources: {}
dnsPolicy: ClusterFirst
restartPolicy: Always
status: {}
```

- A simple example using a Bash script demonstrates a basic custom scheduler that:
- `git clone https://github.com/spurin/simple-kubernetes-scheduler-example`
  - Queries available nodes.
  - Filters pods scheduled with a custom scheduler.
  - Randomly selects a node and binds the pod.

### Bypassing the Scheduler

- Instead of using a scheduler, a pod can be assigned directly to a node by setting `nodeName` in the pod spec.
- `nodeName` specifies the desired node for scheduling, but it may not reflect the actual node the pod is running.
- In this example, the pod is assigned to the node `worker-2`.

```yaml
apiVersion: v1
kind: Pod
metadata:
creationTimestamp: null
labels:
  run: nginx
name: nginx
spec:
  nodeName: worker-2
  containers:
    - image: nginx
  name: nginx
  resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  status: {}
```

Alternatively, it can be created a bind object that binds a pod to an object, and send a post request to the binding api.

```yaml
apiVersion: v1
kind: Binding
metadata:
  name: nginx
target:
  apiVersion: v1
  kind: Node
  name: worker-2
```

```bash
curl --header "Content-Type:application/json" --request '{"apiVersion": "v1", "kind": "Binding", ...}' POST --data http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/
```

### Labels and Selectors

- A more targeted approach than `nodeName` is `NodeSelectors`.
  - Instead of specifying a direct node, `NodeSelectors` use Kubernetes labels to identify the target node.
  - Selectors filters the items by the label.
    - We can group by the type, or by the application, or by the functionality.
  - This allows for more flexibility and dynamic scheduling based on node characteristics.
- To view available node labels, use the following command:

```bash
kubectl describe node/worker-1 | more

Name:               worker-1
Roles:              <none>
Labels:             beta.kubernetes.io/arch=arm64
                    beta.kubernetes.io/instance-type=k3s
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=arm64
                    kubernetes.io/hostname=worker-1
                    kubernetes.io/os=linux
                    node.kubernetes.io/instance-type=k3s
```

- You can use the `nodeSelector` option to select specific nodes through the use of labels.

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
  app: App1
  function: backend
    run: nginx
  name: nginx
spec:
  nodeSelector:
    kubernetes.io/hostname: worker-1
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```bash
kubectl get pods --selector app=App1
```

To create a ReplicaSet with 3 different pods, you label the pod definitions and use a selector in the ReplicaSet to group them. In the ReplicaSet definition file, labels appear in two places:

- The labels at the top define labels for the ReplicaSet.
- The labels under the template section define labels for the pods themselves.

```yaml
apiVersion: v1
kind: ReplicaSet
metadata:
  name: single-webapp
  labels:
    app: App1
    function: Front-end
spec:
  replicas: 3
  selector:
    matchLabels:
      app: App1
  template:
    metadata:
      labels:
        app: App1
        function: Front-end
    spec:
      containers:
        - name: simple-webapp
          image: nginx
```

The labels on the ReplicaSet are not immediately important for connecting the ReplicaSet to the pods. The key part is using the selector field in the ReplicaSet specification, which matches the labels on the pods.
A single label can suffice if it uniquely identifies the pods, but if there might be other pods with the same label serving different purposes, you can specify multiple labels to ensure the correct pods are selected by the ReplicaSet.
On creation, if the labels of the selector matches the label of the pods, the ReplicaSet is created successfully.

### Taints and Tolerations

- Taints and Tolerations in Kubernetes control which pods can be scheduled on specific nodes.
  - Taints are set on nodes.
  - Tolerations are set on pods.
- Imagine a cluster with 3 nodes (Node1, Node2, Node3) and 4 pods (A, B, C, D).
  - By default, the scheduler distributes pods evenly across nodes.
  - If you want Node1 to be reserved for specific pods (e.g., for a special application):
    - Add a taint (e.g., blue) to Node1.
      - This prevents all pods from being scheduled on it unless they can tolerate the taint.
  - How the scheduler works in this example:
    - Pod A → Tries Node1 → Rejected due to taint → Placed on Node2
    - Pod B → Tries Node1 → Rejected → Placed on Node3
    - Pod C → Tries Node1 → Rejected → Placed on Node2
    - Pod D → Tries Node1 → Accepted (has toleration for the taint) → Successfully scheduled on Node1
  - Taints and tolerations does not tell the POD to go to only a particular Node. Instead it tells the Node to only accept PODs with certain tolerations.
    - If your requirement is to restrict a POD to certain nodes, it is achieved through another concept called as Node Affinity

Pods have no tolerations by default, so they will not be placed on Node1 anymore.

To allow specific pods (e.g., Pod D) to run on Node1, add a toleration for the blue taint in Pod D’s definition.

```bash
kubectl taint nodes node-name key=value:taint-effect
```

- The taint effects tell what do happens to PODs that do not tolerate the taint.
  - NoSchedule
  - PreferNoSchedule: try to avoid to place the pod on the node, but there is no guarantee.
  - NoExecute: New pods won´t be scheduled on the node, and existing pods on the node will be evicted if they do not tolerate the taint. These pods were scheduled on the node, before they were applied.

```bash
kubectl taint nodes worker-1 app=blue:NoSchedule
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
spec:
  containers:
  - name: nginx-container
    image: nginx
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```

### Node Selectors

Label nodes

```bash
kubectl label nodes <node-name> <label-key>=<label-value>
kubectl label nodes node-1 size=Large
```

Then we can create the pod and force it to run in the node with the label Large.

```yaml
apiVersion:
kind: Pod
metadata:
  name: myapp-pod
spec:
containers:
  - name: data-processor
    image: data-processor
nodeSelector:
  size: Large
```

But node selectors has some limitations.

- 🚫 Limitations of nodeSelector:
  - Single Key-Value Match Only
  - nodeSelector can only match one or more exact key-value labels on a node.
  - You can't use complex logic like OR, NOT, or inequalities.
  - ❌ Example: You can’t say “schedule on nodes in region X or zone Y”.
- No Advanced Expressions
  - Unlike nodeAffinity, nodeSelector doesn't support set-based matching (like In, NotIn, Exists, etc.).
- Static Configuration
  - It's a static binding—you define it in the pod spec.
  - There’s no way to update or modify the behavior dynamically without changing the pod spec and redeploying.
- Not Flexible for Multiple Constraints
  - If you need to match a combination of different labels under flexible conditions, nodeSelector won't cut it.
- No Weighting or Prioritization
  - nodeSelector only filters eligible nodes.
  - It doesn’t allow prioritization or ranking of nodes (e.g., prefer a node, but not strictly require it).
- Limited Scheduling Control
  - You can’t express soft constraints (preferences) — all constraints are hard.
  - If no node matches, the pod simply stays pending forever.

### Node Affinity

The Node Affinity feature provides us with advanced capabilities to limit POD placement on specific Nodes.
This poh is set to execute in a node with the label Large.

```yaml
apiVersion:
kind:
metadata:
  name: myapp-pod
spec:
  containers:
    - name: data-processor
      image: data-processor
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
              - key: size
                operator: In
                values:
                  - Large
```

- There are two additional types of Node Affinity planned

  - requiredDuringSchedulingRequiredDuringExecution
  - preferredDuringSchedulingRequiredDuringExecution

- 🧱 Types of Node Affinity:
  - requiredDuringSchedulingIgnoredDuringExecution
    - Strict requirement: the pod must be scheduled on a node that matches the label.
    - ❌ If no matching node exists, the pod stays Pending.
  - ✅ Use when placement is critical.
- preferredDuringSchedulingIgnoredDuringExecution
  - Soft requirement: scheduler tries to find a matching node,
    - 😌 If none is found, it still schedules the pod on another available node.
    - ✅ Use when placement is preferred, but running the workload is more important.
- requiredDuringSchedulingRequiredDuringExecution
  - The pod can only be scheduled on nodes that match the affinity rules.
  - The pod would be evicted or fail if the node’s labels changed and no longer matched

#### Exercise

- 🎯 Goal
  - Place each colored pod (Blue, Red, Green) on its matching colored node and prevent interference from other teams’ pods.
- 🛠 Taints and Tolerations Only
  - Taints are applied to nodes (e.g., color=blue:NoSchedule).
- Tolerations are added to pods (e.g., tolerate: color=blue).
  - ✅ Ensures only pods with the correct toleration run on the node.
  - ❌ But doesn't force pods to go to the tainted node — they can land elsewhere if allowed.
- 🧭 Node Affinity Only
  - Labels are added to nodes (e.g., color=blue).
- Node selectors/affinity are added to pods to match labels.
  - ✅ Ensures pods are placed on matching nodes.
  - ❌ But doesn’t stop other (unrelated) pods from landing on those nodes.
- ✅ Best Practice: Combine Both
  - Use taints/tolerations to protect nodes from unwanted pods.
  - Use node affinity to ensure your pods go to the correct nodes.
  - This dedicates specific nodes to specific pods — no cross-contamination.

### Scheduler Profile

#### Queueing phase Profile

##### 1. Define the `PriorityClass` (run this first if not already created)

```yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000
globalDefault: false
description: "This priority class is used for high-priority pods."
```

Apply it:

```bash
kubectl apply -f high-priority-class.yaml
```

---

##### 2. Define the Pod that uses the `PriorityClass`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
spec:
  priorityClassName: high-priority
  containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      resources:
        requests:
          memory: "1Gi"
          cpu: "10"
```

This pod will get scheduled with higher precedence compared to pods with lower or no priority class, when provided resources are available.
Then the pod enters the Filter phase. This is where nodes that cannot run the pod are filtered out.
Then it enters the Scoring phase. This is where nodes are scored with different weights.
Then it enters the binding phase. This is where the pod is finally bound to a node with the highest score.

#### Extension Points

- The Kubernetes scheduler provides multiple extension points where custom plugins can hook into the scheduling process. These points allow developers to run custom logic at different phases of scheduling a Pod:
  - PreFilter: Runs before the Filter phase. Used for preparing or shortlisting nodes early.
  - Filter: Filters out nodes that do not meet the Pod’s requirements.
  - PostFilter: Runs after the Filter phase. Can be used to take action if no suitable nodes were found.
  - PreScore: Executes before Scoring. Used to set up or prepare scoring calculations.
  - Score: Assigns scores to nodes based on criteria like available resources.
  - Reserve: Runs after Scoring to temporarily reserve resources on the selected node.
  - Permit: A checkpoint before Binding, which can allow or deny Pod placement.
  - PreBind: Executes just before the Pod is bound to the selected node.
  - Bind: Actually binds the Pod to the node.
  - PostBind: Runs after the Bind phase is complete. Useful for cleanup or notification.
- These extension points make the scheduler highly customizable using custom scheduler plugins.
- Kubernetes Scheduler Profile with a custom configuration using plugins like `NodeResourcesFit`, `NodeAffinity`, and a sample custom plugin named `MyCustomPlugin`.
- You define the scheduler profile in the scheduler configuration file, typically passed via `--config=/path/to/scheduler-config.yaml`.
- `scheduler-config.yaml` example

```yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
clientConnection:
  kubeconfig: /etc/kubernetes/scheduler.conf
leaderElection:
  leaderElect: true
profiles:
  - schedulerName: my-scheduler
    plugins:
      preFilter:
        enabled:
          - name: MyCustomPlugin
      filter:
        enabled:
          - name: NodeResourcesFit
          - name: NodeAffinity
          - name: MyCustomPlugin
      score:
        enabled:
          - name: NodeResourcesBalancedAllocation
          - name: MyCustomPlugin
        disabled:
          - name: "*"
      bind:
        enabled:
          - name: DefaultBinder
    pluginConfig:
      - name: MyCustomPlugin
        args:
          someKey: someValue
```

- Pod example using this scheduler

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  schedulerName: my-scheduler
  containers:
    - name: app
      image: nginx
```

##### 🧠 What’s Going On

- schedulerName: You can match this in your Pod spec using schedulerName: my-scheduler.
- plugins: Enables or disables specific scheduling plugins at various extension points.
- pluginConfig: Supplies arguments to plugins, including your custom plugin.
- MyCustomPlugin: This is a placeholder name for a scheduler plugin you’ve developed.

## Resource limits

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: manual-claim
spec:
  containers:
    - name: simple-webapp
      image: nginx
      ports:
        containerPort: 80
      resources:
        requests:
          mem: "250Mi"
          cpu: "100mi"
        limits:
          memory: "1Gi"
          cpu: 1
```

- The scheduler looks for a pod with enough resources to deploy the pod.
- CPU can have decimal values
  - 0.1 => 100 mili
  - 1 CPU can mean 1 vCPU in AWS
- resources (requests)
  - Definition: The minimum amount of CPU or memory that a container is guaranteed to get.
  - Purpose: Used by the scheduler to decide where to place the pod.
  - Effect: If a node doesn’t have the requested resources available, the pod won’t be scheduled there.
- limits
  - Definition: The maximum amount of CPU or memory a container is allowed to use.
  - Purpose: Prevents a container from over-consuming resources.
  - Effect:
    - For CPU: If it exceeds the limit, it’s throttled.
    - For Memory: If it exceeds the limit, it gets killed (OOMKilled).
- In a Pod with multiple containers, the `resources.requests` and `resources.limits` are assigned per container, not per Pod.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-example
spec:
  containers:
    - name: app
      image: my-app
      resources:
        requests:
          cpu: "100m"
          memory: "200Mi"
        limits:
          cpu: "200m"
          memory: "400Mi"

    - name: sidecar
      image: my-sidecar
      resources:
        requests:
          cpu: "50m"
          memory: "100Mi"
        limits:
          cpu: "100m"
          memory: "200Mi"
```

- Exceed Limits
  - In a CPU, the container cannot exceed the CPU limits.
  - If the pod starts to consume constantly more memory then set, the pod will be terminated with OOM (out of memory) error.
- It is possible to combine the requests and limits

| Requests Set | Limits Set | Scheduling Behavior                    | Runtime Behavior                                                        |
| ------------ | ---------- | -------------------------------------- | ----------------------------------------------------------------------- |
| ✅ Yes       | ✅ Yes     | Pod is scheduled based on requests     | Container is throttled (CPU) or OOMKilled (memory) if it exceeds limits |
| ✅ Yes       | ❌ No      | Pod is scheduled based on requests     | Container can use **unlimited** resources (up to node capacity)         |
| ❌ No        | ✅ Yes     | Pod might not be scheduled efficiently | Container is throttled or OOMKilled if it exceeds limits                |
| ❌ No        | ❌ No      | Best-effort pod, lowest priority       | Can use any free resources but **can be evicted** anytime               |

- Requests set and no limits is the ideal setup, athough sometimes it is necesary to set the limits.

#### 🔍 Detailed Breakdown

---

##### ✅ Requests + ✅ Limits

- **Best practice** in production.
- Pod gets scheduled on a node with at least the amount of CPU/memory in the request.
- Container cannot exceed the specified limit at runtime.
- **CPU overuse**: throttled.
- **Memory overuse**: OOMKilled.

---

##### ✅ Requests + ❌ Limits

- Pod gets scheduled normally.
- Container can consume **more than requested**, even up to the node’s free capacity.
- But there’s **no upper bound** – risk of one container starving others.
- Good for **batch jobs** or **low-priority workloads** that can scale opportunistically.

---

##### ❌ Requests + ✅ Limits

- Kubernetes doesn't know how much to reserve for scheduling.
- May overcommit nodes (since there's no reservation).
- **Can cause scheduling delays** or poor balancing.
- Limits are still enforced at runtime – the container will be **throttled or killed** if it exceeds them.

---

##### ❌ Requests + ❌ Limits

- This is a **BestEffort** pod.
- Kubernetes won’t reserve **any resources**.
- Scheduled **only if resources are free** at the moment.
- Will be the **first to get evicted** under memory pressure.
- Useful for **non-critical** background tasks.

---

##### 📌 Summary Table

| Quality of Service (QoS) Class | Requests | Limits    | Description                      |
| ------------------------------ | -------- | --------- | -------------------------------- |
| **Guaranteed**                 | ✅ Same  | ✅ Same   | Best; stable and protected       |
| **Burstable**                  | ✅ Yes   | ✅ Yes/No | Flexible; for variable workloads |
| **BestEffort**                 | ❌ No    | ❌ No     | Worst; no guarantees             |

- You can set defaults limits for each container created with a 'LimitRange` object.
  - This is set at namespace level.
- This limits will only be enforced for new pods.

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: resource-limits
  namespace: dev
spec:
  limits:
    - default:
        cpu: "500m"
        memory: "512Mi"
      defaultRequest:
        cpu: "250m"
        memory: "256Mi"
      max:
        cpu: "1"
        memory: "1Gi"
      min:
        cpu: "100m"
        memory: "128Mi"
      type: Container
```

- Set the limits
  - default: If a container has no limits defined, it gets these.
  - defaultRequest: If a container has no requests, these will be applied.
  - max: Users cannot set limits above this.
  - min: Users must request at least this.
  - type: Container: Applies to each container, not the whole Pod.
- To restrict the total amount of resources that can be consumed by applications, there is ResourceQuotas.

```yaml
apiVersion: v1
kind: ResourceQuotas
metadata:
  name: ResourceQuotas
spec:
  hard:
    requests.cpu: 4
    requests.memory: 4Gi
    limits.cpu: 10
    limits.memory: 10Gi
```

## Storage

### Docker Storage

#### Layered Architecture

- When you build a Docker image using a Dockerfile, each instruction creates a new layer in the final image. These layers stack on top of each other — like a cake — and together they form the complete image.

```Dockerfile
FROM ubuntu:22.04             # Layer 1: Base image
RUN apt-get update            # Layer 2
RUN apt-get install -y nginx  # Layer 3
COPY index.html /usr/share/nginx/html  # Layer 4
```

🔁 Layer Caching

- Docker caches each layer.
- If you rebuild and don’t change a layer, it reuses the cache.
- This makes rebuilds much faster and more efficient.

🧹 Storage Efficiency

- Shared base layers (like ubuntu:22.04) are reused across images.
- Reduces duplication on disk.

📦 Image Distribution

- When pushing/pulling images, only new layers are sent.

- 🔄 Writable Container Layer
  - When you start a container:
    - Docker adds a read-write layer on top of the image.
    - Any file changes (writes, deletions, etc.) happen there.
    - The image layers below stay unchanged (read-only).
  - Faster deployment across nodes (especially in Kubernetes or CI/CD).
- Common Docker storage drivers: AUFS, ZFS, BTRFS, Device Mapper

##### Volumes

- Volumes are Docker's way of persisting data outside the container's ephemeral filesystem.
- By default, anything written inside a container is lost when the container stops or is removed.
- Volumes solve that by storing data on the host machine, outside the container lifecycle.
- Volumes are not handled by storage drivers. They are handled by volume drivers plugins.
  - Azure File Storage, Convoy, GlusterFS, Portworx, VMWare vSphere Storage, etc.

```bash
docker volume create data_volume
```

The directory is created in `/var/lib/docker/volumes/data_volume`.

```bash
docker run -d -v data_volume:/var/lib/mysql mysql
```

- Mounts `data_volume` to the path `/var/lib/mysql` inside mysql container.
- When mounting, it is possible to define a specific volume driver.

```bash
docker run -it \
 --name mysql \
 --volume-driver rexray/ebs \
 --mount src=ebs-vol, target=/var/lib/mysql \
 mysql
```

Link for reference: [https://docs.docker.com/storage/bind-mounts/#choose-the--v-or---mount-flag](https://docs.docker.com/storage/bind-mounts/#choose-the--v-or---mount-flag)

- Bind mounts mounts a directory from any location in the host filesystem.

```bash
docker run -v /host/path:/container/path <image>
```

| Feature                             | **Bind Mount**                               | **Volume Mount**                                |
| ----------------------------------- | -------------------------------------------- | ----------------------------------------------- |
| **Storage Location**                | Any path on the **host filesystem**          | Managed by Docker in `/var/lib/docker/volumes/` |
| **Managed By**                      | You (manually)                               | Docker                                          |
| **Host Path Must Exist?**           | ✅ Yes                                       | ❌ No (Docker creates it)                       |
| **Isolation from Host**             | ❌ Low (direct access to host files)         | ✅ High (Docker manages the storage)            |
| **Portability**                     | ❌ Tied to specific host paths               | ✅ Portable across systems                      |
| **Permissions Management**          | Manual (can be tricky)                       | Handled by Docker                               |
| **Backups**                         | Must be handled manually                     | Easier with `docker volume` commands            |
| **Performance (Linux)**             | Slightly better (no Docker overlay involved) | Slightly slower, but negligible in most cases   |
| **Typical Use Cases**               | Development, config injection                | Databases, persistent container data            |
| **Docker Compose Compatibility**    | ✅ Supported                                 | ✅ Supported                                    |
| **Data Sharing Between Containers** | Yes (via path)                               | Yes (via named volume)                          |

<https://kubernetes.io/docs/concepts/storage/volumes/#emptydir>

### Container Storage Interface

- The **Container Storage Interface (CSI)** is a (universal) **standardized API** that allows **container orchestration systems** (like Kubernetes) to **interact with storage systems** in a consistent way.
  - CSI lets **Kubernetes (or any other orchestrator)** plug in and manage **different storage solutions** (like AWS EBS, Ceph, OpenEBS, NFS, etc.) **without hardcoding support** for each.
  - CSI defines a set of RPC that will be called by the container orchestractor, and this must be implemented by the storage driver (kubernetes, mesos, etc.).
    - should call to provision new volume (Create a volume)
    - should call to delete a volume (Delete a volume)
    - should call to place a workload that uses the volume onto a node (ControllerPublishVolume)
  - Storage drivers should (portworx, Amazon EBS, GlusterFS, etc):
    - provision a new volume
    - delete a volume
    - make the storage available on the node
- The **Container Runtime Interface (CRI)** is a **standard API** that allows **Kubernetes** to communicate with different **container runtimes** (like containerd, CRI-O, Docker via shim, etc.).
  - Kubernetes doesn’t directly manage containers — instead, it talks to a **container runtime** using CRI to:
    - Start / stop containers
    - Pull images
    - Manage logs
    - Handle networking
- The **Container Network Interface (CNI)** is a **specification and set of libraries** for configuring **network interfaces in Linux containers**.
  - CNI is what Kubernetes (or other container orchestrators) uses to **set up networking for containers** — like assigning IP addresses, setting up routes, and applying network policies.

### **Ephemeral Storage:**

- **Does not persist** across restarts.
  - Example: `emptyDir`, used for temporary storage or fast caching.
  - **emptyDir** is a volume created when a Pod is assigned to a node. It's initially empty, and all containers in the Pod can access and modify the files in it, even if mounted at different paths.
  - **Data loss**: When the Pod is removed, the data in the emptyDir is deleted. However, **container crashes do not affect the data**.
- **Use cases**:
  - Scratch space (e.g., disk-based merge sort).
  - Checkpointing for crash recovery.
  - Holding temporary files for content management.
- **Storage options**:
  - By default, stored on the node's medium (disk, SSD, or network storage).
  - If `emptyDir.medium` is set to "Memory", it uses a **tmpfs (RAM-backed filesystem)**, which is a high-performed cache area.
  - A **size limit** can be specified for storage, and it will be limited by the node's ephemeral storage. If no size is specified, memory-backed volumes use the node's allocatable memory.
- Demonstrates an `emptyDir` volume backed by **memory**, showcasing its speed with a performance test using `dd`.

### **Persistent Storage:**

<https://kubernetes.io/docs/concepts/storage/persistent-volumes/>
<https://kubernetes.io/docs/concepts/storage/dynamic-provisioning/>

- **Persists** even after a pod is removed.
- Discusses **Storage Classes, Persistent Volumes (PV), and Persistent Volume Claims (PVC)**.
  - When provisioning storage, there are 3 aspects to take into account
    - **Storage classes:** defines the type and characteristics of storage provided by the cluster. It allows administrators to describe the "classes" of storage they offer.
    - **Persistent Volumes:** represents a piece of storage in the cluster that has been provisioned by the administrator or dynamically using a StorageClass.
      - It is a cluster-wide pool of storage volumes configured to be used by apps on the cluster.
      - The user can select the storage using PVC.
    - **Persistent Volume Claims:** A **request for storage by the user**. It specifies the amount and characteristics of storage needed by a pod.
      - Once bound to a PV, the PVC can be used to access the storage.
        - PVC are namespaced objects.
    - A PVC is bound to a single PV.
    - If PVC is deleted, by default the volume is retained, or it can be deleted automatically
      `persistentVolumeReclaimPolicy: Delete`
    - Or set to `recycle`. The data is deleted before being used to other claims.
- Manual vs. dynamic provisioning:
  - **Manual**: We create a PV and PVC is created explicitly.
  - **Dynamic**: We create a PVC against a StorageClass. This will create a PV automatically.
- Volumes are created automatically when PVC is created.

  - These approaches have unique characteristics in Reclaim Policies.
    - **Delete**: The volume will be deleted on the release from its claim
    - **Retain**: The volume will be left in its current phase. This is the default policy.
    - **Recycle**: The files are deleted and the persistent volume is reused. Deprecated since Kubernetes 1.9 in favor of external storage solutions that handle cleanup process.
  - Ceph is a Kubernetes storage for Openshift
    - Ceph is a highly scalable and flexible storage solution that provides comprehensive storage capabilities, including block, file, and object storage, in distributed systems.
    - It uses a decentralized architecture to store data across multiple nodes, making it highly fault-tolerant and reliable.
    - [ceph](https://www.redhat.com/en/technologies/storage/ceph)
    - [ceph example](https://docs.openshift.com/container-platform/3.11/install_config/storage_examples/ceph_example.html)

- Local Persistent Volume

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
 name: pv-voll
spec:
 accessModes:
  - ReadWriteOnce
 capacity:
  storage: 1Gi
 hostPath:
  path: /tmp/data
```

- AWS Persistent Volume

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
 name: pv-voll
spec:
 accessModes:
  - ReadWriteOnce
 capacity:
  storage: 1Gi
 awsElasticBlockStore:
  volumeID: <volume-id>
  fsType: ext4
```

- Persistent Volume Claim

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
 name: myclaim
spec:
 accessModes:
  - ReadWriteOnce
 resources:
  requests:
   storage: 500Mi
```

- The PVC requests 500Mi, but the volume is 1Gi.
  - Overprovisioning is common, especially when dynamic provisioning creates standard-size volumes (like 1Gi or 10Gi), and apps don’t always need that much.

### Storage Classes

- **StorageClasses** in Kubernetes define **how** and **what kind** of **storage** to provision **dynamically** when a user requests a PersistentVolumeClaim (PVC).
  - It acts like a blueprint for **creating volumes on demand**.
  - When a storage class is created, a PV is created.

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: aws-storage
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp3
  encrypted: "true"
```

- Define the storage class name in the PVC definition

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
 name: myclaim
spec:
 accessModes:
  - ReadWriteOnce
 storageClassName: aws-storage
 resources:
  requests:
   storage: 500Mi
```

## StatefulSets

- **StatefulSets** are workload API objects that are used for managing stateful applications, differing from deployments and replica sets, which are typically for stateless workloads.
- StatefulSets are ideal for applications like **databases, caches, or other services requiring state persistence and stable identities across pod restarts and updates.**

### Key Points

- ### Deployments and ReplicaSets (Stateless)
	- **Deployments** and **ReplicaSets** are **stateless** by design.
	- In a Deployment, all pods are **interchangeable** and **share** any persistent storage (if configured).
	- Pods can be **replaced at any time** during rolling updates (new pod comes up before the old pod is terminated), meaning **no stable identity** is guaranteed.
	- Each **ReplicaSet** has its own identifier, but **pods** under a ReplicaSet have **randomized names**.
	- Although Deployments can use **Persistent Volumes (PVs)**, the storage is typically **shared** across all pods rather than assigned uniquely.

### StatefulSets (Stateful)

- **StatefulSets** manage **stateful applications** by maintaining **stable, unique identities** for each pod.
- Each pod gets:
    - A **persistent, unique network identity** (DNS and hostname)
    - A **dedicated Persistent Volume Claim (PVC)**, mapped to a unique Persistent Volume (PV)
- Pods are named **sequentially**, starting from zero and prefixed by the StatefulSet name (e.g., `my-statefulset-0`, `my-statefulset-1`, etc.).
- Pods retain their identity across **rescheduling and restarts** — identity is "sticky."
- Persistent storage is **decoupled from pod lifecycle**: data survives even if a pod is deleted or recreated.

### Networking: Headless Service

- **StatefulSets require a `serviceName`** field pointing to a **headless Service** (a Service with `clusterIP: None`).
- A **headless Service** provides **stable DNS entries** without a load-balancer IP, enabling direct pod-to-pod communication.
- Example DNS for a StatefulSet pod:
```pgsql
<pod-name>.<service-name>.<namespace>.svc.cluster.local
```

### When to Use StatefulSets

- Applications requiring **stable, unique network identities** (e.g., databases, messaging queues).
- Applications needing **dedicated persistent storage** for each instance.
- Applications needing **ordered deployment, scaling, and rolling updates** (e.g., master-slave databases).
- Systems where **identity preservation** is critical for consistency and recovery.
- A headless service is a special type of service that doesn't have an IP address or port associated with it. Instead, it provides a way to access the pods in the StatefulSet using DNS. By defining a serviceName, you can create a stable network identity for the StatefulSet, which allows other components in your application to communicate with it reliably.

#### Steps to Use StatefulSets

1. **Creating a StatefulSet**:
   - You can start with a **deployment YAML** and modify it to create a StatefulSet.
   - Ensure a **service name** is added (optional but recommended for stable network IDs).
   - Change the `kind` to `StatefulSet` in the YAML.
2. **Network Identity**:
   - Each pod in the StatefulSet gets a name based on the StatefulSet name (e.g., `nginx-0`, `nginx-1`, etc.).
   - For stable networking, use a **headless service** (with `clusterIP: None`), which allows pods to have DNS names like `nginx-0.nginx.default.svc.cluster.local`.
3. **Rolling Updates**:
   - The StatefulSet supports **ordered** updates. Pods are updated one by one in a controlled sequence.
   - The `partition` field can be set to **control which pods are updated during a rolling update**. E.g., setting `partition: 2` ensures that pods `nginx-0` and `nginx-1` remain unchanged while `nginx-2` is updated.
     - The `partition` value indicates the starting point for a rolling update, specifying which pods should be updated first. By setting the partition value to a specific number, you can control which subset of pods are updated initially, allowing for more fine-grained control over the rollout process.
4. **Persistent Storage**:
   - Each pod in a StatefulSet can have its own persistent storage, with dynamic provisioning.
   - If the StatefulSet is deleted, the **PVC** and **PV** remain, allowing the StatefulSet to **reuse the same storage** when it is recreated, preserving data between pod restarts.
5. **Example**:
   - Create a StatefulSet YAML from a deployment template, adding a service name and changing `kind` to `StatefulSet`.
   - After deploying the StatefulSet, you can see that each pod is named `nginx-0`, `nginx-1`, etc., and each has its own persistent volume claim.
   - Even after pod deletion and recreation, the data persists in the associated volume.

### Key Features of StatefulSets

- **Stable Identity**: Pods are named sequentially (e.g., `nginx-0`, `nginx-1`).
- **Persistent Storage**: Each pod can claim its own persistent storage (PV/PVC).
- **Ordered Deployments and Updates**: Pods are managed and updated in a specified order.
- **Headless Service**: Needed for stable network identities.

## Network Policies

- Helps to **isolate pods from each other and control their communication.**
- We can restrict access to pods, namespaces, and IP blocks using network traffic rules that allow or deny communication based on labels, ports, and other criteria.
- An 'Ingress' rule defines the types of traffic permitted to enter pods from external sources or other pods.
-  NetworkPolicy in Kubernetes becomes **effective after it is applied**. When a NetworkPolicy is created and applied to a cluster, it **defines the rules for traffic flow between pods and services**. The policy takes effect immediately after it is successfully applied, allowing or blocking traffic according to its specifications.
- When a pod is created using `kubectl run` in Kubernetes, a label with key `run` and `value` is automatically assigned to it
- `key` is `run`
- `value` is the name of the deployment or replica set that manages the pod.

```bash
kubectl describe pod/curl | more
Name:             curl
Namespace:        default
Priority:         0
Service Account:  default
Node:             worker-1/172.18.0.4
Start Time:       Sun, 16 Feb 2025 21:41:02 +0000
Labels:           run=curl
```

### Container Network Interface (CNI)

- CNI plugin is essential for the enforcement of NetworkPolicies in Kubernetes.
- CNI plugins are responsible for **configuring and managing the network interfaces of containers**, which includes enforcing NetworkPolicies.
- When a NetworkPolicy is created, the CNI plugin is responsible for **implementing the policy by configuring the necessary network rules to allow or deny traffic**.
- When **multiple NetworkPolicies** are applied to a set of pods in Kubernetes, their **effects are additive and cumulative**. This means that each policy builds upon the previous ones, creating a more complex set of rules that govern network traffic to and from the pods. The resulting policy is the combination of all the individual policies.
- By **default**, a pod in a Kubernetes cluster without any NetworkPolicies applied to it **can send and receive traffic from any source**. This means that there are no restrictions on incoming or outgoing traffic, allowing the pod to freely communicate with other pods, services, and external networks.

## Pod Disruption Budgets

- PDB vs Replicas
- Replicas **ensure that a specified number of replicas** (i.e., copies) of a pod are **running** at any given time, which helps **maintain availability under normal operations**.
- **PDBs protect against disruptions** by **specifying the maximum number of pods that can be terminated** within a certain time period, ensuring that a **minimum number of replicas remain available even during disruptions**.
- PDBs ensure **high availability** of applications during **voluntary disruptions**, such as maintenance or node autoscaling. Unlike replicas, which maintain availability under normal conditions, PDBs prevent excessive disruption during planned operations.
- PDB improves application stability during maintenance by **ensuring that a minimum number of pods remain available when nodes undergo voluntary disruptions**, such as upgrades, autoscaling, or planned maintenance, even when nodes are being drained or maintained.
- **Voluntary disruptions** in Kubernetes refer to **intentional actions taken by administrators**, such as maintenance or upgrades, that may cause pods to be terminated or become unavailable. These disruptions are planned and executed by humans, hence the term "voluntary".
- When you use the `kubectl drain` command on a node, it removes all running pods from that node and marks it as unschedulable to prevent new pods from being scheduled on it. This is typically done before performing maintenance or upgrades on the node.

### Demonstration

1. **Deployment Setup:**
   - A deployment with **five replicas** is created, distributing pods across multiple nodes.
   - Nodes are **cordoned** (marked unschedulable), and pods are deleted.
   - Replicas automatically reschedule on available nodes.
2. **Impact of Draining Nodes:**
   - Running `kubectl drain` on a node removes pods, potentially leaving the deployment at **zero** replicas.
   - In real applications, this could cause downtime and trigger alerts.
3. **Using a Pod Disruption Budget (PDB):**
   - A **PDB is created** to enforce a minimum of **two available pods**.
   - When attempting to drain nodes, Kubernetes **prevents eviction** of critical pods.
   - The operation only proceeds when conditions allow at least two pods to remain active.

## Security

### Security Context

- **Security Contexts**: Kubernetes settings that **define access control and privileges for pods or containers**.
- We modify a pod to run as a non-root user, preventing escalation to root access, improving security for each pod. They allow you to specify the user ID, group ID, and other security-related settings for a container.
- This [image](https://github.com/spurin/rootshell) has a Dockerfile that creates a container image with a custom `rootshell` binary. In this example, we use `runAsUser` and `runAsGroup` to

```bash
% docker run -it --rm --user 1000:1000 spurin/rootshell:latest
nonpriv@00eac4da6562:/$
nonpriv@00eac4da6562:/$ id
uid=1000(nonpriv) gid=1000(nonpriv) groups=1000(nonpriv)
nonpriv@00eac4da6562:/$ /rootshell
root@00eac4da6562:/#
root@00eac4da6562:/# id
uid=0(root) gid=1000(nonpriv) groups=1000(nonpriv)
```

- **runAsUser**: This specifies the user ID (UID) that the container should run as. By setting this, you can ensure that the container runs with a non-root user, even if the container image itself is designed to run as root by default. This is a key measure in preventing privilege escalation.
- **runAsGroup**: Similar to **runAsUser**, **runAsGroup** specifies the group ID (GID) that the container should run as. This ensures that the container’s processes belong to a specific group, further enhancing control over its access and privileges.
- In the demonstration, the **runAsUser** and **runAsGroup** are set to 1000 (the UID and GID of the user defined in the Docker image). By configuring these fields in the pod's security context, the pod is instructed to run with a specific user and group, rather than as root. This is crucial for restricting access and reducing security risks.
- These settings are part of the broader security context configuration, helping administrators enforce **least privilege** access in Kubernetes environments.
- Setting `allowPrivilegeEscalation` to false in the container's security context prevents the escalation of privileges within the container. When this field is set to false, the container cannot gain additional privileges and is restricted to its current user ID. This helps prevent privilege escalation attacks where an attacker gains elevated privileges within a container.

### Admission Controller

- **Admission Controllers**: Since Kubernetes 1.25, **admission controllers replace deprecated pod security policies to enforce security at the cluster level**. They act as **gatekeepers**, preventing actions like root container execution and process escalation, or enforce critical security policies to reduce attack vectors.
- They intercept requests to the API server and can modify or reject them based on custom rules and policies. This allows cluster administrators to enforce specific requirements, such as security checks or resource quotas, before allowing pods to be created or modified.
- Tools like **Kyverno** and **OPA Gatekeeper** provide additional policy enforcement.
- Pod Admission Controllers in Kubernetes are used to enforce policies on incoming pods, such as validating or mutating their configuration before they are created. Kyverno and Open Policy Agent (OPA) Gatekeeper are two popular options that **allow administrators to define custom policies using YAML or Rego**, respectively, which can then be applied to incoming pods.
- **Other Tools**:
- **Falco**: A runtime security tool that **monitors abnormal behavior and security threats**.
- Falco is an open-source runtime security project that integrates with Kubernetes to identify abnormal behavior and potential security threats. It uses a behavioral approach to detect anomalies in system calls, network activity, and other system interactions, providing real-time alerts and notifications for security teams.
- **Kubescape**: A security platform that **scans for vulnerabilities and misconfigurations**. It provides a comprehensive security scan of the cluster, identifying potential vulnerabilities and misconfigurations that could be exploited by attackers.
- **[OIDC (OpenID Connect)](https://medium.com/@extio/kubernetes-authentication-with-oidc-simplifying-identity-management-c56ede8f2dec)**: An authentication protocol based on OAuth 2.0, **recommended for large-scale Kubernetes clusters.**
- OpenID Connect (OIDC) is an i**dentity layer built on top of OAuth 2.0** that provides a **standardized way to authenticate users in large-scale deployments**.
- OIDC provides a scalable and secure authentication mechanism that can be easily integrated with Kubernetes, making it a recommended protocol for enhanced authentication.

### 4Cs

- 4C’s of Cloud Native Security, namely Cloud, Cluster, Container, Code.
- This sequence represents the layers of abstraction in cloud native environments, starting from the broadest scope (cloud providers) to the narrowest scope (application code).
- Each layer builds upon the previous one, and securing each layer is crucial for overall security.
- Each endpoint acts as a layer from the outside-in and is a security best practice in Cloud Native Computing.
- Each subsequent layer acts as a reinforcement for the layer within. For example, Code would benefit from the security implementations of Container, Cluster and Cloud respectively.

## Helm and Helm charts

- Helm simplifies Kubernetes application management using **Helm Charts** (packages with pre-configured Kubernetes resources) by providing a way to easily install, upgrade, and manage applications on a Kubernetes cluster.
- They provide a way to package, distribute, and manage applications, making it easier to deploy and manage complex applications in a Kubernetes environment.
- It uses a templating engine to generate Kubernetes resources from templates, making it easier to manage complex applications.
- Think of Helm as the **APT** or **YUM** of Kubernetes, allowing easier deployment, updates, and management of applications.
- You can package charts for version control and easy distribution.
- Helm enables easier scaling and upgrading of Kubernetes applications in real-world scenarios.

1. **Creating a Helm Chart:**
   - Create a basic chart using: `helm create flappy-app`.
   - Customize:
     - **`Chart.yaml`** → Add name, description, and version (following semantic versioning).
     - **`values.yaml`** → Set the Docker image to `spurin/flappy-doc:latest`. Disable unnecessary service accounts.
2. **Packaging and Deploying:**
   - Package the chart with: `helm package`.
   - Deploy using: `helm install flappy-app ./flappy-app-0.1.0.tgz`
   - Verify deployment with:
     - `kubectl get deployments`
     - `kubectl get pods`
     - `kubectl get services`
   - Use port forwarding to access the game locally via `kubectl port-forward`.
3. **Exploring and Cleaning Up:**

   - List running charts: `helm list`
   - Uninstall: `helm uninstall flappy-app`
   - Remove the project directory for cleanup.

## Service Meshes

Microservices have revolutionized software development by providing efficiency, scalability, and resilience. They consist of small, independently deployable units, making them ideal for diverse deployment environments, including hybrid and multi-cloud setups. However, as microservices grow, managing service-to-service communication becomes complex.

Service meshes are for managing complex microservice architectures in cloud-native environments. They improve security, observability, and reliability. Exploring solutions like **Istio**, **Linkerd**, or experimenting with the **SMI** standard is an excellent next step for deepening your understanding.

### Role of Service Meshes

A **service mesh** simplifies this complexity by ensuring efficient, reliable, and secure communication between microservices, especially as applications scale.

![[Kubernetes Deep Dive Sidecar proxy.png]]
A service mesh consists of:

- **Data Plane**: Manages internal network traffic between services using either:
  - **Sidecar proxies** (e.g., Istio, Linkerd) attached to each microservice for fine-grained traffic control, providing additional functionality such as traffic management and security. This pattern enables the injection of additional capabilities into the data path without modifying the application code.
  - **Host node proxies** (e.g., Traffic Mesh) installed at the node level for simpler management. They route network traffic between external client, nodes, and pods.
- **Control Plane**: Acts as the management layer, configuring and directing proxies across the system. It dictates the policies for traffic routing, security, and monitoring and provides the instructions that the proxies follow when handling requests between services.

### Key Benefits of Service Meshes

- **Security**: **Mutual TLS ensures two-way verification for secure communication**. It ensures that only authorized services can communicate with each other, reducing the risk of unauthorized access or man-in-the-middle attacks.
- **Access Control**: Fine-grained policy management.
- **Observability**: Tracing and monitoring tools provide deep insights.
- **Reliability**: Features like rate limiting and circuit breaking improve system resilience.

### Standardization with SMI (Service Mesh Interface)

Kubernetes supports the **Service Mesh Interface (SMI)**, which **provides a unified API standard for various service mesh solutions**. By defining a set of common APIs, SMI enables users to manage and configure their Service Mesh environments in a consistent manner, regardless of the underlying implementation. It allows developers to:

- Manage traffic, access control, and metrics across different meshes.
- Avoid vendor lock-in with consistent functionalities across implementations.

## Data Plane and Control Plane in a service mesh vs normal kubernetes

### 1️⃣ In a Service Mesh

A **service mesh** introduces a dedicated **control plane** and **data plane** to manage communication between microservices efficiently.

#### 🛠 Control Plane (Management Layer)

- **Manages and configures the data plane proxies** across the mesh.
- Enforces **security policies, routing, observability, and traffic shaping**.
- Examples:
  - **Istio** → **Istiod** (control plane) manages Envoy sidecars.
  - **Linkerd** → **Control plane** manages Linkerd proxies.

#### 🚀 Data Plane (Traffic Handling Layer)

- **Intercepts and processes network traffic** between services.
- Enforces **routing, retries, load balancing, mTLS (mutual TLS), and telemetry**.
- Typically implemented as **sidecar proxies** (e.g., **Envoy**, **Linkerd Proxy**).

🖼 **Example in Istio:**

```yaml
# Istiod (Control Plane)
# Envoy (Data Plane - injected as a sidecar in every Pod)
```

- **Istiod** manages service discovery, security policies, and configuration.
- **Envoy** is a high-performance, open-source edge and service proxy designed to manage microservices communication within a distributed system. 
	- **Envoy proxies** inside Pods handle all network traffic.

---

### 2️⃣ In a Normal Kubernetes Pod

In a regular **Kubernetes cluster (without a service mesh)**, the **data plane and control plane** have different meanings.

#### **🛠 Control Plane (Kubernetes Cluster Management)**

- **Manages the cluster** (Pods, scheduling, API, etc.).
- Components:
  - **kube-apiserver** → Entry point for all API requests.
  - **kube-controller-manager** → Handles controller loops.
  - **kube-scheduler** → Assigns Pods to nodes.
  - **etcd** → Stores cluster state.

#### 🚀 Data Plane (Workloads Running on Nodes)

- The **data plane consists of worker nodes** running application workloads.
- Components:
  - **kubelet** → Ensures Pods are running.
  - **Container runtime (e.g., CRI-O, containerd, Docker)** → Runs containers.
  - **kube-proxy** → Manages network rules.

🖼 **Example in Kubernetes:**

```yaml
# Control Plane:
# - kube-apiserver
# - etcd
# - kube-controller-manager
# - kube-scheduler
# Data Plane:
# - Worker nodes
# - kubelet
# - Pods (running apps)
# - kube-proxy (handles networking)
```

### 🔍 Key Differences

| Feature           | **Service Mesh**                               | **Normal Kubernetes Pod**                                      |
| ----------------- | ---------------------------------------------- | -------------------------------------------------------------- |
| **Control Plane** | Manages proxies, policies, security            | Manages cluster, scheduling, API                               |
| **Data Plane**    | Proxies network traffic between services       | Runs actual application workloads                              |
| **Example**       | Istio (**Istiod** = control, **Envoy** = data) | Kubernetes (**API server** = control, **Worker nodes** = data) |
| **Purpose**       | Service-to-service communication               | Running containerized applications                             |
|                   |                                                |                                                                |
## Questions

- Which statement accurately describes the behavior of a static pod in a Kubernetes cluster?
  - When a kubelet creates a static pod, it also creates a read-only mirror object in the Kube API server.
- How can you taint a node in Kubernetes using the kubectl command?
  - kubectl taint nodes node-name key=value:taint-effect,kubectl taint node node-name key=value:taint-effect
- How can you change the default resource limit for containers in Kubernetes?
  - Add a ‘limit’ section under the resources section in the pod definition file.
- Where are the labels defined for the pods in a ReplicaSet?
  - Under the template section of the Replicaset definition file.
- What approach can be used to specify the path for static pod definition files when not directly specifying it in the kubelet.service file?
  - Creating a separate config file and defining the directory path as ‘staticPodPath’
- How do you label a node in Kubernetes?
  - kubectl label nodes <node_name\> <label-key\>=<label-value\>
- The Kubernetes API server is aware of the static pods created by the kubelet.
  - True
- What does the taint effect "NoSchedule" indicate in Kubernetes?
  - Pods will not be scheduled on the node.
- To connect a ReplicaSet to the desired pods, which field in the ReplicaSet specification is used to match the labels defined on the pod?
  - selector
- What can be done to manually schedule a pod to a specific node in Kubernetes when there is no scheduler monitoring and scheduling the nodes?
  - Set the NodeName field in the pod manifest to the desired node name.
- How can you limit a pod to run on a specific node in Kubernetes?
  - Add a label to the node and use nodeSelector in the pod's spec section.
- Which of the following statements is true regarding service accounts in Kubernetes version 1.24?
  - When a service account is created, it no longer automatically creates a secret or a token access secret.
- What is a service account in Kubernetes?
  - An account used by an application to interact with a Kubernetes cluster
- Which of the following statements is true about the default behavior of kubectl tool?
  - kubectl looks for a file named config under a directory .kube in the user's home directory.
- If you create a cluster role for namespaced resources, what access do the users get?
  - Access to all resources across all namespaces in the cluster
- How can you choose not to automatically mount a service account token to a Kubernetes pod?
  - By setting the automountServiceAccountToken field to false in the pod's spec
- What are the characteristics of tokens generated by the Kubernetes Token Request API?
  - Audience bound, time bound, and object bound
- Can you edit the service account of an existing deployment in Kubernetes?
  - Yes, by modifying the deployment manifest file and updating the service account field.
- In the context of a Docker image name in a Kubernetes pod definition file, when the image name is provided without a prefix, what does the prefix usually represent, and what is the default value assumed if none is provided?
  - It specifies the user or account name, and the default value is library.
- Which of the following statements is true about using KubeConfig in kubectl?
  - KubeConfig is a file that contains Kubernetes configuration information.
- Which security mechanism is used to ensure secure communication between the various components of a cluster, including the ETCD cluster, Kube controller, API server, and worker node components?
  - TLS encryption
- What is the purpose of the named group APIs in Kubernetes?
  - To organize and provide access to newer features
- What happens when you configure security settings at both the pod and container levels in Kubernetes?
  - The settings at the container level override the settings at the pod level.
- Which of the following Kubernetes resources are cluster-scoped?
  - Persistent volumes
- Which field can be used to provide the contents of a certificate itself instead of using certificate-authority and the path to the file?
  - certificate-authority-data
- What is the purpose of the "library" in the image name (library/nginx)?
  - It is the default account where Docker's official images are stored.
- Which of the following solutions support Kubernetes network policies?
  - Kube router, calico, and weave-net
- Which of the following measures should be taken to secure a Kubernetes infrastructure and ensure secure access to the cluster's hosts?
  - Disable root access and password-based authentication on the hosts.
- Which of the following modules can be used as part of the authorization process in a Kubernetes environment?
  - Node Authorizater modules and Attribute-based access control (ABAC) modules
- How can we specify the secret to our private registry inside our pod definition file?
  - By specifying the secret under the imagePullSecrets section in the pod definition file
- Which of the following statements is true about the scope of roles and role bindings in Kubernetes?
  - Both roles and role bindings fall under the scope of namespaces.
- What is Docker's default registry?
  - Docker Hub
- Which of the following is true about the format of the sections in the kubeconfig file?
  - Each section is in an array format.
- What command can be used to view the contents of a secret object in Kubernetes?
  - `kubectl describe secret <token-name>`
- What is the role of the node authoriser in Kubernetes cluster?
  - Handles requests from the kubelets on nodes to the kube-apiserver
- What is the kubectl command to change the current context?
  - `kubectl config use-context <context>`
- When configuring security settings at the pod level, what happens to the settings?
  - They apply to all containers within the pod.
- What are the different access modes available for a volume in Kubernetes?
  - readonlymany, readwriteonce, readwritemany
- What is the preferred option to mount a volume in the Docker run command?
  - --mount
- What happens to the bound volume when a persistent volume claim gets deleted in Kubernetes?
  - The bound volume becomes unbound.
- Which of the following are common Container Storage Interface (CSI) drivers used for container orchestrators?
  - Portworx, Amazon EBS, GlusterFs, Dell EMS
- What happens when persistent volume claims are created in Kubernetes?
  - Kubernetes binds the persistent volumes to claims based on the request and properties set on the volume.
- What is the purpose of Docker's writable layer?
  - To store data created by the container
- What happens to the data stored in the container layer when a container is deleted?
  - The data is deleted along with the container.
- Which of the following is responsible for handling Docker volumes?
  - Volume driver plugins
- What is the role of storage drivers in the Container Storage Interface (CSI) standard?
  - To implement the set of remote procedure calls defined by CSI
- What is the purpose of Docker storage drivers?
  - To enable the layered architecture of Docker images
- What happens when you run a container based on a Docker image using the docker run command?
  - A new writeable layer is created on top of the existing image layer.
- Which command is used to create a volume in Docker?
  - docker volume create
- What is the purpose of the Container Storage Interface (CSI)?
  - To support multiple storage solutions to work with kubernetes
- How can you specify a preferred volume driver plugin solution for Docker volumes?
  - Use the --volume-driver option with the docker run command.
- Which significant achievement did the Envoy project accomplish in its timeline with the Cloud Native Computing Foundation (CNCF)?
	- It reached graduation level in 2018.
- Which of the following is true about a proxy service in an application architecture?
	- It manages user authentication and retries failed requests
	- In ensures connections through TLS encryption
- Which component in Istio combines the three control plane components (Citadel, Pilot, and Galley) into a single daemon?
	- Istiod
- In Kubernetes, which storage API resource represents a distinct unit of storage in the cluster, provisioned either by an administrator or dynamically provisioned using storage classes, and can be mounted as volumes in pods?
	- PV
- In the context of containerized applications, which networking approach employs a Sidecar pattern, co-locating an auxiliary container with the primary container, to handle port mapping and traffic management tasks?
	- Service Mesh
- In the evolving landscape of Kubernetes, what is the recommended successor to PodSecurityPolicies (PSP) for enforcing fine-grained security policies?
	- Kubernetes Admission Controllers
- Which component in Kubernetes continuously monitors the state of the cluster, detects any deviations from the desired state, and initiates or requests changes to ensure the cluster remains in the desired state?
	- Controller Manager
- What is the purpose of the kubelet?
	-  In a Kubernetes cluster, the kubelet is responsible for managing the state of worker nodes and ensuring their proper functioning. This contrasts with the controller manager, that runs controllers that watch the **cluster state** and try to move it toward the **desired state**.