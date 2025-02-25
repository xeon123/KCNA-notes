There isn't a quick way of creating a StatefulSet from the CLI but a Deployment gets us very close, we'll mock up a Deployment and tee to a file -

`kubectl create deployment nginx --image=nginx --replicas=3 --dry-run=client -o yaml | tee statefulset.yaml`

And we'll modify this, to change it to a StatefulSet, we're also going to include a reference to `serviceName`.

`serviceName` is optional, but this allows statefulsets to have stable network ids.

```yaml
cat <<EOF > statefulset.yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  serviceName: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
EOF
```

And let's apply this file -

`kubectl apply -f statefulset.yaml`

And lets check our statefulset -

```bash
kubectl get statefulset -o wide
NAME    READY   AGE   CONTAINERS   IMAGES
nginx   0/3     12s   nginx        nginx
```

If we check our pods, these will have been named accordingly -

```bash
kubectl get pods -o wide
NAME      READY   STATUS    RESTARTS   AGE   IP          NODE            NOMINATED NODE   READINESS GATES
nginx-0   1/1     Running   0          44m   10.42.1.2   worker-2        <none>           <none>
nginx-1   1/1     Running   0          44m   10.42.3.2   worker-1        <none>           <none>
nginx-2   1/1     Running   0          43m   10.42.0.5   control-plane   <none>           <none>
```

**Bonus Lab Content:** If a pod in a statefulset was removed, causing a restart, whilst the ip may change, the network name of the pod  will stay consistent.

Delete pod/nginx-2 -

`kubectl delete pod/nginx-2 --now`

And now query our pods, and see that the pod has restarted. -

```bash
kubectl get pods -o wide
NAME      READY   STATUS    RESTARTS   AGE   IP          NODE            NOMINATED NODE   READINESS GATES
nginx-0   1/1     Running   0          47m   10.42.1.2   worker-2        <none>           <none>
nginx-1   1/1     Running   0          46m   10.42.3.2   worker-1        <none>           <none>
nginx-2   1/1     Running   0          8s    10.42.0.6   control-plane   <none>           <none>
```

## Headless Service

Let's make use of the statefulsets service functionality, we'll create a headless service called nginx -

`kubectl create service clusterip --clusterip=None nginx`

Check the service is visible -

```bash
kubectl get service
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   55m
nginx        ClusterIP   None         <none>        <none>    2s
```

And if we take a look at, we can see our endpoint slices -

```bash
kubectl get endpoints
NAME         ENDPOINTS                       AGE
kubernetes   172.18.0.2:6443                 56m
nginx        10.42.0.6,10.42.1.2,10.42.3.2   30s
```

The yaml output is great for viewing endpoints -

```bash
kubectl get endpoints/nginx -o yaml
apiVersion: v1
kind: Endpoints
metadata:
  annotations:
    endpoints.kubernetes.io/last-change-trigger-time: "2025-02-16T20:11:11Z"
  creationTimestamp: "2025-02-16T20:11:11Z"
  labels:
    app: nginx
    service.kubernetes.io/headless: ""
  name: nginx
  namespace: default
  resourceVersion: "2250"
  uid: 4b8d6b5b-bd86-48ec-ba58-eaaf489003db
subsets:
- addresses:
  - hostname: nginx-2
    ip: 10.42.0.6
    nodeName: control-plane
    targetRef:
      kind: Pod
      name: nginx-2
      namespace: default
      uid: a5a4819e-0ba3-4b7b-ac9f-bd390cbc84e2
  - hostname: nginx-0
    ip: 10.42.1.2
    nodeName: worker-2
    targetRef:
      kind: Pod
      name: nginx-0
      namespace: default
      uid: f351ebf8-4f53-4711-b5c6-d1678b7643cd
  - hostname: nginx-1
    ip: 10.42.3.2
    nodeName: worker-1
    targetRef:
      kind: Pod
      name: nginx-1
      namespace: default
      uid: d70e92dc-220d-4bd5-bf19-0a4a928dfd58
```

The stable network ID takes the form of:
* `<hostname>.<servicename>.<namespace>.svc.cluster.local`

Let's check this from the viewpoint of a pod, we'll run a curlimages/curl container -

```bash
kubectl run --rm -i --tty curl --image=curlimages/curl:8.4.0 --restart=Never -- sh
```

And let's curl our statefulset service name -

```bash
curl nginx-1.nginx.default.svc.cluster.local
```

And we'll exit this container -

`exit`

## Strategy

Take a look at the statefulset yaml, you'll notice that the updateStrategy/rollingUpdate has a partition value of 0 -

`kubectl get statefulset/nginx -o yaml | more`

We can seeth that the partition is set to 0.

```yaml
  updateStrategy:
    rollingUpdate:
      partition: 0
    type: RollingUpdate
```


If we set the partition value to 2, it will consider the rolling update from the third pod.

The counter starts at 0 when naming pods. A partition of 2 refers to nginx-1 onwards. Onwards means any pod after this.

```bash
kubectl patch statefulset/nginx -p '{"spec":{"updateStrategy":{"rollingUpdate":{"partition":2}}}}'
```

We will set the image to `nginx:alpine` and watch the rollout -

