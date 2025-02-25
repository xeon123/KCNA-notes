Here is your exercise reformatted in Markdown for clarity and better readability:

markdown



```bash
# Nginx Pod Exercises
### 1. Create an Nginx Pod Run a simple pod with a single Nginx container: 

kubectl run nginx --image=nginx
```


### 2. Query Running Pods

Check the running pods using `kubectl get pods`:

`kubectl get pods`

### 3. View Pod Logs

See the logs of the Nginx pod:

`kubectl logs pod/nginx`

### 4. Get Pod Details

Retrieve detailed pod information, including its IP address:

`kubectl get pods -o wide`

### 5. Capture and Verify Pod IP Address

Capture the pod's IP address and display it:

`NGINX_IP=$(kubectl get pods -o wide | awk '/nginx/ { print $6 }'); echo $NGINX_IP`

Ping the IP address from the control plane:

`ping -c 3 $NGINX_IP`

### 6. Verify Pod Accessibility from Nodes

Test connectivity from worker nodes:

`ssh worker-1 ping -c 3 $NGINX_IP $ ssh worker-2 ping -c 3 $NGINX_IP`

### 8. Use Port-Forward to Access Nginx Locally

Forward the Nginx pod to localhost on the control plane:

`kubectl port-forward pod/nginx 8080:80`

Access the page via the Reverse Proxy tab using:

- Instance: **Control Plane**
- Target: **localhost**
- Port: **8080**

Close the port-forward with `ctrl-c` when done.

### 9. Query Nginx Pod Using Curl

Run a throwaway pod to query the Nginx pod:

`kubectl run -it --rm curl --image=curlimages/curl:8.4.0 --restart=Never -- http://$NGINX_IP`

### 10. Run a Persistent Ubuntu Pod

Create a pod with the `sleep infinity` command and the Nginx IP as an environment variable:

`kubectl run --image=ubuntu ubuntu --env="NGINX_IP=$NGINX_IP" -- sleep infinity`

Check the pod's status:

`kubectl get pods -o wide`

Access the shell of the Ubuntu pod:

`kubectl exec -it ubuntu -- bash`

### 11. Query Nginx Pod from Ubuntu Pod

Install `curl` in the Ubuntu pod:

`apt update && apt install -y curl`

Query the Nginx pod:

`curl $NGINX_IP`

Exit the Ubuntu pod shell:

`exit`

### 12. Clean Up Pods

Delete the Nginx and Ubuntu pods:

`kubectl delete pod/nginx pod/ubuntu --now`

---

## YAML File Management

### 13. Generate and Apply Nginx YAML

Generate the YAML file for the Nginx pod:

`kubectl run nginx --image=nginx --dry-run=client -o yaml | tee nginx.yaml`

Apply the YAML file:

`kubectl create -f nginx.yaml`

Switch to the declarative approach:

`kubectl apply -f nginx.yaml`

### 14. Create and Apply Ubuntu YAML

Generate the YAML file for the Ubuntu pod:

`kubectl run --image=ubuntu --dry-run=client -o yaml ubuntu sleep infinity | tee ubuntu.yaml`

Apply the YAML file:

`kubectl apply -f ubuntu.yaml`

### 15. Combine YAML Files

Merge both YAML files into `combined.yaml`:

`{ cat nginx.yaml; echo "---"; cat ubuntu.yaml; } | tee combined.yaml`

Apply the combined YAML:

`kubectl apply -f combined.yaml`

### 16. Modify and Test Multi-Container Pod

Create a YAML file for a multi-container pod with an Nginx webserver and a sidecar:

```bash
cat <<EOF > combined.yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: webserver
    image: nginx
  - name: sidecar
    image: ubuntu
    args:
    - /bin/sh
    - -c
    - while true; do echo "\$(date +'%T') - Hello from the sidecar"; sleep 5; if [ -f /tmp/crash ]; then exit 1; fi; done
EOF
```


Apply the YAML file:

`kubectl apply -f combined.yaml`

Verify the pod status:

`kubectl get pods -o wide`

Check logs for the sidecar container:

`kubectl logs pod/mypod -c sidecar`

Crash the sidecar container:

`kubectl exec -it mypod -c sidecar -- touch /tmp/crash`

---

## Challenge!

Run a pod named `matrix` using the `spurin/cmatrix` image. If successful, you'll see a Matrix-style display:

`kubectl run -it --rm matrix --image=spurin/cmatrix`