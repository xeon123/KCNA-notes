Show the yaml output for an nginx pod, note the default label of `run:nginx` -

`kubectl run nginx --image nginx --port 80 -o yaml --dry-run=client`

And let's run this -

`kubectl run nginx --image nginx --port 80`

Review the yaml for exposing this pod, note that the selector references 'run:nginx' which is how it matches our pod -

`kubectl expose pod/nginx --dry-run=client -o yaml`

We'll also run this -

`kubectl expose pod/nginx`

With these labels set, we could query these using a selector parameter -

`kubectl get all --selector run=nginx`

Let's try out labels ourselves, we will create three pods, each with a specific colour as a label. The following yaml will be used as a template to create these (longer pastes like this may not appear to work as expected but, if you review the file they will be okay) -

```bash
cat <<EOF > coloured_pods.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ubuntu
    colour: red
  name: ubuntu-red
spec:
  containers:
  - command:
    - sleep
    - infinity
    image: ubuntu
    name: ubuntu
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ubuntu
    colour: green
  name: ubuntu-green
spec:
  containers:
  - command:
    - sleep
    - infinity
    image: ubuntu
    name: ubuntu
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ubuntu
    colour: pink
  name: ubuntu-pink
spec:
  containers:
  - command:
    - sleep
    - infinity
    image: ubuntu
    name: ubuntu
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
EOF
```

We'll apply this and create our pods -

`kubectl apply -f coloured_pods.yaml`

If we check we will have our coloured pods in the output -

`kubectl get pods -o wide`

And now if we desire, we can specifically select resources based on that selector -

`kubectl get all --selector colour=green`

We can also use the shorthand method of -l which is a synonym for --selector -

`kubectl get all -l colour=green`

Let's cleanup after ourselves -

```bash
kubectl delete pod/nginx service/nginx pod/ubuntu-red pod/ubuntu-green pod/ubuntu-pink --now
```

And remove our yaml file -

`rm coloured_pods.yaml`