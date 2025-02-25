
The following example is a pod manifest that is preconfigured with a simple init container. It will quite simply echo a countdown from 120 to 0, after which the init container will complete and the main container will run. The main container will echo main container every 30 seconds with a counter iteration, until it is stopped. Create a yaml manifest called countdown-pod.yaml -

```bash
cat <<EOF > countdown-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: countdown-pod
spec:
  initContainers:
  - name: init-countdown
    image: busybox
    command: ['sh', '-c', 'for i in \$(seq 120 -1 0); do echo init-countdown: \$i; sleep 1; done']

  containers:
  - name: main-container
    image: busybox
    command: ['sh', '-c', 'while true; do count=\$((count + 1)); echo main-container: sleeping for 30 seconds - iteration \$count; sleep 30; done']
EOF
```

Apply this resource -

`kubectl apply -f countdown-pod.yaml`

If you check the pod at this stage you'll notice it has an init container reference of 0/1 (0 out of 1 init stages complete) -

`kubectl get pods -o wide`

Lets watch the logs from each container. We’ll watch the init container logs first and when the logs follow completes, we’ll start following the logs for the main container. We will use an until as the commands may require multiple attempts to start whilst the containers initialise -

```bash
until kubectl logs pod/countdown-pod -c init-countdown --follow --pod-running-timeout=5m; do sleep 1; done; until kubectl logs pod/countdown-pod -c main-container --follow --pod-running-timeout=5m; do sleep 1; done
```

Once you’ve reached the main container, press ctrl-c when you’re ready. Check the pod status for comparison, you’ll notice that the init section is now removed and the status will be Running -

`kubectl get pods -o wide`

We’ll remove our pod and clean-up -

```bash
kubectl delete -f countdown-pod.yaml --now
rm countdown-pod.yaml
```