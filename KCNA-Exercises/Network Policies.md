Welcome to the interactive lab on Network Policies in Kubernetes. We'll explore how Network Policies can help isolate pods and control their communication within a Kubernetes cluster. This is particularly useful in multi-tenancy environments or when you need to restrict access to certain services within your cluster -

First, let's create an nginx pod -

`kubectl run nginx --image=nginx`

Next, expose the nginx pod as a service with a ClusterIP. This step makes the nginx pod accessible within the Kubernetes cluster. Use the following command to expose the nginx pod -

```bash
kubectl expose pod/nginx --port=80
```


Now, let's test the accessibility of the nginx service. We'll run a temporary curl pod and use it to send a request to the nginx service. This demonstrates that, by default, any pod in the cluster can access the nginx service. Execute the following command to start an interactive curl session -

```bash
kubectl run --rm -i --tty curl --image=curlimages/curl:8.4.0 --restart=Never -- sh
```

Once in the curl pod, use curl to request the nginx page. Notice that there are no restrictions, and the nginx service responds -

```bash
curl nginx.default.svc.cluster.local
```

After testing, type 'exit' to leave the curl pod -

`exit`

Now, we will implement a Network Policy to restrict access to the nginx pod. Network Policies in Kubernetes allow you to control the flow of traffic. We will create a policy that allows access to the nginx pod only from pods with specific labels. Start by creating a network policy configuration file named 'networkpolicy.yaml' -

```yaml
cat <<EOF > networkpolicy.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-nginx-access
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: nginx
  policyTypes:
    - Ingress
  ingress:
    - from:
      - podSelector:
          matchLabels:
            run: curl
EOF
```

Apply the network policy to your cluster. This will enforce the rules defined in the policy, restricting the access to the `nginx` pod as specified. Run the following command to apply the network policy -

```bash
kubectl apply -f networkpolicy.yaml
```

```bash
kubectl get netpol
NAME                 POD-SELECTOR   AGE
allow-nginx-access   run=nginx      19s
```

Test the network policy. Start another curl pod which we'll be using to accessing the nginx service again. Since this pod has the label `run:curl`, it should be allowed by the network policy. Run the following command to start an interactive session in a new curl pod -

```bash
kubectl run --rm -i --tty curl --image=curlimages/curl --restart=Never -- sh
```

Bring up a new control-plane tab and then do a describe on the pod, you'll see the `run=curl` label, when done close the tab -

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

In the curl pod, use curl to test access. The request should succeed, demonstrating the network policy's effect. -

`curl nginx.default.svc.cluster.local`

Then exit the pod -

`exit`

To further validate the network policy, let's test with a pod that doesn't match the network policy criteria. Create a new curl pod with a different name, which results in a different label. This pod should not be able to access the nginx service. Run the following command to start an interactive session in the new pod -

```bash
kubectl run --rm -i --tty curl2 --image=curlimages/curl --restart=Never -- sh
curl: (7) Failed to connect to nginx.default.svc.cluster.local port 80 after 4 ms: Could not connect to server
```

Inside the curl2 pod, again, attempt to access the nginx service using curl. The request should be blocked by the network policy, illustrating how Network Policies can be used to control access in a Kubernetes cluster -

`curl nginx.default.svc.cluster.local`

And then exit the pod -

`exit`

Finally, let's clean up the environment. Execute the following commands to delete the nginx pod, the nginx service, the network policy, and the network policy configuration file -

```bash
kubectl delete pod nginx; kubectl delete service nginx; kubectl delete networkpolicy allow-nginx-access; rm -rf networkpolicy.yaml
```