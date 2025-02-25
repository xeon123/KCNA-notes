Let’s create a simple deployment using nginx, as we do so we’ll tee the yaml to a file and we’ll apply it, at the same time, we’re going to revisit this yaml later -

```bash
kubectl create deployment nginx --image=nginx --dry-run=client -o yaml | tee nginx-deployment.yaml | kubectl apply -f -
```

If we query deployments, we will see our nginx deployment -

`kubectl get deployment`

We will now have a replicaSet, with a unique identifier, note the name of the identifier, make a reference to this for comparison later on -

`kubectl get replicaset`

If we query our pods, these will be named with the name of the deployment, replicaSet and a unique identifier -

`kubectl get pods -o wide`

Deployments have a history, lets check the initial rollout history -

`kubectl rollout history deployment/nginx`

At the moment there is only 1 section in our history, we can annote this to make use of this record -

`kubectl annotate deployment/nginx kubernetes.io/change-cause="initial deployment"`

And if we check again, we can see our annotation -

`kubectl rollout history deployment/nginx`

Currently our deployment has a single replica and it's using the nginx image, verify this using get deployment -

`kubectl get deployment -o wide`

Let's scale the number of replicas, we will run a watch command straight after to see this as it progresses, press ctrl-c to exit -

`kubectl scale deployment/nginx --replicas=10; watch kubectl get pods -o wide`

Checking our deployment again will show 10/10 replicas -

`kubectl get deployment -o wide`

Modify the nginx-deployment.yaml file so that it is using 12 replicas, for example -

```yaml
cat <<EOF > nginx-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 12
  selector:
    matchLabels:
      app: nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
EOF
```

Apply the update and watch the changes to pods in real time -

`kubectl apply -f nginx-deployment.yaml; watch kubectl get pods -o wide`

Check against deployments, it should now state 12/12 -

`kubectl get deployments -o wide`

Even though we scaled the deployment, this does not change from a history viewpoint -

`kubectl rollout history deployment/nginx`

If you review the yaml for the deployment, you will see additional context that has been added. Review the strategy of rollingUpdate and it's parameters -

`kubectl get deployment/nginx -o yaml`

Modify the nginx-deployment.yaml file so that it is using the image nginx:stable, for example -

```bash
cat <<EOF > nginx-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 12
  selector:
    matchLabels:
      app: nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:stable
        name: nginx
        resources: {}
status: {}
EOF
```

Let's apply these changes and watch the rollout as it happens

`kubectl apply -f nginx-deployment.yaml && kubectl rollout status deployment/nginx`

We will now see 12 replicas, in the deployment status -

`kubectl get deployments -o wide`

Changing the image would have resulted in the creation of a new replicaSet -

`kubectl get replicaset`

And we can see that our pods, are now using this new replicaSet -

`kubectl get pods -o wide`

Let’s track this change -

`kubectl annotate deployment/nginx kubernetes.io/change-cause="change image to nginx:stable"`

And let's check the rollout history -

`kubectl rollout history deployment/nginx`

We can also change a deployment image, using the CLI. Set the image to alpine and watch the rollout status -

`kubectl set image deployment/nginx nginx=nginx:alpine && kubectl rollout status deployment/nginx`

We will annotate this change as well -

`kubectl annotate deployment/nginx kubernetes.io/change-cause="change image to nginx:alpine"`

And check our rollout history again -

`kubectl rollout history deployment/nginx`

Set the image to nginx:perl but this time, we will watch the pod output in real time, keep an eye on the pod replicaSet name (the left most column) -

`kubectl set image deployment/nginx nginx=nginx:perl && watch kubectl get pods -o wide`

Let's annotate this change as well -

`kubectl annotate deployment/nginx kubernetes.io/change-cause="change image to nginx:perl"`

If you describe the deployment, you will now see references to the new and old replicaSets -

`kubectl describe deployment/nginx`

