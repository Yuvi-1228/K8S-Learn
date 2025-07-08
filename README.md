# Kubernetes 
- K8S is a orchestration system which is manage to multiple container


# Architecture of K8S
## Master Node
- Control Room ( Head Of system ) which is help to control whole system. It will decide what, where and when system will run. 
## Master Node Parts (Family of master nodes)
### API SERVER 
- It is a gatekeeper. It is record and observation to all the 
### Scheduler 

### Control Manager 

### etcd 

## kubectl 
- It is a communication 



## Worker Nodes
### Kubelet 

### Container Runtime 


### kube Proxy 



# Pod 



# Kubernetes Cluster type 
- Kubeadm : 
- Minikube : Local or single EC2 
- Kind Cluster : 
- EKS / AKS / GKS


# If you want to set default context (cluster)
- kubectl get use-context <your-cluster-name>


# If you want see cluster log in every 2 sec 
```
watch kubectl get nodes
```


# Process 
- Docker [Nginx] ___ Pods___Deployement___Services____Ingress_____User
- Docker [MySQL] ___ Pods___Deployement___Services____Ingress_____User
- Namespace [ NS] - Resources [ Pods , deploymnet......]

# What is Deployment , replica and stateful sets?
## Deployment set
- Provides declarative updates for Pods and ReplicaSets. Most commonly used controller.
- 
## Replication set 
-  Ensures that a specified number of identical pods are running at all times.

## stateful set 
- Manages stateful applications, maintaining persistent identity and storage for each pod.

## Daemon Sets
- Ensures that a copy of a pod runs on every node (or a subset of nodes) in the cluster.

## Jobs
- 

## Cron jobs


## Persistent Volume 

## persisten volume Claim 


# Stateless [ Like Frontend changeable or consistency work]
## Generally use 
- Deployment 
- replicasets
- darmonsets
# Steteful [like backend their data is persistent]
## Genrally use 
- statefulsets : Pods are arrange in sequence or number wise. 



# How to Setup Kubernetes Dashbaord
- Deploy the Dashboard Apply the Kubernetes Dashboard manifest:
```

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```
- Create an Admin User Create a dashboard-admin-user.yml file with the following content:
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```
  - Apply the configuration:
```
kubectl apply -f dashboard-admin-user.yml
```

- Get the Access Token Retrieve the token for the admin-user:
```
kubectl -n kubernetes-dashboard create token admin-user
```
- Copy the token for use in the Dashboard login.

- Access the Dashboard Start the Dashboard using kubectl proxy:
```

kubectl proxy
```
- Open the Dashboard in your browser:
```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```
- Use the token from the previous step to log in.

# Delete kind Cluster 
```

kind delete cluster --name my-kind-cluster
```
