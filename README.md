
# Init containers used in backend app 
    spec:
      initContainers:
      - name: init-myservice
        image: busybox:1.28
        command: ['sh', '-c', "until nslookup mysql; do echo waiting for myservice; sleep 2; done"]
      containers:
      - name: backend
        image: lingadevops/backend:v1.1

# It runs a loop in the shell:
until nslookup mysql → keep trying to resolve mysql
If it fails → print message and wait 2 seconds
Repeat until it succeeds

👉 In short: waits until the mysql hostname is available in DNS before continuing

# Note: Here we have discussed about init Containers only. liveness probe, readiness probe, resource limits, requests added here are optional.

* Init Containers are used to run setup tasks (like preload data, waiting for dependencies, Running DB migrations, setting permissions) before the main container starts. They’re not meant for running main services like DBs or app servers.

# How to create and delete all pods?
```
kubectl apply -f namespace.yaml
```
```
kubectl apply -f mysql/manifest.yaml
```
```
kubectl apply -f backend/manifest.yaml
```
```
kubectl apply -f frontend/manifest.yaml
```
```
kubectl apply -f debug/manifest.yaml
```

```
kubectl delete pods --all -n your-namespace
```

* This is classic load balancer:
```
http://afc79b680d5f34a1694fae92ec7cf3cc-617678930.us-east-1.elb.amazonaws.com

```