And you can see the replicaSets via kubectl -

`kubectl get replicaset -o wide`

If you review the rollout history, you'll see a correlation between the age and the revisions -

`kubectl rollout history deployment/nginx`

Let's see what happens, if a deployment fails, we will use a tag that doesn't exist ... 🍌 ... Owing to the RollingUpdate Strategy, we can lose a percentage of our containers before it interveens as a safeguard, press ctrl-c to exit -

`kubectl set image deployment/nginx nginx=nginx:bananas && watch kubectl get pods -o wide`

We'll annotate that this failed -

`kubectl annotate deployment/nginx kubernetes.io/change-cause="change image to nginx:bananas - failed"`

Review the replicaSets at this point as the deployment is using a combination of the failing replicaSet and the previous replicaSet -

`kubectl describe deployment/nginx`

We will rollback this change, to get us back to a working version -

`kubectl rollout undo deployment/nginx && kubectl rollout status deployment/nginx`

Note that revision 4, has now become revision 6-

`kubectl rollout history deployment/nginx`

If we review our replicaSets, you'll see that our pods, are now running under the previous replicaSet -

`kubectl get replicaset -o wide`

We can also implicity rollback to a specific revision. If we rollback to the initial deployment which is nginx alone, we’ll see that the first replicaSet, will become the replicaSet in use -

`kubectl rollout undo deployment/nginx --to-revision=1 && kubectl rollout status deployment/nginx`

We can now see that we're re-using our original replicaSet -

`kubectl get replicaset -o wide`

And that revision 1, has now become revision 7. Because of the earlier change where revision 4 became revision 5, our history is now 2,3,5,6,7 -

`kubectl rollout history deployment/nginx`

kubectl describe will show that it is using our original replicaSet -

`kubectl describe deployment/nginx`

Let's clean up the deployment and it's associated replicaSets -

`kubectl delete deployment/nginx --now`

🎯 **Challenge!** 🎯 Create a deployment called web-app using the image httpd -

`kubectl create deployment web-app --image=httpd`

🎯 **Challenge!** 🎯 Verify the deployment rollout status -

`kubectl rollout status deployment/web-app`

🎯 **Challenge!** 🎯 Scale deployment/web-app to 3 replicas -

`kubectl scale deployment/web-app --replicas=3`

🎯 **Challenge!** 🎯 Verify the number of pods for deployment/web-app -

`kubectl get pods`

🎯 **Challenge!** 🎯 Annotate the deployment/web-app with "This is my web-app deployment" -

`kubectl annotate deployment/web-app kubernetes.io/change-cause="This is my web-app deployment"`

🎯 **Challenge!** 🎯 Show the deployment/web-app history using "rollout history" -

`kubectl rollout history deployment/web-app`

🎯 **Challenge!** 🎯 Set the deployment/web-app image to an image that doesn't exist (httpd:nonexistent) -

`kubectl set image deployment/web-app httpd=httpd:nonexistent`

🎯 **Challenge!** 🎯 Check the deployment/web-app with rollout status, it should be stuck -

`kubectl rollout status deployment/web-app`

🎯 **Challenge!** 🎯 Annotate the deployment/web-app with "Switched to httpd:nonexistent - failed" -

`kubectl annotate deployment/web-app kubernetes.io/change-cause="Switched to httpd:nonexistent - failed"`

🎯 **Challenge!** 🎯 Undo the rollout using "rollout undo" and immediately watch the "rollout status" -

`kubectl rollout undo deployment/web-app && kubectl rollout status deployment/web-app`

🎯 **Challenge!** 🎯 Show the deployment/web-app history using "rollout history" -

`kubectl rollout history deployment/web-app`

🎯 **Challenge!** 🎯 Verify that the pods are running -

`kubectl get pods`

🎯 **Challenge!** 🎯 Finally, delete deployment/web-app -

`kubectl delete deployment/web-app --now`