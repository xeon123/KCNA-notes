Create a ConfigMap with two entries using the --from-literal parameter -

```bash
kubectl create configmap colour-configmap --from-literal=COLOUR=red --from-literal=KEY=value
```

Describe the ConfigMap to show the available values -

```bash
kubectl describe configmap/colour-configmap
```

And lets remove this ConfigMap -

`kubectl delete configmap/colour-configmap`

We'll use an alternative approach for creating the same ConfigMap, we'll save our values to a file -

```bash
cat <<EOF > configmap-colour.properties
COLOUR=green
KEY=value
EOF
```

Check our file -

`cat configmap-colour.properties`

And create the ConfigMap using the --from-env-file parameter -

```bash
kubectl create configmap colour-configmap --from-env-file=configmap-colour.properties
```

If we run our describe again, it will be as per the previous attempt -

`kubectl describe configmap/colour-configmap`

Let's capture the yaml for a pod that dumps environment variables, and then sleeps for infinity -

```bash
kubectl run --image=ubuntu --dry-run=client --restart=Never -o yaml ubuntu --command bash -- -c 'env; sleep infinity' | tee env-dump-pod.yaml
```

We'll apply this pod -

`kubectl apply -f env-dump-pod.yaml`

And can see it in our get pods output -

`kubectl get pods`

And if we view the logs for this, we'll see the default environment variables including those added by Kubernetes

`kubectl logs ubuntu`

- Update our pod yaml so that it includes an `envFrom`, `configMapRef`
	- When you use envFrom, Kubernetes will automatically inject the values from the referenced ConfigMap into the container's environment variables, making them available for your application to use.


```yaml
cat <<EOF > env-dump-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ubuntu
  name: ubuntu
spec:
  containers:
  - command:
    - bash
    - -c
    - env; sleep infinity
    image: ubuntu
    name: ubuntu
    resources: {}
    envFrom:
    - configMapRef:
        name: colour-configmap
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
EOF
```

If we attempt to apply this at present it will fail as we can only update certain fields -

`kubectl apply -f env-dump-pod.yaml`

We will need to delete and recreate the instance -

`kubectl delete -f env-dump-pod.yaml --now; kubectl apply -f env-dump-pod.yaml`

If we check the logs again, we will see both of our values as environment variables -

`kubectl logs ubuntu`

ConfigMaps provide a centralised location for configuration data on our cluster, edit the current configmap and change the colour to red -

`kubectl edit configmap/colour-configmap`

Cycle the pod again -

`kubectl delete -f env-dump-pod.yaml --now && kubectl apply -f env-dump-pod.yaml`

And check the logs, the output will now show red -

`kubectl logs ubuntu`

Let's make our ConfigMap immutable. Update the configmap and add `immutable: true` without any indentation in the file on a new line, also change the colour to purple -

`kubectl edit configmap/colour-configmap`

Recycle the pod again -

`kubectl delete -f env-dump-pod.yaml --now && kubectl apply -f env-dump-pod.yaml`

And check the logs to confirm that it shows purple -

`kubectl logs ubuntu`

If you try to change the configmap again, it will fail as it is now in an immutable state, change the colour and try to save it -

`kubectl edit configmap/colour-configmap`

We'll clean up the resources -

`kubectl delete pod/ubuntu configmap/colour-configmap --now`

And remove our files -

`rm configmap-colour.properties env-dump-pod.yaml`