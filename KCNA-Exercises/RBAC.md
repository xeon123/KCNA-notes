
###### Default KubeConfig

Let's start by taking a look at our current kubeconfig -

`kubectl config view`

To see the information that is currently hidden, we can add the parameter of --raw -

`kubectl config view --raw`

Let's take a closer look at the certificate-authority-data string, firstly we'll capture this -

```bash
CERTIFICATE_AUTHORITY_DATA=$(kubectl config view --raw | grep certificate-authority-data | awk -F': ' {'print $2'}) && echo $CERTIFICATE_AUTHORITY_DATA
```

We'll decode this using base64 -

`echo $CERTIFICATE_AUTHORITY_DATA | base64 -d`

And we can see the certificate public key using openssl -

`echo $CERTIFICATE_AUTHORITY_DATA | base64 -d | openssl x509 -text --noout`

Review our configuration again -

`kubectl config view --raw`

This time we'll take a look at users, we'll capture the user private key -

```bash
CLIENT_KEY_DATA=$(kubectl config view --raw | grep client-key-data | awk -F': ' {'print $2'}) && echo $CLIENT_KEY_DATA
```

And again, we can decode this with base64 to see the users private key -

`echo $CLIENT_KEY_DATA | base64 -d`

We also have client-certificate-data which is where we enter the main realms of rbac, capture this to a variable -

```bash
CLIENT_CERTIFICATE_DATA=$(kubectl config view --raw | grep client-certificate-data | awk -F': ' {'print $2'}) && echo $CLIENT_CERTIFICATE_DATA
```

We'll decode this, at this point you may see one or more certificates for the client-certificate-data -

`echo $CLIENT_CERTIFICATE_DATA | base64 -d`

And we will decode this using openssl, note how the client-certificate-data contains a subject line with O and CN, O for a Group and CN for a username -

`echo $CLIENT_CERTIFICATE_DATA | base64 -d | openssl x509 -text --noout`

---

###### Cluster Role Bindings

Let's take a look at this from the view of Cluster Role Bindings in RBAC, review the Users, Groups and ServiceAccounts, note that group system:masters has a binding with cluster-admin and a role of ClusterRole/cluster-admin (you can click the Tutorial button to toggle the sidebar if you need more viewing space) -

`kubectl get clusterrolebindings -o wide`

If you wish to see every resource and verb that is available we can do so with kube api-resources -

`kubectl api-resources --sort-by name -o wide | more`

With this knowledge, let's recap the view of clusterrolebindings and how our group, relates to the admin user -

`kubectl describe ClusterRole/cluster-admin`

Let's build our own alternative super user group using the theme of superheroes, we'll start by creating a clusterrole and we'll assign full permission -

`kubectl create clusterrole cluster-superhero --verb='*' --resource='*'`

We'll bind this using a clusterrolebinding and we'll instruct it to bind to the group of cluster-superheroes -

```bash
kubectl create clusterrolebinding cluster-superhero --clusterrole=cluster-superhero --group=cluster-superheroes
```

If we check again with clusterrolebindings and filter our output, we'll see out new variation -

`kubectl get clusterrolebindings -o wide | egrep 'Name|^cluster-'`

We can use kubectl auth can-i to verify permissions, as an example, as our super user account check that we can do access any resource with any verb -

`kubectl auth can-i '*' '*'`

We can expand this further, for example, we could check this against our group with an assigned user of batman -

