minikube addons enable ingress
minikube addons list
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace

helm repo add metallb https://metallb.github.io/metallb
helm install metallb metallb/metallb

helm dependency build
helm dependency update
helm install backend-server .

#=============================
minikube start --kubernetes-version=v1.21.9 --nodes 2
minikube addons list
minikube addons enable metallb
minikube addons configure metallb
helm list
helm upgrade backend-server . --install

kubectl create secret tls videonetics-tls-secret -n default --key secrets/videonetics.key --cert secrets/videonetics.crt

base64 -w0 secrets/videonetics.crt