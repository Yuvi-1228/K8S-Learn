# Install Kind cluster and kubectl 
## create a shell script and run it
```
#!/bin/bash

[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

VERSION="v1.30.0"
URL="https://dl.k8s.io/release/${VERSION}/bin/linux/amd64/kubectl"
INSTALL_DIR="/usr/local/bin"

curl -LO "$URL"
chmod +x kubectl
sudo mv kubectl $INSTALL_DIR/
kubectl version --client

rm -f kubectl
rm -rf kind

echo "kind & kubectl installation complete."
```
## Install Docker 
``` 
sudo apt-get install docker.io
```
## Give permission to user 
```
sudo usermod -aG docker $USER && newgrp docker
```
# Configuration Kind Cluster
## Setting Kind Cluster configuration file in yaml file
```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
- role: control-plane
  image: kindest/node:v1.31.2
- role: worker
  image: kindest/node:v1.31.2
- role: worker
  image: kindest/node:v1.31.2
```

## Create a cluster using configuration file 
```
kind create cluster --config=<config file name> --name=<clustername>
```
## verify cluster 
```
kubectl get nodes
kubectl cluster-info
```
## Accessing the cluster 
```
kubectl cluster-info
```
# Settig Kubernetes dashboard 
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

# Create a admin user create a dashboard-admin-user.yml file with the following content:
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

# Apply the configuration tools
```

kubectl apply -f dashboard-admin-user.yml
```
# Get the Access token Retrieve the token for the admin user
```

kubectl -n kubernetes-dashboard create token admin-user
```
- copy the token for use in the Dashboard login.

# Access the dashboard start the dashbaord using kubectl proxy:
```
kubectl proxy
```
# open dashboard in your browser 
```

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```
- Use the token from the previous step to log in.

# Delete the cluster 
```

kind delete cluster --name my-kind-cluster
```



# How to switch cluster 
# Check Available context 
```
kubectl config get-contexts

```
# Switch cluster 
```
kubectl config use-context <cluster-name:kind-cwy-cluster>
```
# Verify cluster 
```
kubectl config current-context
```

