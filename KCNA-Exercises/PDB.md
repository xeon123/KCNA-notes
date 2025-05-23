# Cordon and uncordon nodes

Let's create a simple deployment with multiple replicas to see how they distribute across different nodes -

```bash
kubectl create deployment nginx --image=nginx --replicas=5
```

With the deployment created, check how the replicas are spread across the control-plane and worker nodes -

`kubectl get pods -o wide`

Let's simulate a voluntary disruption. Use the kubectl cordon command to make a node unschedulable, and then delete the pods on that node. This will demonstrate how Kubernetes handles pod rescheduling when a node becomes unavailable. We use a different approach here than in the video to account for randomised pod names.

* cordon => make the node unscheduled
* uncordon => make the node scheduled

```bash
kubectl cordon control-plane && kubectl delete pods -l app=nginx --field-selector=spec.nodeName=control-plane --now
```

Recheck the pods to observe how they are rescheduled -

```bash
kubectl get pods -o wide
```

Now, repeat the cordon and delete process for the pods on worker-1 -

```bash
kubectl cordon worker-1 && kubectl delete pods -l app=nginx --field-selector=spec.nodeName=worker-1 --now
```

Recheck the pods again to observe how they are rescheduled, they should all be running on worker-2 -

```bash
kubectl get pods -o wide
```

Familiarise yourself with the `kubectl drain` command -

```bash
kubectl drain --help | more
```

Introduce a more disruptive scenario by draining worker-2 using the kubectl drain command. This simulates a scenario like node maintenance, where the node is taken offline, and observe the impact on pod availability -

```bash
kubectl drain worker-2 --delete-emptydir-data=true --ignore-daemonsets=true
```

Check the deployment and pod status to observe how the disruption has affected the application's availability. This will highlight the limitations of relying solely on replicas for high availability -

```bash
kubectl get deployment; echo; kubectl get pods -o wide
```

Uncordon all the nodes to make them available for scheduling again. This step is important to bring the cluster back to a normal state before introducing Pod Disruption Budgets -

```bash
kubectl uncordon control-plane worker-1 worker-2
```

Recheck the pods again -

```bash
kubectl get pods -o wide
```


# PDB

Now, explore the creation of a Pod Disruption Budget. First, describe the existing deployment to understand its configuration and use that information to set up the PDB -

```bash
kubectl describe deployment/nginx | more
```

Create a Pod Disruption Budget for the nginx deployment, ensuring that a minimum of 2 pods are always available -

```bash
kubectl create pdb nginx --selector=app=nginx --min-available=2
```

Test the PDB by cordoning all nodes and attempting to drain them. This will demonstrate how the PDB prevents excessive disruptions that could compromise application availability. This loop will continue, once run.

```bash
kubectl cordon control-plane worker-1 worker-2; kubectl drain control-plane worker-1 worker-2 --delete-emptydir-data=true --ignore-daemonsets=true
node/control-plane cordoned
node/worker-1 cordoned
node/worker-2 cordoned
node/control-plane already cordoned
node/worker-1 already cordoned
node/worker-2 already cordoned
evicting pod kube-system/metrics-server-5dc58b587c-cmg25
evicting pod default/nginx-676b6c5bbc-klzth
evicting pod default/nginx-676b6c5bbc-8klhv
evicting pod default/nginx-676b6c5bbc-dg6pv
evicting pod kube-system/coredns-5dd589bf46-fz4z9
evicting pod default/nginx-676b6c5bbc-579th
evicting pod default/nginx-676b6c5bbc-sx6pd
evicting pod kube-system/local-path-provisioner-846b9dcb6c-xzh9n
pod/nginx-676b6c5bbc-klzth evicted
pod/nginx-676b6c5bbc-8klhv evicted
pod/nginx-676b6c5bbc-dg6pv evicted
pod/coredns-5dd589bf46-fz4z9 evicted
pod/nginx-676b6c5bbc-sx6pd evicted
pod/nginx-676b6c5bbc-579th evicted
pod/metrics-server-5dc58b587c-cmg25 evicted
kubectl create pdb nginx --selector=app=nginx --min-available=2
pod/local-path-provisioner-846b9dcb6c-xzh9n evicted
node/control-plane drained
node/worker-1 drained
node/worker-2 drained
```

In a new control-plane root user tab, uncordon worker-1 and worker-2 and then switch back to the previous tab, this will allow Kubernetes to reschedule pods and adhere to the PDB requirements. This step shows how Kubernetes manages to maintain the minimum number of available pods specified in the PDB (this can take some time for the environment to normalise) -

```bash
kubectl uncordon worker-1 worker-2
```

```bash
kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx-676b6c5bbc-hh82j   1/1     Running   0          92s   10.42.3.8    worker-1   <none>           <none>
nginx-676b6c5bbc-qs5gr   1/1     Running   0          92s   10.42.3.10   worker-1   <none>           <none>
nginx-676b6c5bbc-t5v78   1/1     Running   0          92s   10.42.3.9    worker-1   <none>           <none>
nginx-676b6c5bbc-tw6jn   1/1     Running   0          92s   10.42.3.11   worker-1   <none>           <none>
nginx-676b6c5bbc-wt4ls   1/1     Running   0          92s   10.42.3.12   worker-1   <none>           <none>
```

Finally, uncordon the control-plane and clean up the environment by deleting the deployment and the Pod Disruption Budget -

```bash
kubectl uncordon control-plane; kubectl delete deployment/nginx pdb/nginx --now
```