
# Ephemeral Storage

We'll begin by capturing ubuntu pod yaml with sleep infinity as a command -

```bash
kubectl run --image=ubuntu ubuntu -o yaml --dry-run=client --command sleep infinity | tee ubuntu_emptydir.yaml
```

Update this specification so that it includes a volumeMount and an emptyDir volume -

```yaml
cat <<EOF > ubuntu_emptydir.yaml
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
    - sleep
    - infinity
    image: ubuntu
    name: ubuntu
    resources: {}
    volumeMounts:
    - mountPath: /cache
      name: cache-volume
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  volumes:
  - name: cache-volume
    emptyDir: {
      medium: Memory,
    }
status: {}
EOF
```
And let's apply this file -

`kubectl apply -f ubuntu_emptydir.yaml`

And lets check our pod is running -

```bash
kubectl get pods -o wide
NAME     READY   STATUS    RESTARTS   AGE   IP          NODE       NOMINATED NODE   READINESS GATES
ubuntu   1/1     Running   0          15s   10.42.1.2   worker-1   <none>           <none>
```

We'll hop into this pod with an interactive shell -

`kubectl exec -it ubuntu -- bash`

And if we go into cache and do a `df -h`, we can see that the filesytem being used is tmpfs -

```bash
cd cache; df -h .
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           7.6G     0  7.6G   0% /cache
```

With this being a memory based filesystem, it will be highly performant. Let's test this using dd -

```bash
dd if=/dev/zero of=output oflag=sync bs=1024k count=1000
1000+0 records in
1000+0 records out
1048576000 bytes (1.0 GB, 1000 MiB) copied, 0.611228 s, 1.7 GB/s
```

We can see that 1GB of data with blockes of 1024k where written in 0.6 seconds, with a speed of 1.7GB/s.

For now, let's exit the pod -

`exit`

When we get rid of this container it will also remove the emptyDir volume -

`kubectl delete pod/ubuntu --now`

---

# Persistent Storage

https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/

Let's take a look at the storageclasses available in our cluster, we will have a single storageclass and it will be marked as default -

```bash
kubectl get storageclass
NAME                   PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
local-path (default)   rancher.io/local-path   Delete          WaitForFirstConsumer   false                  18m
```

Familiarize yourself with the `storageclass` specification, in particular the `reclaimPolicy` section -

`kubectl explain storageclass | more`

We'll create a persistent volume using the k3s storageClass of local-path. The path is where the storage will be stored.

```yaml
cat <<EOF > manual_pv.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: manual-pv001
spec:
  storageClassName: local-path
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/var/lib/rancher/k3s/storage/manual-pv001"
    type: DirectoryOrCreate
EOF
```

And we will apply this file -

`kubectl apply -f manual_pv.yaml`

Check the available persistent volumes and note that the reclaim policy is set to `Retain` 

```bash
kubectl get pv
NAME           CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
manual-pv001   10Gi       RWO            Retain           Available           local-path     <unset>                          6s
```

Let's now create a manual persistent volume claim against this persistent volume 

```yaml
cat <<EOF > manual_pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: manual-claim
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 10Gi
  storageClassName: local-path
  volumeName: manual-pv001
EOF
```

`volumeName` is the selected volume when the PVC will be bound.

Apply this file -

`kubectl apply -f manual_pvc.yaml`

And check the persistent volume claim -

```bash
kubectl get persistentvolumeclaim
NAME           STATUS   VOLUME         CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
manual-claim   Bound    manual-pv001   10Gi       RWO            local-path     <unset>                 2s
```

This can also be done the short way -

`kubectl get pvc`

---

# Dynamic PVC

Let's create a dynamic version of our Persistent Volume Claim, this will be similar to our manual approach but without a volumeName entry. The dynamic approach is just creating a PVC.

```yaml
cat <<EOF > dynamic_pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: dynamic-claim
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 10Gi
  storageClassName: local-path
EOF
```

Let's apply this -

`kubectl apply -f dynamic_pvc.yaml`

And if we get our pvc's you'll see that this is Pending -

```bash
kubectl get pvc
NAME            STATUS    VOLUME         CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
dynamic-claim   Pending                                            local-path     <unset>                 4s
manual-claim    Bound     manual-pv001   10Gi       RWO            local-path     <unset>                 3m29s
```

If we run a describe on the pvc, we will see that it is awaiting a consumer before it is bound -

```bash
kubectl describe pvc/dynamic-claim
Name:          dynamic-claim
Namespace:     default
StorageClass:  local-path
Status:        Pending
Volume:        
Labels:        <none>
Annotations:   <none>
Finalizers:    [kubernetes.io/pvc-protection]
Capacity:      
Access Modes:  
VolumeMode:    Filesystem
Used By:       <none>
Events:
  Type    Reason                Age               From                         Message
  ----    ------                ----              ----                         -------
  Normal  WaitForFirstConsumer  4s (x4 over 44s)  persistentvolume-controller  waiting for first consumer to be created before binding
```

And if we check our pv's, we only have the manual pv -

