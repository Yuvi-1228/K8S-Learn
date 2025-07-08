# NameSpace 


# Basic Commnand 
## Check Namespace list 
```
kubectl get namespace 
```
OR
```
kubectl get ns
```
- If your are not create namespace then it will use by default namespace. 

## To check running pods
```
kubectl get pods 
```
## To check the running pods of the specific namespace 
```
kubectl get pods -n <namespace-name>
```
## Create Namespace 
```
kubectl create ns <namespace-name: nginx>
```
## Delete Namespace
```
kubectl delete ns <namespace-name>
```

## Create Pod 
```
kubectl run nginx --image=nginx 
```

## create pods in the specific namespace
```
kubectl run nginx --image=nginx -n <namespace-name>
```

## Delete Pods 
```
kubectl delete pod <pod-name> -n <namespace-name>
```

## To see all information of pods
```
kubectl get all -n <namespace-name>
```

# Create Menifest File 
## Create namespace from menifest file using yml file 
```
sudo vi namespace.yml
```
### Paste the following script inside namespace file 
```
kind: Namespace
apiVersion: v1
metadata: 
    name: nginx
```
### Now Apply the namespace file to create and apply and update 
```
kubectl apply -f namespace.yml
```
## Create Pods
### create pods using menifest file 
```
sudo vi pod.yaml
```
### Paste the following script 
```
kind: Pod
apiVersion: v1
metadata:
    name: nginx-pod
    namespace: nginx
spec:
    containers:
    - name: nginx-container
      image: nginx:latest
      ports:
      - containerPort: 80
```
### now, apply pods
```
kubectl apply -f pod.yaml
```
### How to access pod after create 
```
kubectl exec -it <pod-name: nginx-pod> -n <namespace-name: nginx> -- bash
```
### To see debug or description of pod 
```
kubectl describe pod/<pod-name: nginx-pod> -n <namespace: nginx> 
```
## Create Deployement
### create deployment using menifest file 
```
sudo vi deployment.yaml
```
### Paste the following script 
```
kind: Deployment
apiVersion: apps/v1
metadata:
  name: nginx-deployment
  namespace: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      name: nginx-deploy-app
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```
### now, apply pods
```
kubectl apply -f deployment.yaml
```
### to check the deployment list 
```
kubectl get deployment -n nginx
```
### Scale your pods
```
kubectl scale deployment/<deployment-name> -n <namespace-name> --replicas=5
```
### Rolling Update
```
kubectl set image deployment/<deployment-name: nginx-dployment> -n <namespace-name: nginx> <pod-app-name: nginx>=nginx:1.27.3
```
### To get more information about pod 
```
kubectl get pod -n nginx -o wide
```

## Create Replicasets
- Same as deployment just change the kind name and metadata name 

## Daemon Sets 
- Same as deployment but just change the metadata and kind name and also remove replicas under spec object. 

## Job 


## Cron Job 



## persisten Volume 



## Persisten Volume Claim 


## Service command 
### To see all the imformation of pods 
```
kubectl get all -n <namespace-name>
```

### Expose Port
```
kubectl port-forward service/<service-name: nginx-service> -n <namespace-name: nginx> 80:80 --address=0.0.0.0 
```
- If you get error like permission denied then use bypass
```
sudo -E kubectl port-forward service/<service-name: nginx-service> -n <namespace-name: nginx> 80:80 --address=0.0.0.0
```

