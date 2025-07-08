# Taint 


## How to use taint
- It is used to run command in terminal 
```
kubectl taint node <your-node/cluster-name> prod=true:NoSchedule
```
## How to untaint
```
kubectl taint node <your-node/cluster-name> prod=true:NoSchedule-
```

# Tolerence 

## How to use 
- It is use in inside menifest file

