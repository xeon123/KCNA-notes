# ClusterIP Service

Let's start by creating an `nginx` deployment using the `spurin/nginx-debug` image, we'll use 3 replicas and for now, we'll just view the yaml rather than executing, note the use of the label 'app: nginx' and a ports section -

`kubectl create deployment nginx --image=spurin/nginx-debug --port=80 --replicas=3 -o yaml --dry-run=client`

Let's execute and create this deployment -

`kubectl create deployment nginx --image=spurin/nginx-debug --port=80 --replicas=3`

We're going to expose this as a ClusterIP service, firstly, let's review the yaml declaration, notice how this has re-used the port information -

`kubectl expose deployment/nginx --dry-run=client -o yaml`

And let's execute this -

`kubectl expose deployment/nginx`

If we now check, we should see our nginx service listed as a ClusterIP -

`kubectl get service`

It will have created endpoints in the background and you can see these by querying endpoints with kubectl -

`kubectl get endpoints`

Each of these endpoints will relate to our nginx deployment pods -

`kubectl get pods -o wide`

If we run a describe on the service, we will also see the endpoints listed -

`kubectl describe service/nginx`

Let's capture our ClusterIP -

`CLUSTER_IP=$(kubectl get services | grep ClusterIP | grep nginx | awk {'print $3'}); echo $CLUSTER_IP`

And lets try accessing this, with curl -

`curl $CLUSTER_IP`

We can also access this service from another pod, let's spin up a curlimages/curl:8.4.0 pod -

`kubectl run --rm -it curl --image=curlimages/curl:8.4.0 --restart=Never -- sh`

And from this pod we'll use curl against the service DNS name of nginx.default.svc.cluster.local -

`curl nginx.default.svc.cluster.local`

Pods automatically inherit a DNS search path relating to the service, if we check /etc/resolv.conf, we can see this search path -

`cat /etc/resolv.conf`

This means that technically, we could also just curl nginx and it would work as expected -

`curl nginx`

Let's exit from this container -

`exit`

And let's delete this service -

`kubectl delete service/nginx`

---

## NodePort Service

This time, we are going to re-create our service but as a NodePort 

`kubectl expose deployment/nginx --type=NodePort`

If we check our services, we will see this listed as a NodePort with two ports, the first being for the application and the second being the NodePort Port

`kubectl get service`

Let's view the node information

`kubectl get nodes -o wide`

We'll capture the required information, firstly the IP of our control-plane node -

`CONTROL_PLANE_IP=$(kubectl get nodes -o wide | grep control-plane | awk {'print $6'}); echo $CONTROL_PLANE_IP`

And also the designated NodePort Port

`NODEPORT_PORT=$(kubectl get services | grep NodePort | grep nginx | awk -F'[:/]' '{print $2}'); echo $NODEPORT_PORT`

And using both of these, we can query the NodePort service from one of the Nodes -

`curl ${CONTROL_PLANE_IP}:${NODEPORT_PORT}`

Let's remove this service -

`kubectl delete service/nginx`

---

## LoadBalancer Service

We'll now create a LoadBalancer service using the built in k3s LoadBalancer by exposing a service of type LoadBalancer -

`kubectl expose deployment/nginx --type=LoadBalancer --port 8080 --target-port 80`

If we check services, we can see this listed with IP addresses (repeat command until this transitions from pending) -

`kubectl get service`

❗ If you're using Google Cloud Shell for the lab environment, you may find that the service briefly shows IP addresses but then, reverts back to pending. Further investigation with 'kubectl get all -A' may also show the LoadBalancer pods with an error. The issue relates to an incompatibility on the Google Cloud Shell setup with nf_tables and more recent changes in k3s to support nf_tables. If you encounter this, reverting the internal image used by the LoadBalancer to an earlier version that uses iptables legacy instead will resolve this, use the following script only if you encounter this issue -

`/resources/patch_klipper_iptables_legacy.sh`

Let's capture the first IP address shared by the LoadBalancer -

`LOADBALANCER_IP=$(kubectl get service | grep LoadBalancer | grep nginx | awk '{split($0,a," "); split(a[4],b,","); print b[1]}'); echo $LOADBALANCER_IP`

And we will also capture the LoadBalancer Port -

