# Setting Up the Kubernetes Dashboard
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
kubectl proxy --port=8001 --address=0.0.0.0 --accept-hosts='.*'
```
- Open the Dashboard in your browser:
```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

```

- Notes: If you using aws then may be you are facing login because of kubernetes security . It will working on localhost and https. So you can follow below step. 
# If you are using AWS EC2 then try this one 
- Use SSH Tunnel [connect EC2 on your local system]
- run this command 
```
ssh -i your-key.pem -L 8001:localhost:8001 ec2-user@your-ec2-public-ip

```
- Then, on the EC2 instance via SSH terminal:
```
kubectl proxy

```
- Now on your local laptop, open the same URL:
```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

- Use the token from the previous step to log in.

# Deleting the Cluster
- Delete the KIND cluster:
```
kind delete cluster --name my-kind-cluster
```



