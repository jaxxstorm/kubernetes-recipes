# Basic Commands

## Launch a cluster

This will launch a cluster, you need to specify a provider first:

```
curl -sS https://get.k8s.io | bash
```
 
## Pods

### Launch a Pod

This launches a pod with two replicas, replicas is optional
```
kubectl run my-nginx --image=nginx --port=80 --replicas=2
```

### List running pods

```
kubectl get pods
```

## Services

### List Services

```
kubectl get svc|services
```

### Detailed Service Info

```
kubectl describe service <servicename>
```

## Maintenance

### Pod Logs

```
kubectl logs mysql
```

More verbose logs:

```
kubectl logs mysql -p
```

# AWS Specific

## Launch a cluster

Launch a cluster in AWS. Assumes you have set up the [AWS cli](http://aws.amazon.com/cli)

```
export KUBERNETES_PROVIDER=aws; curl -sS https://get.k8s.io | bash
```

## Expose a service

```
kubectl expose rc <rc-name> --port=80 --type=LoadBalancer
```


