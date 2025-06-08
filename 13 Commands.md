## Commands

### Namespaces

```bash
kubectl get namespaces
kubectl create namespace new-namespace-name
```

### Pods

```bash
**kubectl create -f def-pod.yaml**
**$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80**
**$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \  
--dry-run=client -o yaml > nginx-pod.yaml**
**$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80 \  
--dry-run=client -o json > nginx-pod.json**
**$ kubectl create -f nginx-pod.yaml  
$ kubectl create -f nginx-pod.json**
**$ kubectl apply -f nginx-pod.yaml  
$ kubectl get pods  
$ kubectl get pod nginx-pod -o yaml  
$ kubectl get pod nginx-pod -o json  
$ kubectl describe pod nginx-pod  
$ kubectl delete pod nginx-pod**
```

### ReplicaSets

```bash
**$ kubectl create -f redis-rs.yaml**
**$ kubectl apply -f redis-rs.yaml  
$ kubectl get replicasets  
$ kubectl get rs  
$ kubectl scale rs frontend --replicas=4  
$ kubectl get rs frontend -o yaml  
$ kubectl get rs frontend -o json  
$ kubectl describe rs frontend  
$ kubectl delete rs frontend**
```

### DeploymentSets

```bash
**$ kubectl create -f def-deploy.yaml**
**$ kubectl create deployment nginx-deployment \  
--image=nginx:1.20.2 --port=80 --replicas=3**
**$ kubectl create deployment nginx-deployment \  
--image=nginx:1.20.2 --port=80 --replicas=3 \  
--dry-run=client -o yaml > nginx-deploy.yaml**
**$ kubectl create deployment nginx-deployment \  
--image=nginx:1.20.2 --port=80 --replicas=3 \  
--dry-run=client -o json > nginx-deploy.json**
**$ kubectl create -f nginx-deploy.yaml  
$ kubectl create -f nginx-deploy.json**
**$ kubectl apply -f nginx-deploy.yaml --record  
$ kubectl get deployments  
$ kubectl get deploy -o wide  
$ kubectl scale deploy nginx-deployment --replicas=4  
$ kubectl get deploy nginx-deployment -o yaml  
$ kubectl get deploy nginx-deployment -o json  
$ kubectl describe deploy nginx-deployment  
$ kubectl rollout status deploy nginx-deployment  
$ kubectl rollout history deploy nginx-deployment  
$ kubectl rollout history deploy nginx-deployment --revision=1  
$ kubectl set image deploy nginx-deployment nginx=nginx:1.21.5 --record  
$ kubectl rollout history deploy nginx-deployment --revision=2  
$ kubectl rollout undo deploy nginx-deployment --to-revision=1  
$ kubectl get all -l app=nginx -o wide  
$ kubectl delete deploy nginx-deployment  
$ kubectl get deploy,rs,po -l app=nginx**
```

### DaemonSets

```bash
**$ kubectl create -f fluentd-ds.yaml**
**$ kubectl apply -f fluentd-ds.yaml --record  
$ kubectl get daemonsets  
$ kubectl get ds -o wide  
$ kubectl get ds fluentd-agent -o yaml  
$ kubectl get ds fluentd-agent -o json  
$ kubectl describe ds fluentd-agent  
$ kubectl rollout status ds fluentd-agent  
$ kubectl rollout history ds fluentd-agent  
$ kubectl rollout history ds fluentd-agent --revision=1  
$ kubectl set image ds fluentd-agent fluentd=quay.io/fluentd_elasticsearch/fluentd:v4.6.2 --record  
$ kubectl rollout history ds fluentd-agent --revision=2  
$ kubectl rollout undo ds fluentd-agent --to-revision=1  
$ kubectl get all -l k8s-app=fluentd-agent -o wide  
$ kubectl delete ds fluentd-agent  
$ kubectl get ds,po -l k8s-app=fluentd-agent**
```