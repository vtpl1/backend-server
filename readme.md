# Install minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```
# Install kubectl
```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
# echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-$(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

# Install helm
```bash
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

# Start minikube
```bash
minikube start --nodes 5 -p multinode --kubernetes-version=v1.21.12 --driver=docker --docker-opt bip=172.16.4.1/16 
docker network inspect multinode
```

# Delete minikube
```bash
minikube delete -p multinode
minikube delete --all
```

# Test minikube
```bash
minikube profile list
minikube status -p multinode
kubectl get nodes
kubectl apply -f hello-deployment.yaml
kubectl rollout status deployment/hello
kubectl apply -f hello-svc.yaml
kubectl get pods -o wide
minikube service list -p multinode
curl  http://192.168.49.2:31000
```

# Delete deployment
```bash
kubectl get deploy
kubectl delete deploy hello
kubectl get svc
kubectl delete svc hello
```

# Use helm

kubectl create secret tls videonetics-tls-secret -n default --key secrets/videonetics.key --cert secrets/videonetics.cer
helm dependency build
helm dependency update
helm uninstall backend-server
helm upgrade backend-server . --install


minikube addons enable ingress
minikube addons list
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace

helm repo add metallb https://metallb.github.io/metallb
helm install metallb metallb/metallb

helm dependency build
helm dependency update
helm upgrade backend-server . --install -n vcpaas

#=============================
minikube start --kubernetes-version=v1.21.9 --nodes 2
minikube addons list
minikube addons enable metallb
minikube addons configure metallb
helm list
helm upgrade backend-server . --install

kubectl create secret tls videonetics-tls-secret -n default --key secrets/videonetics.key --cert secrets/videonetics.crt

base64 -w0 secrets/videonetics.crt