`kubectl auth can-i '*' '*' --as-group="cluster-superheroes" --as="batman"

As the permissions are assigned at a group level, any user in this group will have the equivalent access, for example -

```bash
kubectl auth can-i '*' '*' --as-group="cluster-superheroes" --as="superman"
kubectl auth can-i '*' '*' --as-group="cluster-superheroes" --as="wonder-woman"
```

---

###### Configuring an RBAC User/Group Manually

We'll see how we can configure a user/group for use with RBAC from scratch, first we'll start by generating an rsa private key for our user batman -

`openssl genrsa -out batman.key 4096`

We'll now create a certificate signing request, we'll reference our private key, an output file and we'll specify the subject information with CN and O for our user and group -

`openssl req -new -key batman.key -out batman.csr -subj "/CN=batman/O=cluster-superheroes" -sha256 `

If we check we'll have two files -

`ls `

And if you wish you can review the files -

```bash
cat batman.key
cat batman.csr```

We'll now structure a CSR signing request, first we'll create variables with our data -

```bash
CSR_DATA=$(base64 batman.csr | tr -d '\n')
CSR_USER=batman
```

And if we check these, both will now be set -

```bash
echo $CSR_DATA
echo $CSR_USER
```

Let's template a Kubernetes Certificate Signing Request as yaml, we'll use placeholders for our variables and will redirect to a file -

```yaml
cat <<EOF > batman-csr-request.yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: ${CSR_USER}
spec:
  request: ${CSR_DATA}
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
EOF
```


And if we check our file, we will see these entries populated -

`cat batman-csr-request.yaml`

We'll apply this request -

`kubectl apply -f batman-csr-request.yaml`

And if we check in Kubernetes we will see this certificate signing request as pending -

`kubectl get certificatesigningrequest`

We can use the shorthand of csr which is more commonly used in these circumstances -

`kubectl get csr`

As our current super user we'll approve this request -

`kubectl certificate approve batman`

And if we check again, we will have an approved csr -

`kubectl get csr`

If we query the signed certificate using json as output, we will be able to see our certificate in base64 -

`kubectl get csr batman -o json`

To directly capture this entry, we'll use jsonpath and will tell it to walk the path of status, then certificate -

`kubectl get csr batman -o jsonpath='{.status.certificate}'`

At the moment this is base64 so let's decode this -

`kubectl get csr batman -o jsonpath='{.status.certificate}' | base64 -d`

And we'll redirect this to a file -

`kubectl get csr batman -o jsonpath='{.status.certificate}' | base64 -d > batman.crt`

And now we have the following files -

`ls -l`

We can use openssl to decode the approved/signed certificate, showing that the subject line contains CN and O -

`openssl x509 -in batman.crt -text -noout`

---

###### Adding Information to a Kubeconfig file

Let's use our existing kubeconfig file as a template -

`cp /root/.kube/config batman-clustersuperheroes.config`

If we take a look, this currently has a lot of information that we don't want our batman user to have -

`cat batman-clustersuperheroes.config`

Let's clean this file, we'll use the KUBECONFIG variable and we'll unset users.default -

`KUBECONFIG=batman-clustersuperheroes.config kubectl config unset users.default`

We'll delete the current context -

`KUBECONFIG=batman-clustersuperheroes.config kubectl config delete-context default`

And we'll unset the current context -

`KUBECONFIG=batman-clustersuperheroes.config kubectl config unset current-context`

Our base file is now simplified -

`cat batman-clustersuperheroes.config`

Let's embed the new information, first set-credentials as batman, we'll pass the certificate data and the private key, we'll also embed this information -

```bash
KUBECONFIG=batman-clustersuperheroes.config kubectl config set-credentials batman --client-certificate=batman.crt --client-key=batman.key --embed-certs=true
```

Set the default context to use the default cluster with a user of batman -

```bash
KUBECONFIG=batman-clustersuperheroes.config kubectl config set-context default --cluster=default --user=batman
```

And use the context of default -

`KUBECONFIG=batman-clustersuperheroes.config kubectl config use-context default`

Now our configuration files looks like the following -

`cat batman-clustersuperheroes.config`

And we can test that this is working, we'll be able to get nodes, create a pod and delete a pod, essentially our user is a super user -

```bash
KUBECONFIG=batman-clustersuperheroes.config kubectl get nodes
KUBECONFIG=./batman-clustersuperheroes.config kubectl run nginx --image=nginx
KUBECONFIG=./batman-clustersuperheroes.config kubectl delete pod/nginx --now
```

---

###### Automating an RBAC Kubeconfig file

We're going to setup a convenient tool for automating kubeconfig configurations, first install prerequisites -

`apt update && apt install -y git jq openssl`

We'll clone the project -

`git clone https://github.com/spurin/kubeconfig-creator.git`

Change into the project directory -

`cd kubeconfig-creator`

And we'll create another user similar to batman, this time we'll create superman -

