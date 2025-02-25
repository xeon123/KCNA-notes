Create a job to calculate pi using the perl image -

```bash
kubectl create job calculatepi --image=perl:5.34.0 -- "perl" "-Mbignum=bpi" "-wle" "print bpi(2000)"
```

And watch the job completion, can take up to 60 seconds, press ctrl-c to exit -

`watch kubectl get jobs`

If we run a describe against this job, we can see that this completed -

`kubectl describe job/calculatepi`

Show the pods -

`kubectl get pods -o wide`

Let's capture the pod name -

`PI_POD=$(kubectl get pods | grep calculatepi | awk {'print $1'}); echo $PI_POD`

And lets view the logs from the pod, showing pi to 2000 places -

`kubectl logs $PI_POD`

Let's delete this job -

`kubectl delete job/calculatepi`

We'll capture the yaml from our pi job to a file called calculatepi -

`kubectl create job calculatepi --image=perl:5.34.0 --dry-run=client -o yaml -- "perl" "-Mbignum=bpi" "-wle" "print bpi(2000)" | tee calculatepi.yaml`

We're going to update this to include completions and parallelism, review these options using kubectl explain -

`kubectl explain job.spec | more`

Update the calculatepi.yaml to include completions of 20 and parallelism of 5 -

```yaml
cat <<EOF > calculatepi.yaml
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  name: calculatepi
spec:
  completions: 20
  parallelism: 5
  template:
    metadata:
      creationTimestamp: null
    spec:
      containers:
      - command:
        - perl
        - -Mbignum=bpi
        - -wle
        - print bpi(2000)
        image: perl:5.34.0
        name: calculatepi
        resources: {}
      restartPolicy: Never
status: {}
EOF
```

Apply this file, we'll add a small sleep and will watch the output of kubectl get pods, this will run until we have 20 pods with up to 5 pods at any time -

`kubectl apply -f calculatepi.yaml && sleep 1 && watch kubectl get pods -o wide`

We could from any of these pods, get the logs and see pi calculated -

`PI_POD=$(kubectl get pods | grep calculatepi | tail -1 | awk {'print $1'}); kubectl logs $PI_POD`

Remove the job, this will also delete all of the associated pods -

`kubectl delete job/calculatepi`

And if we check, all of these pods have been removed -

`kubectl get pods -o wide`

Let's cleanup, the yaml file -

`rm calculatepi.yaml`

## CronJobs

We'll re-use the previous example but we will repurpose it as a CronJob with a schedule of `* * * * * -`

```bash
kubectl create cronjob calculatepi --image=perl:5.34.0 --schedule="* * * * *" -- "perl" "-Mbignum=bpi" "-wle" "print bpi(2000)"
```

If we watch this, as the minute ticks over it should start, you may need to toggle the tutorial button to close this pane to see this, press ctrl-c to exit -

`watch kubectl get jobs`

Regardless of how many jobs are completed, we will only have 3 running in total, it may briefly step up to 4 but then it will go back to 3.

This is because of the successfulJobsHistoryLimit parameter. You can see this by running an edit on the job, whilst here, add in completions:20 and parallelism: 5 under spec.jobTemplate.spec, save the file and exit when done -

`kubectl edit cronjob/calculatepi`

Watch the jobs again, press ctrl-c to exit -

`watch kubectl get jobs`

Let's cleanup the CronJob, this will delete associated jobs and pods -

`kubectl delete cronjob/calculatepi`

We can confirm all jobs are removed -

`kubectl get jobs`

And all pods -

`kubectl get pods`