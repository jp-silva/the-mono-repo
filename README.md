- colima start --kubernetes

# install sealed secrets in minikube
- kubectl apply -n kube-system -f https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.19.3/controller.yaml

# install ArgoCD in k8s
- kubectl create namespace argocd && kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
 
# access ArgoCD UI
- kubectl get svc -n argocd && kubectl port-forward svc/argocd-server 8080:443 -n argocd

# login with admin user and below token (as in documentation):
- kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 --decode && echo

# create a secret with a github token that can access the package registry
# then seal and apply that sealed secret
- kubectl create secret docker-registry dockerconfigjson-github-com \                                           130 ↵
--dry-run=client \
--docker-server=https://docker.pkg.github.com \
--docker-username=<username> \
--docker-password=<https://github.com/settings/tokens/new\> \
-o yaml > argocd/setup/githubsecret.yaml

- kubeseal <argocd/setup/githubsecret.yaml >argocd/setup/githubsealedsecret.json
- kubectl create -f argocd/setup/githubsealedsecret.json -n apps

