minikube addons enable ingress
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace

helm repo add metallb https://metallb.github.io/metallb
helm install metallb metallb/metallb

helm dependency build