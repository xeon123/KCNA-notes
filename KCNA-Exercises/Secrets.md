
Create a secret using the --from-literal approach which is identical in usage to that of ConfigMaps, send the output to yaml as a dry-run and note the encoded values for COLOUR and KEY -

```bash
kubectl create secret generic colour-secret --from-literal=COLOUR=red --from-literal=KEY=value --dry-run=client -o yaml
```

We can verify that this is a base64 encoded value by passing the same value and encoding it ourselves with base64 manually -

`echo -n value | base64`

And we can also decode this by reversing the base64 encoding -

`echo dmFsdWU= | base64 -d`

Let's run our previous example and create the secret -

```bash
kubectl create secret generic colour-secret --from-literal=COLOUR=red --from-literal=KEY=value
```

List available secrets -

`kubectl get secrets`

And show the yaml output for the secret, we will see the encoded secrets within -

`kubectl get secret/colour-secret -o yaml`

We'll re-use our ubuntu pod that sleeps for infinity and dumps environment variables, this time we will use envFrom with secretRef, the following is a complete yaml example -

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
    - secretRef:
        name: colour-secret
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
EOF
```

We'll apply this and create our ubuntu pod -

`kubectl apply -f env-dump-pod.yaml`

And if we view the logs for this, we can see our secrets listed accordingly in a similar way to the use of ConfigMaps -

`kubectl logs ubuntu`

Let's delete these resources -

`kubectl delete pod/ubuntu secret/colour-secret --now`

And remove our file -

`rm env-dump-pod.yaml`