`LOADBALANCER_PORT=$(kubectl get service | grep LoadBalancer | grep nginx | awk -F'[:/]' '{print $2}'); echo $LOADBALANCER_PORT`

And let's watch the curl connections to the LoadBalancer, press ctrl-c to exit -

`watch --differences "curl ${LOADBALANCER_IP}:${LOADBALANCER_PORT} 2>/dev/null"`

Let's scale down our deployment to 1 replica and watch connections, press ctrl-c to exit -

`kubectl scale deployment/nginx --replicas=1; watch --differences "curl ${LOADBALANCER_IP}:${LOADBALANCER_PORT} 2>/dev/null"`

Let's scale up our deployment to 3 replicas and watch connections, press ctrl-c to exit -

`kubectl scale deployment/nginx --replicas=3; watch --differences "curl ${LOADBALANCER_IP}:${LOADBALANCER_PORT} 2>/dev/null"`

And now, we'll remove our deployment and our service

`kubectl delete deployment/nginx service/nginx`

---

## ExternalName Service

We're now going to look at the ExternalName service, firstly create a deployment called nginx-red with an image of spurin/nginx-red -

`kubectl create deployment nginx-red --image=spurin/nginx-red --port=80`

And create the equivalent with nginx-blue -

`kubectl create deployment nginx-blue --image=spurin/nginx-blue --port=80`

If we check our Deployments, we will see both of these listed -

`kubectl get deployment`

Let's expose deployment nginx-red -

`kubectl expose deployment/nginx-red`

And also expose deployment nginx-blue -

`kubectl expose deployment/nginx-blue`

And if we issue a get service, we can see both of these -

`kubectl get service`

Lets create an ExternalName service of my-service that points to nginx-red -

`kubectl create service externalname my-service --external-name nginx-red.default.svc.cluster.local`

Checking our services shows this additional ExternalName service -

`kubectl get service`

Let's take a look at this from the viewpoint of a pod, we'll execute a curl pod -

`kubectl run --rm -it curl --image=curlimages/curl:8.4.0 --restart=Never -- sh`

And from within the pod, let's query nginx-red -

`curl nginx-red`

And then, nginx-blue -

`curl nginx-blue`

And also our my-service which is currently pointing at nginx-red -

`curl my-service`

If we take a look with nslookup, we can see that my-service is a CNAME for nginx-red.default.svc.cluster.local -

`nslookup my-service`

Using the UI interface, bring up a new tab on the Control-Plane, log in, and within this tab, edit the my-service manifest (this will default to using vim), change red to blue and save the file and exit -

`kubectl edit service/my-service`

Go back to the original tab and once again issue an nslookup, it should now reference nginx-blue -

`nslookup my-service`

And if we curl my-service, we can see the blue html code -

`curl my-service`

Let's exit from this curl pod -

`exit`

And we will clean up our deployments and services -

`kubectl delete deployment/nginx-blue deployment/nginx-red service/nginx-blue service/nginx-red service/my-service`

---

## Headless Service

We will now take a look at Headless services which are a variation of ClusterIP, let's create a deployment using spurin/nginx with 3 replicas and a specified port of 80 -

`kubectl create deployment nginx --image=spurin/nginx-debug --replicas=3 --port=80`

To create a headless service, we need to modify a ClusterIP Yaml Declaration, capture the yaml from a basic ClusterIP Deployment and save to headless.yaml -

`kubectl expose deployment/nginx --dry-run=client -o yaml --type=ClusterIP | tee headless.yaml`

We then need to add an entry of 'clusterIP: None' under spec to this file, for convenience, this command will update the file as expected -

`grep -q 'clusterIP: None' headless.yaml || sed -i '/spec:/a\ \ clusterIP: None' headless.yaml; cat headless.yaml`

And we will apply this file -

`kubectl apply -f headless.yaml`

And can now see this as a service -

`kubectl get service`

Let's use our convenient curl pod to check this -

`kubectl run --rm -it curl --image=curlimages/curl:8.4.0 --restart=Never -- sh`

And with this setup, if we watch the output of nslookup nginx, we will see this dynamically changing -

`watch nslookup nginx`

And if we curl this (repeat the command multiple times) we will see this accessing different pods -

`curl nginx`

Let's exit -

`exit`

And finally, we will cleanup headless.yaml, our deployment and our service -

`rm headless.yaml; kubectl delete deployment/nginx service/nginx`