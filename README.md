- podman machine init --cpus 2 --memory 2048 --rootful
- podman machine start
- minikube start --driver=podman

# install ArgoCD in k8s
- kubectl create namespace argocd
- kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# access ArgoCD UI
- kubectl get svc -n argocd
- kubectl port-forward svc/argocd-server 8080:443 -n argocd

# login with admin user and below token (as in documentation):
- kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo

# install sealed secrets in minikube
- kubectl apply -n kube-system -f https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.19.3/controller-norbac.yaml

# create a secret with a github token that can access the package registry
# then seal and apply that sealed secret
kubeseal <secrets.yaml >mysealedsecret.json
kubectl create -f argocd/setup/mysealedsecret.json -n apps

