# What is minikube


# How to install 
- First update system packages 
```
sudo apt-get update 
```
- Install required packages 
```
sudo apt install -y curl wget apt-transport-https
```
- Install docker 
```
sudo apt-get install docker.io
sudo usermod -aG docker && newgrp docker
```
- Now, enable docker 
```
sudo systemctl enable --now docker
```
# Install Minikube
- First download minikube binary 
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
- Make it executable and move to path 
```
chmod +x minikube
sudo mv minikube /usr/local/bin/
```
# Install Kubectl 
- Install kubectl 
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
- Make it executable and move your path
```
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```
# Start minikube
```
minikube start --driver=docker --vm=true 
```

# Check cluster status 
```
minikube status
```
# Get nodes information
```
kubectl get nodes
```
# Stop Minikube 
```
minikube stop
```
# Delete Minikube cluster 
```
minikube delete
```

