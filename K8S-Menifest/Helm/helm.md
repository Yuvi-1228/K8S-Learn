# Helm 
- It is package manager of Kubernetes

# How to install Helm 
- Go to google and type helm install 
- download script and run on your system 
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

# How to create helm 
- Let's create aapche helm chart 
```
helm create apache=helm
```

- You will get all the template and you can change value according to your needs
- after that you have to package 
```
helm package . 
```
# How to install that packages 
```
helm install dev-apache apache-helm
```
# If you want to delete helm packages
```
helm uninstall dev-apache
```
# install helm chart with the namespace 
``` 
helm install >your-chart-name: dev-apache> <existing-chart-name : apache-helm> -n <namespace-name:dev-apache> --create-namesapce
```

# If you change something in value fiel for updagrade then use
- First you have to package file after change teh vlaue 
```
helm package apache-helm
```
- Now updgrade 
```
help upgrade <environment name: prd-helm> ./<package name : apache-helm> -n prd-apache
```
# If you want to rollback 
``` 
helm rollback prd-apache 1 -n prd-apache
```
# If you want to search install repo 
```
helm search repo nginx
```
# If you want to add repo on your system 
```
helm repo add stable https://charts.helm.sh/stable
```

