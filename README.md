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

# Switch into the Istio download folder
cd istio-1.28.2

# Add the istioctl binary to the system path
export PATH=$PWD/bin:$PATH

# Verify Istioctl is properly installed
istioctl version

# Install Istio i=unto your kubernetes cluster
istioctl install --set profile=demo -y

# Verify Istio installation for Istio versions < 1.18
istioctl verify-install

# for Istio version >= 1.18
kubectl get pods -n istio-system
```