```bash
kubectl set image statefulset/nginx nginx=nginx:alpine && kubectl rollout status statefulset/nginx

statefulset.apps/nginx image updated
Waiting for partitioned roll out to finish: 0 out of 1 new pods have been updated...
Waiting for 1 pods to be ready...
```

Because of the partition value, the rollout only took place on nginx-2, we can verify this with the following commands (check the image) -

```bash
kubectl describe pod/nginx-1 | more

Containers:
  nginx:
    Container ID:   containerd://8ba55948c5a0db0a47860aef6bf779463d1fa7319150f5a33adfd33d85a69f7c
    Image:          nginx
    Image ID:       docker.io/library/nginx@sha256:91734281c0ebfc6f1aea979cffeed5079cfe786228a71cc6f1f46a228cde6e34
```

```bash
kubectl describe pod/nginx-2 | more

Containers:
  nginx:
    Container ID:   containerd://0b39bf466d4b5621c4ca0ab1925ae619a6315952839961d3d45aa64606951596
    Image:          nginx:alpine
    Image ID:       docker.io/library/nginx@sha256:4ff102c5d78d254a6f0da062b3cf39eaf07f01eec0927fd21e219d0af8bc0591
```

---

## StatefulSet with Persistent Storage

Update our statefulset to include a dynamic volume -

```yaml
cat <<EOF > statefulset.yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  labels:
    app: nginx
  name: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  serviceName: nginx
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        volumeMounts:
        - name: nginx
          mountPath: /data
  volumeClaimTemplates:
  - metadata:
      name: nginx
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "local-path"
      resources:
        requests:
          storage: 1Gi
EOF
```

And we will recycle our statefulset -

`kubectl delete -f statefulset.yaml && kubectl apply -f statefulset.yaml

Watch our pods start, press ctrl-c to exit -

`watch --differences kubectl get pods -o wide

And we can verify that we have an individual `pvc` and `pv` for each pod -

```bash
kubectl get pvc
NAME            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
nginx-nginx-0   Bound    pvc-177766f7-5837-403f-8526-1d940b6ec162   1Gi        RWO            local-path     <unset>                 39s
nginx-nginx-1   Bound    pvc-6d84208d-1f5b-470b-b811-fc5acda0c2f5   1Gi        RWO            local-path     <unset>                 25s
nginx-nginx-2   Bound    pvc-cd472363-e6af-4a2c-8e59-46445f182e88   1Gi        RWO            local-path     <unset>                 12s
```

```bash
kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                   STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
pvc-177766f7-5837-403f-8526-1d940b6ec162   1Gi        RWO            Delete           Bound    default/nginx-nginx-0   local-path     <unset>                          49s
pvc-6d84208d-1f5b-470b-b811-fc5acda0c2f5   1Gi        RWO            Delete           Bound    default/nginx-nginx-1   local-path     <unset>                          35s
pvc-cd472363-e6af-4a2c-8e59-46445f182e88   1Gi        RWO            Delete           Bound    default/nginx-nginx-2   local-path     <unset>                          22s
```

We'll execute a shell into nginx-0 -

`kubectl exec -it nginx-0 -- bash`

Create some data -

`cd /data; touch this_is_nginx-0; ls -l`

And exit -

`exit

Let's delete the pod and watch it be recreated -

```bash
kubectl delete pod/nginx-0 --now; watch --differences kubectl get pods -o wide
```

And if we execute a shell again -

`kubectl exec -it nginx-0 -- bash`

We can verify that our data still exists -

```bash
ls /data
```

Let's exit -

`exit`

And we can take this further, we can delete the entire statefulset -

```bash
kubectl delete -f statefulset.yaml
```

Our pvc and pv will still exist -

```bash
kubectl get pvc
AME            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
nginx-nginx-0   Bound    pvc-177766f7-5837-403f-8526-1d940b6ec162   1Gi        RWO            local-path     <unset>                 3m12s
nginx-nginx-1   Bound    pvc-6d84208d-1f5b-470b-b811-fc5acda0c2f5   1Gi        RWO            local-path     <unset>                 2m58s
nginx-nginx-2   Bound    pvc-cd472363-e6af-4a2c-8e59-46445f182e88   1Gi        RWO            local-path     <unset>                 2m45s
```

```bash
kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                   STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
pvc-177766f7-5837-403f-8526-1d940b6ec162   1Gi        RWO            Delete           Bound    default/nginx-nginx-0   local-path     <unset>                          3m2s
pvc-6d84208d-1f5b-470b-b811-fc5acda0c2f5   1Gi        RWO            Delete           Bound    default/nginx-nginx-1   local-path     <unset>                          2m48s
pvc-cd472363-e6af-4a2c-8e59-46445f182e88   1Gi        RWO            Delete           Bound    default/nginx-nginx-2   local-path     <unset>                          2m35s

```

And if we apply the statefulset again -

`kubectl apply -f statefulset.yaml`

Check our pods are running -

`kubectl get pods -o wide`

And exec back into the pod -

`kubectl exec -it nginx-0 -- bash`

And check our data, we can confirm that the file is available as expected -

`ls /data`

Let's exit -

`exit`

And we'll cleanup -

```bash
kubectl delete statefulset/nginx --now; for i in 0 1 2; do kubectl delete pvc/nginx-nginx-$i --now; done; kubectl delete service/nginx; rm statefulset.yaml
```