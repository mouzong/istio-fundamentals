# istio Fundamentals

## 1 - Installation
Istio can be installed following three possible approaches:
- The command-line tool istioctl
- The Istio operator
- The Helm package installer

### 1.1 - Using the Command Line Tool Istioctl

```bash
# Install istioctl tool (version 1.28.2)
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.28.2 TARGET_ARCH=x86_64 sh -
```

```bash
# Switch into the Istio download folder
cd istio-1.28.2
```

```bash
# Add the istioctl binary to the system path
export PATH=$PWD/bin:$PATH
```

```bash
# Verify Istioctl is properly installed
istioctl version
```

```bash
# Install Istio i=unto your kubernetes cluster
istioctl install --set profile=demo -y
```

```bash
# Verify Istio installation for Istio versions < 1.18
istioctl verify-install
```

```bash
# for Istio version >= 1.18
kubectl get pods -n istio-system
```

## 2 - Deploy an application with Istio

```bash
# Activate automatic injection of the sidecar into the pods inside the default namespace
kubectl label namespaces default istio-injection=enabled

# Deploy sample bookinfo application in the default namespace
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

# verify pods have 2 containers running in each of them
kubectl get pods
```

## 3 - Visualize Traffic using Kiali
```bash
# Install addons on the cluster fr the sample app
kubectl apply -f samples/addons

# Check the rollout status of the kiali deployment
kubectl rollout -n istio-system status deployments/kiali

# Verify kiali service is active
kubectl get svc -n istio-system kiali

# Start kiali dashboard
kubectl dashboard kiali
```

```bash
# Install a gateway for traffic generation
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml

# Set INGRESS_PORT and INGRESS_HOST environment varibales
export INGRESS_HOST=<cluster-advertised-api>

# Export service PORT
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')

# Test access to product page application
curl "http://$INGRESS_HOST:$INGRESS_PORT/productpage"

# Generate traffic from terminal and watch on kiali dashboard
while sleep 0.01; do
  curl -sS "http://$INGRESS_HOST:$INGRESS_PORT/productpage" &> /dev/null
done
```
