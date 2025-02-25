
# Managing Kubernetes Resources and Namespaces

## Reviewing Resources and Namespaces
To review the resources currently running and their namespaces, note that Kubernetes components are predominantly assigned to the `kube-system` namespace:

```bash
kubectl get all -A
```

### Viewing Default Namespaces

Kubernetes provides several default namespaces, which can be listed with:

`kubectl get namespaces`

This command can be shortened to:

`kubectl get ns`

### Shortened Commands in Kubernetes

Many Kubernetes commands have shortnames. To see these, use the following command:

`kubectl api-resources | more`

For example:
- `pods` can be shortened to `po`.
- The output also shows whether a component is namespaced (e.g., `pod`) or not (e.g., `node`).

---

## Default Namespace Behavior

### Creating a Pod in the Default Namespace

If you create a pod without specifying a namespace, it will be assigned to the `default` namespace:

`kubectl run nginx --image=nginx`

Verify the pod is running by listing all resources across all namespaces:

`kubectl get all -A`

### Removing the Pod

To remove the `nginx` pod:

`kubectl delete pod/nginx --now`

---

## Working with Custom Namespaces

### Creating a Custom Namespace

Create a namespace called `mynamespace`:

`kubectl create namespace mynamespace`

To create a resource in this namespace, use the `-n` flag:

`kubectl -n mynamespace run nginx --image=nginx`

### Querying Pods in Namespaces

If you query pods without specifying a namespace, it defaults to the `default` namespace:

`kubectl get pods`

To query pods in `mynamespace`, use:

`kubectl -n mynamespace get pods`

---

## Configuring kubectl Default Namespace

### Viewing kubectl Configuration

Run the following command to view the current kubectl configuration:

`kubectl config view`

### Updating the Default Namespace

Update the kubectl configuration to use `mynamespace` as the default namespace:

`kubectl config set-context --current --namespace=mynamespace`

Verify the change:

`kubectl config view`

Now, querying pods without specifying the namespace will show resources in `mynamespace`:

`kubectl get pods`

### Reverting to the Default Namespace

Revert back to the `default` namespace:

`kubectl config set-context --current --namespace=default`

---

## Cleaning Up Resources

### Deleting a Namespace

Remove the `mynamespace` namespace, which also deletes all resources within it:

`kubectl delete namespace/mynamespace --now`

---

## 🎯 Challenges 🎯

1. **Create a Namespace**
    - Create a namespace called `myawesomenamespace`:
        `kubectl create namespace myawesomenamespace`
2. **Run an nginx Pod**
    
    - Run an nginx pod in `myawesomenamespace`:
        
        `kubectl -n myawesomenamespace run nginx --image=nginx`
        
3. **Set Default Namespace**
    
    - Update your configuration to use `myawesomenamespace` as the default namespace:
            
        `kubectl config set-context --current --namespace=myawesomenamespace`
        
4. **Verify Pod in Namespace**
    
    - Verify the nginx pod is running in `myawesomenamespace` using shortnames for pods:
        
        `kubectl get po`
        
5. **Delete the Namespace**
    
    - Delete the `myawesomenamespace` namespace:
                
        `kubectl delete namespace/myawesomenamespace --now`
        
6. **Revert Default Namespace**
    
    - Revert your configuration back to the `default` namespace:
        
        `kubectl config set-context --current --namespace=default`