```bash
kubectl get pv
NAME           CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                  STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
manual-pv001   10Gi       RWO            Retain           Bound    default/manual-claim   local-path     <unset>                          11m
```

Let's mock up a ubuntu pod with sleep infinity as a starting base -

```bash
kubectl run --image=ubuntu ubuntu -o yaml --dry-run=client --command sleep infinity | tee ubuntu_with_volumes.yaml
```

And we'll modify this to include volumeMounts and volumes for both our manual and dynamic pvc's -

```bash
cat <<EOF > ubuntu_with_volumes.yaml
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
    - sleep
    - infinity
    image: ubuntu
    name: ubuntu
    resources: {}
    volumeMounts:
    - mountPath: /manual
      name: manual-volume
    - mountPath: /dynamic
      name: dynamic-volume
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  volumes:
    - name: manual-volume
      persistentVolumeClaim:
        claimName: manual-claim
    - name: dynamic-volume
      persistentVolumeClaim:
        claimName: dynamic-claim
status: {}
EOF
```

As the k3s `storageclass` provisioner is very basic, we should use a `nodeSelector` to guide this pod to a specific node, check the labels for `node/worker-1` -

```bash
kubectl describe node/worker-1 | more
Name:               worker-1
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/instance-type=k3s
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=worker-1
                    kubernetes.io/os=linux
                    node.kubernetes.io/instance-type=k3s
Annotations:        alpha.kubernetes.io/provided-node-ip: 172.18.0.2
                    flannel.alpha.coreos.com/backend-data: {"VNI":1,"VtepMAC":"8e:91:e9:4d:68:88"}
                    flannel.alpha.coreos.com/backend-type: vxlan
                    flannel.alpha.coreos.com/kube-subnet-manager: true
                    flannel.alpha.coreos.com/public-ip: 172.18.0.2
                    k3s.io/hostname: worker-1
                    k3s.io/internal-ip: 172.18.0.2
                    k3s.io/node-args: ["agent","--kubelet-arg","eviction-hard=imagefs.available\u003c1%,nodefs.available\u003c1%"]
                    k3s.io/node-config-hash: Y2YJXFJB3HS5ITQ7HI4YLWI2AZTXKBI3XMRYHIOJJLD6BJZRB6PQ====
                    k3s.io/node-env: {"K3S_TOKEN":"********","K3S_URL":"https://control-plane:6443"}
                    node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Sun, 16 Feb 2025 03:24:03 +0000
Taints:             <none>
Unschedulable:      false
Lease:
  HolderIdentity:  worker-1
  AcquireTime:     <unset>
  RenewTime:       Sun, 16 Feb 2025 04:11:01 +0000
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  MemoryPressure   False   Sun, 16 Feb 2025 04:07:26 +0000   Sun, 16 Feb 2025 03:24:03 +0000   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Sun, 16 Feb 2025 04:07:26 +0000   Sun, 16 Feb 2025 03:24:03 +0000   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Sun, 16 Feb 2025 04:07:26 +0000   Sun, 16 Feb 2025 03:24:03 +0000   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Sun, 16 Feb 2025 04:07:26 +0000   Sun, 16 Feb 2025 03:24:03 +0000   KubeletReady                 kubelet is posting ready status
Addresses:
  InternalIP:  172.18.0.2
  Hostname:    worker-1
Capacity:
  cpu:                24
  ephemeral-storage:  1055761844Ki
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             7895220Ki
  pods:               110
Allocatable:
  cpu:                24
  ephemeral-storage:  1070289127216
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             7895220Ki
  pods:               110
System Info:
  Machine ID:                 2eeb9f639e8a1f21944edaa76478c190
  System UUID:                2eeb9f639e8a1f21944edaa76478c190
  Boot ID:                    f5bf0f33-0ab5-4d1f-8a71-1adbb7286ca4
  Kernel Version:             6.12.5-linuxkit
  OS Image:                   Ubuntu 22.04.2 LTS
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  containerd://1.7.20-k3s1
  Kubelet Version:            v1.31.0+k3s1
  Kube-Proxy Version:         
PodCIDR:                      10.42.1.0/24
PodCIDRs:                     10.42.1.0/24
ProviderID:                   k3s://worker-1
Non-terminated Pods:          (0 in total)
  Namespace                   Name    CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----    ------------  ----------  ---------------  -------------  ---
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests  Limits
  --------           --------  ------
  cpu                0 (0%)    0 (0%)
  memory             0 (0%)    0 (0%)
  ephemeral-storage  0 (0%)    0 (0%)
  hugepages-1Gi      0 (0%)    0 (0%)
  hugepages-2Mi      0 (0%)    0 (0%)
Events:
  Type     Reason                   Age                From                   Message
  ----     ------                   ----               ----                   -------
  Normal   Starting                 46m                kube-proxy             
  Normal   CIDRAssignmentFailed     46m                cidrAllocator          Node worker-1 status is now: CIDRAssignmentFailed
  Normal   Synced                   46m                cloud-node-controller  Node synced successfully
  Normal   Starting                 46m                kubelet                Starting kubelet.
  Warning  InvalidDiskCapacity      46m                kubelet                invalid capacity 0 on image filesystem
  Normal   NodeHasSufficientMemory  46m (x2 over 46m)  kubelet                Node worker-1 status is now: NodeHasSufficientMemory
  Normal   NodeHasNoDiskPressure    46m (x2 over 46m)  kubelet                Node worker-1 status is now: NodeHasNoDiskPressure
  Normal   NodeHasSufficientPID     46m (x2 over 46m)  kubelet                Node worker-1 status is now: NodeHasSufficientPID
  Normal   NodeAllocatableEnforced  46m                kubelet                Updated Node Allocatable limit across pods
  Normal   NodeReady                46m                kubelet                Node worker-1 status is now: NodeReady
  Normal   RegisteredNode           46m                node-controller        Node worker-1 event: Registered Node worker-1 in Controller
```