`./kubeconfig_creator.sh -u superman -g cluster-superheroes`

And because superman is also part of the cluster-superheroes group, we can execute commands as if we are a super user -

`KUBECONFIG=./superman-clustersuperheroes.config kubectl get nodes`

We'll create another user, this time we'll create wonder-woman -

`./kubeconfig_creator.sh -u wonder-woman -g cluster-superheroes`

And again, she will have full access -

`KUBECONFIG=./wonderwoman-clustersuperheroes.config kubectl get nodes`

---

###### Creating a watch only RBAC group

We'll create a ClusterRole that can only read resources with the verbs list,get,watch -

`kubectl create clusterrole cluster-watcher --verb=list,get,watch --resource='*'`

We'll create our binding and our cluster-viewers group at the same time -

`kubectl create clusterrolebinding cluster-watcher --clusterrole=cluster-watcher --group=cluster-watchers`

If we check the auth against a user called uatu in this group, we no longer have full access like before -

`kubectl auth can-i '*' '*' --as-group="cluster-watchers" --as="uatu"`

However if we check, we can observe -

`kubectl auth can-i 'list' '*' --as-group="cluster-watchers" --as="uatu"`

Let's create a KUBECONFIG file to test this for the uatu user -

`./kubeconfig_creator.sh -u uatu -g cluster-watchers`

If we run a get nodes, this will work -

`KUBECONFIG=./uatu-clusterwatchers.config kubectl get nodes`

We can also query pods -

`KUBECONFIG=./uatu-clusterwatchers.config kubectl get pods`

However, if we try to create something as this user it will fail -

`KUBECONFIG=./uatu-clusterwatchers.config kubectl run nginx --image=nginx`

---

###### Creating an RBAC managed user

All of our examples so far have been against wildcard ClusterRoles and Verbs, and against groups. Let's try this out for a standalone user, we'll create a ClusterRole as a pod manager with the verbs list,get,create,delete and the resource as pods -

```bash
kubectl create clusterrole cluster-pod-manager --verb=list,get,create,delete --resource='pods'
```

We'll create a ClusterRoleBinding which we'll use to bind to our ClusterRole and a user called deadpool -

```bash
kubectl create clusterrolebinding cluster-pod-manager --clusterrole=cluster-pod-manager --user=deadpool
```

If we check with auth can -i list * we wont be able to do so as deadpool -

`kubectl auth can-i 'list' '*' --as="deadpool"`

However, if we check against pods, this will be permitted -

`kubectl auth can-i 'list' 'pods' --as="deadpool"`

If we check the ClusterRoleBindings output, we'll see deadpool as a user in the output -

`kubectl get clusterrolebindings -o wide`

Let's create a KUBECONFIG file again, this time we wont use the group, just the user and observe the output, there will be no O entry as the subject line -

`./kubeconfig_creator.sh -u deadpool`

If we try to access pods as this user, it will work -

`KUBECONFIG=./deadpool.config kubectl get pods`

But if we try and access secrets, it will fail -

`KUBECONFIG=./deadpool.config kubectl get secrets`

---

###### Using Roles and RoleBindings

Let's switch to Roles and RoleBindings which are a namespaced resource, first we'll create a namespace called gryffindor -

`kubectl create namespace gryffindor

We'll create a role, in this new namespace with full access -

`kubectl -n gryffindor create role gryffindor-admin --verb='*' --resource='*'

And we'll create a RoleBinding and specify our group -

`kubectl -n gryffindor create rolebinding gryffindor-admin --role=gryffindor-admin --group=gryffindor-admins`

If we check our auth against the group, we'll be rejected if we don't specify a namespace, however, if we do it will work as expected -

```bash
kubectl auth can-i '*' '*' --as-group="gryffindor-admins" --as="harry"
kubectl -n gryffindor auth can-i '*' '*' --as-group="gryffindor-admins" --as="harry"
```

Let's create a kubeconfig file to check this as well, we'll use our convenient tool to do so and we'll set the namespace as we execute -

`./kubeconfig_creator.sh -u harry -g gryffindor-admins -n gryffindor`

If we check our configuration file, it has a namespace set -

