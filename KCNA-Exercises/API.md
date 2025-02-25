Query the kubernetes nodes -

`kubectl get nodes`

We can see the API requests that are being made by adding verbosity flags -

`kubectl get nodes --v=6`

The API resources and version can be referenced with api-resources -

`kubectl api-resources | more`

Re-review the previous get nodes command and note the https call that is being made -

`kubectl get nodes --v=6`

If we try the equivalent call ourselves it won't work as it is missing authentication information supplied by our .kube/config file -

`curl --insecure https://127.0.0.1:6443/api/v1/nodes?limit=500`

Let's run a kubectl proxy to interact with the API in the background, we'll store the pid information to a file to assist with cleaning this up later, press return after executing -

`kubectl proxy & echo $! > /var/run/kubectl-proxy.pid`

We adjust our previous request and this should now work via the proxy -

`curl -s http://127.0.0.1:8001/api/v1/nodes?limit=500 | more`

With our proxy running, we can use this to fetch an OpenAPI specification in v2 -

`curl localhost:8001/openapi/v2`

And in v3, if you desired you could redirect these to a file for use elsewhere -

`curl localhost:8001/openapi/v3`

Let's create a pod via the API, the process for generating this is explained in the video -

```bash
curl --location 'http://localhost:8001/api/v1/namespaces/default/pods?pretty=true' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data '{
    "kind": "Pod",
    "apiVersion": "v1",
    "metadata": {
        "name": "nginx",
        "creationTimestamp": null,
        "labels": {
            "run": "nginx"
        }
    },
    "spec": {
        "containers": [
            {
                "name": "nginx",
                "image": "nginx",
                "resources": {}
            }
        ],
        "restartPolicy": "Always",
        "dnsPolicy": "ClusterFirst"
    },
    "status": {}
}'
```
If we check, we can now see our pod that was created via the API -

`kubectl get pods`

We will also delete a pod, via the api, again the process for generating this is outlined in the video -

```bash
curl --location --request DELETE 'http://localhost:8001/api/v1/namespaces/default/pods/nginx' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--data '{
    "gracePeriodSeconds": 1,
    "propagationPolicy": "Background"
}'
```

And if we check our pods again, this will no longer exist -

`kubectl get pods`

We'll terminate the proxy

`kill $(cat /var/run/kubectl-proxy.pid)`

And remove our pid file -

`rm /var/run/kubectl-proxy.pid`