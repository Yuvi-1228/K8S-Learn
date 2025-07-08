# Service mesh 
- Istio is best tool for service mesh 



# How to install istio
- go to google for install insdie kind cluster : type istio service mesh and follow the document 
- if you want to install demo or local system go to goole and type istio demo and follow the document 


# For Kind Cluster 
Install kind cluster for istio
```
kind create cluster --name istio-testing
```



# For Local machine and demo 
- Go to the Istio release page to download the installation file for your OS, or download and extract the latest release automatically (Linux or macOS):
```
curl -L https://istio.io/downloadIstio | sh -
```
- Move to the Istio package directory. For example, if the package is istio-1.26.2:
```
cd istio-1.26.2
```
- The installation directory contains:
    * Sample applications in samples/
    * The istioctl client binary in the bin/ directory.

- Add the istioctl client to your path (Linux or macOS):
```
export PATH=$PWD/bin:$PATH
```
- Install Istio using the demo profile, without any gateways:
```
istioctl install -f samples/bookinfo/demo-profile-no-gateways.yaml -y
```
- Add a namespace label to instruct Istio to automatically inject Envoy sidecar proxies when you deploy your application later:
```
kubectl label namespace default istio-injection=enabled
```
- To verify istio injection enable 
```
kubectl get namespace -L istio-injection
```
# Install teh kubernetes Gateway API CRD
```
kubectl get crd gateways.gateway.networking.k8s.io &> /dev/null || \
{ kubectl kustomize "github.com/kubernetes-sigs/gateway-api/config/crd?ref=v1.3.0" | kubectl apply -f -; }
```

# Deploy the sample application 
```
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```
# Validate that the app is running inside the cluster by checking for the page title in the response:
```
kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"
```

# Open the application to outside traffic
## Create a Kubernetes Gateway for the Bookinfo application:
```
kubectl apply -f samples/bookinfo/gateway-api/bookinfo-gateway.yaml
```
## Change the service type to ClusterIP by annotating the gateway:
```
kubectl annotate gateway bookinfo-gateway networking.istio.io/service-type=ClusterIP --namespace=default
```
## To check the status of the gateway, run:
```
kubectl get gateway
```

# Access the application
- You will connect to the Bookinfo productpage service through the gateway you just provisioned. To access the gateway, you need to use the kubectl port-forward command:
```
kubectl port-forward svc/bookinfo-gateway-istio 8080:80
```
# View your dashboard 
## Before create dashboard requets product page 
```
for i in $(seq 1 100); do curl -s -o /dev/null "http://$GATEWAY_URL/productpage"; done
```
- Install Kiali and the other addons and wait for them to be deployed.
```
kubectl apply -f samples/addons
kubectl rollout status deployment/kiali -n istio-system
```
- Access the Kiali dashboard.
```
istioctl dashboard kiali
```

# Uninstall istio resources
```
istioctl uninstall --purge -y
```
# Delete istio related namespace
```
kubectl delete namespace istio-system
kubectl delete namespace istio-ingress
```
# unable istio namespace label 
```
kubectl label namespace default istio-injection-
```
# Verify istio uninstall
```
kubectl label namespace default istio-injection-
```

# Uninstall Process for Local machine or demo istio 
- The Istio uninstall deletes the RBAC permissions and all resources hierarchically under the istio-system namespace. It is safe to ignore errors for non-existent resources because they may have been deleted hierarchically.
```
kubectl delete -f samples/addons
istioctl uninstall -y --purge
```
- The istio-system namespace is not removed by default. If no longer needed, use the following command to remove it:
```
kubectl delete namespace istio-system
```
- The label to instruct Istio to automatically inject Envoy sidecar proxies is not removed by default. If no longer needed, use the following command to remove it:
```
kubectl label namespace default istio-injection-
```
- If you installed the Kubernetes Gateway API CRDs and would now like to remove them, run one of the following commands:
- If you ran any tasks that required the experimental version of the CRDs:
```
kubectl kustomize "github.com/kubernetes-sigs/gateway-api/config/crd/experimental?ref=v1.3.0" | kubectl delete -f -
```
- Otherwise: use this 
```
kubectl kustomize "github.com/kubernetes-sigs/gateway-api/config/crd?ref=v1.3.0" | kubectl delete -f -
```



# Architecture 
- Envoy : sidecar container/ it is allow proxy 
- istiod : It is control panel / sub-componet: Pilot: proxy configuration/ Citadel: resposible for certificate issuance and rotation / Galley: responsible for validating, ingesting, aggregating, transfer/ operation: operate with service mesh 