And update the yaml to include a `nodeSelector` field. We add the `nodeSelector` because the current persistent storage is very simple. If we had used Rook, this wouldn´t be needed.

> **Note:** Rook is a complete k8s distributed storage. Rook turns distributed storage systems into self-managing, self-scaling, self-healing storage services. It automates the tasks of a storage administrator: deployment, bootstrapping, configuration, provisioning, scaling, upgrading, migration, disaster recovery, monitoring, and resource management. This solution offers a node traversal, which is a pod can be scheduled in any node, that it can access a shared storage.


```bash
cat <<EOF > ubuntu_with_volumes.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ubuntu
  name: ubuntu
spec:
  nodeSelector:
    kubernetes.io/hostname: worker-1
  containers:
  - command:
    - sleep
    - infinity
    image: ubuntu
    name: ubuntu
    resources: {}
    volumeMounts:
    - mountPath: /manual
      name: manual-volume
    - mountPath: /dynamic
      name: dynamic-volume
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  volumes:
    - name: manual-volume
      persistentVolumeClaim:
        claimName: manual-claim
    - name: dynamic-volume
      persistentVolumeClaim:
        claimName: dynamic-claim
status: {}
EOF
```

And we can apply this file -

`kubectl apply -f ubuntu_with_volumes.yaml`

Check that the pod is running -

```bash
kubectl get pod
NAME     READY   STATUS    RESTARTS   AGE
ubuntu   1/1     Running   0          55s
```

Check pv's, A dynamic pv will have been created, notice the reclaim policies for both -

```bash
kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                   STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
manual-pv001                               10Gi       RWO            Retain           Bound    default/manual-claim    local-path     <unset>                          15m
pvc-bef0212e-c813-4341-9b2e-d31c05c16f93   10Gi       RWO            Delete           Bound    default/dynamic-claim   local-path     <unset>                          1s
```

Check pvc's, they should now both be bound -

```bash
kubectl get pvc
NAME            STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   VOLUMEATTRIBUTESCLASS   AGE
dynamic-claim   Bound    pvc-bef0212e-c813-4341-9b2e-d31c05c16f93   10Gi       RWO            local-path     <unset>                 6m41s
manual-claim    Bound    manual-pv001                               10Gi       RWO            local-path     <unset>                 9m41s
```

And let's write data to our volumes, access the pod with an interactive shell -

`kubectl exec -it ubuntu -- bash`

And if we check both of these, they are mounted volumes -

```bash
cd /manual; df -h .; cd /dynamic; df -h .
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1      1007G  3.4G  953G   1% /manual
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1      1007G  3.4G  953G   1% /dynamic
```

Let's write a text file to each of these and we'll exit -

```bash
echo hello > /manual/test.txt; echo hello > /dynamic/test.txt; exit
```

As these are persistent volumes, if we delete the pod and recreate it -

```bash
kubectl delete pod/ubuntu --now && kubectl apply -f ubuntu_with_volumes.yaml
```

And again access this pod -

`kubectl exec -it ubuntu -- bash`

We can see that our files will still exist as our volumes are persistent -

```bash
cat /manual/test.txt; cat /dynamic/test.txt
hello
hello
```

Let's exit -

`exit`

Before we go, let's see what happens when we delete the pod and pvc's -

```bash
kubectl delete pod/ubuntu pvc/dynamic-claim pvc/manual-claim --now
pod "ubuntu" deleted
persistentvolumeclaim "dynamic-claim" deleted
persistentvolumeclaim "manual-claim" deleted
```

If we check our pv's, the manual one still exists as the Reclaim policy is set to Retain. The dynamic is removed.

```bash
kubectl get pv
NAME           CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS     CLAIM                  STORAGECLASS   VOLUMEATTRIBUTESCLASS   REASON   AGE
manual-pv001   10Gi       RWO            Retain           Released   default/manual-claim   local-path     <unset>                          20m
```

Finally we'll clean this up and remove our files -

```bash
kubectl delete pv/manual-pv001 --now; rm -rf dynamic_pvc.yaml manual_pv* ubuntu_emptydir.yaml ubuntu_with_volumes.yaml
```