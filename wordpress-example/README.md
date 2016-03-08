# Deploy wordpress on k8s in AWS

## Assumptions

You've deployed Kubernetes!

### Create EBS volumes

Do this twice

```
aws ec2 create-volume --availability-zone us-west-2a --size 20 --volume-type standard --region us-west-2
```

### Deploy MySQL Pod:

Update `mysql.yaml` to point to your volumes created previously

```
kubectl create -f mysql.yaml
```

Ensure it's running

```
kubectl get pod
NAME      READY     STATUS    RESTARTS   AGE
mysql     1/1       Running   0          9s
```

Check the logs
```
kubectl logs mysql
```

Expose the service:

```
kubectl create -f mysql-service.yaml
```

Now we deploy wordpress!

```
kubectl create -f wordpress.yaml
```

And expose the service:

```
kubectl create -f wordpress-service.yaml
```

Then you should be able to find the ingress loadbalancer. This takes a few minutes to provision and you have to wait for DNS to update:

```
kubectl describe svc wpfrontend
```

Okay, now we're upgraded. Let's delete the pod

```
kubectl delete -f wordpress.yaml
```

And then create a replication controller:

```
kubectl create -f wordpress-rc.yaml
```

Scale it up:

```
kubectl scale --replicas=3 -f wordpress-rc.yaml
```

And then do a rolling upgrade...

```

kubectl rolling-update wordpress --update-period=30s --image wordpress:latest
```
