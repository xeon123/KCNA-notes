
# My-scheduler

Let's begin by templating an nginx pod that we'll later target with a custom scheduler -

`kubectl run nginx --image=nginx -o yaml --dry-run=client | tee nginx_scheduler.yaml`

We are interested in a pod.spec option known as schedulerName, use kubectl explain to see more information on this -

`kubectl explain pod.spec | more`

Update the nginx_scheduler.yaml file to include a schedulerName for a scheduler called my-scheduler -

```yaml
cat <<EOF > nginx_scheduler.yaml
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
EOF
```

And let's apply this file -

`kubectl apply -f nginx_scheduler.yaml`

Our pod will be stuck in pending as there is no scheduler called my-scheduler running -

`kubectl get pods -o wide`

We're going to make use of a convenient code example that mimicks the functionality of a scheduler as a bash script, firstly install git and jq if required -

`apt update && apt install -y git jq`

And lets clone this project -

`git clone https://github.com/spurin/simple-kubernetes-scheduler-example.git`

And lets change to the directory and take a look at this file -

`cd simple-kubernetes-scheduler-example; more my-scheduler.sh`

And if you're interested in seeing how the json traversal in the script works, take a look at the json as follows -

`kubectl get nodes -o json`

If we run this scheduler, it should schedule our pending pod, press ctrl-c when done -

`./my-scheduler.sh`

And if we check our pods, this will now be scheduled -

`kubectl get pods -o wide`

For now, let's clean this up -

`kubectl delete pod/nginx --now`

---

# Scheduling using nodeName

An alternative way of scheduling is by directly specifying the nodeName in which to schedule the pod, firstly go back to the previous directory -

`cd ..`

Review the options for nodeName under pod.spec -

`kubectl explain pod.spec | more`

Update the yaml so that it directly specifies a nodeName of worker-2 -

```yaml
cat <<EOF > nginx_scheduler.yaml
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
EOF
```

And we will apply this file -

`kubectl apply -f nginx_scheduler.yaml`

Checking our pods again will show this scheduled to worker-2 -

`kubectl get pods -o wide`

For now, remove this pod -

`kubectl delete pod/nginx --now`

---

# Scheduling using nodeSelector (Further Study)

An alternative approach is the use of nodeSelector which makes use of known labels, to select a node. If you run a describe on the worker-1 node you'll see the labels section at the top -

`kubectl describe node/worker-1 | more

We will make use of the label kubernetes.io/hostname=worker-1 to target a node, update the yaml as follows -

```yaml
cat <<EOF > nginx_scheduler.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
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
EOF
```

Apply this update -

`kubectl apply -f nginx_scheduler.yaml`

And if we check, this will have been scheduled to worker-1 -

`kubectl get pods -o wide`

---

# Cleaning up

Finally, we'll clean this up!

```bash
kubectl delete pod/nginx --now; rm -rf simple-kubernetes-scheduler-example; rm -rf nginx_scheduler.yaml
```