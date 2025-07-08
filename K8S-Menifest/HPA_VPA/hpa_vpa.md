# HPA : Horizontal Pods Autoscaling


# VPA : Virtual Pods Autoscaling 



# KEDA : Kubernetes Event Driven Autoscaling 



# How to check metric of nodes or pods 
- Before using this you have to install and configuration metric on your system
```
kubectl top node
kubectl top pod
```

# If you using minikube then 
```
minikube addons enable metrics-server
```
# How to install Metric on kind cluster 
- to install metrics on kind cluster
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
- Edit the Metrics Server Deployment
```
kubectl -n kube-system edit deployment metrics-server
```
- After open metrics server deployment file and search for container.args and paste below security bypass
- Add the security bypass to deployment under container.args
```
- --kubelet-insecure-tls
- --kubelet-preferred-address-types=InternalIP,Hostname,ExternalIP
```

- Restart teh deployment file
```
kubectl -n kube-system rollout restart deployment metrics-server
```

- Verify the metrics server id running or not
```
kubectl get pods -n kube-system
kubectl top nodes
```

# For VPA
```
git clone https://github.com/kubernetes/autoscaler.git
cd autoscaler/vertical-pod-autoscaler
./hack/vpa-up.sh
```
- Verify the pod on VPA 
```
kubectl get pods -n kube-system
```
- First Clone the git repo from kubernetes 
- go to autoscaler then vertical pod autoscaler 
- run  vpa-up script 
- Now, create vpa yaml file 


# For create load generator for testing  
```
kuebctl run -i --tty load-generator --image=busybox -n apache /bin/sh
```
- Then write command 
```
while true; do wget -q -O- https://apache-service.apache.svc.cluster.local; done
```