`cat harry-gryffindoradmins.config`

If we try and access anything outside of a namespace it will fail -

`KUBECONFIG=./harry-gryffindoradmins.config kubectl get nodes`

If we try and access pods in the default namespace, this will also fail -

`KUBECONFIG=./harry-gryffindoradmins.config kubectl -n default get pods`

However, if we access resources in the gryffindor namespace, it will succeed -

`KUBECONFIG=./harry-gryffindoradmins.config kubectl get pods`

---

###### Cleanup

Execute the following commands, to cleanup -

```bash
cd ..
rm -rf batman* kubeconfig-creator
kubectl delete namespace/gryffindor
kubectl delete clusterrole/cluster-superhero clusterrole/cluster-watcher clusterrole/cluster-pod-manager
kubectl delete clusterrolebinding/cluster-superhero clusterrolebinding/cluster-watcher clusterrolebinding/cluster-pod-manager
```

## ServiceAccounts

This is the lab that complements the Kubernetes RBAC further study section. If you have not seen this, please first see this section in the course as it contains important KCNA exam information. This lab provides an automated step through of this optional further study.

We'll start by creating a simple pod with curlimages/curl -

`kubectl run curl --image=curlimages/curl:8.4.0 -- sleep infinity`

Inspect the pod, you will see Service Account at the top and it will be assigned the value of default

`kubectl describe pod/curl | more`

Kubectl exec into the curlimages/curl container with a sh shell

`kubectl exec -it curl -- sh`

We can capture the default ServiceAccount token that has been assigned to the pod by accessing the token file that is available through /var/run/secrets/kubernetes.io, all pods receive API information in this way, via the pods filesystem -

```bash
TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
echo $TOKEN
```

Using this token, we can query the API using curl, for example we could query the /api/v1/pods endpoint. We will use the convenient DNS name of https://kubernetes.default.svc which is setup by default for convenience to allow pods to communicate with the API. If you attempt this at this stage, access will be restricted as there are no access permissions with the default account -

```bash
curl --header "Authorization: Bearer $TOKEN" --insecure https://kubernetes.default.svc/api/v1/pods
```

Exit the pod, and clean-up

```bash
exit
kubectl delete pod/curl --now
```

Let’s create a pod-serviceaccount -

`kubectl create serviceaccount pod-serviceaccount`

Create a ClusterRole or Role as per our video with the desired resources and verbs. We’ll re-use the example we used for deadpool -

```bash
kubectl create clusterrole cluster-pod-manager --verb=list,get,create,delete --resource='pods'
```

Create a ClusterRoleBinding/RoleBinding, again we’ll re-use the deadpool example but instead of specifying a user, we’ll specify a serviceaccount, the format for this is namespace:serviceaccount

```bash
kubectl create clusterrolebinding cluster-pod-manager --clusterrole=cluster-pod-manager --serviceaccount=default:pod-serviceaccount
```

This time we’ll re-create the Pod but we’ll specify our serviceaccount. Sadly it isn’t possible to specify a serviceaccount as part of the CLI but, we can inject this into the yaml on the fly as follows -

```bash
kubectl run curl --image=curlimages/curl:8.4.0 --overrides='{ "spec": { "serviceAccount": "pod-serviceaccount" } }' -- sleep infinity
```

If we describe the pod, this time we will see pod-serviceaccount as the service account -

`kubectl describe pod/curl | more`

Kubectl exec into the curlimages/curl container with an sh shell -

`kubectl exec -it curl -- sh`

Again, capture the default ServiceAccount token that has been assigned to the pod by accessing the token file that is available through /var/run/secrets/kubernetes.io -

```bash
TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
echo $TOKEN
```

Using this token, attempt to query the /api/v1/pods endpoint as the pod-serviceaccount, this time it will be successful -

```bash
curl --header "Authorization: Bearer $TOKEN" --insecure https://kubernetes.default.svc/api/v1/pods
```

Exit the pod, and clean-up all of the resources -

```bash
exit
kubectl delete pod/curl serviceaccount/pod-serviceaccount clusterrole/cluster-pod-manager clusterrolebinding/cluster-pod-manager --now
```

