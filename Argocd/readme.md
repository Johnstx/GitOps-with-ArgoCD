kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

k port-forward svc/argocd-server 8080:80 -n argocd

k get secret argocd-secret -n argocd -o yaml

k get secret argocd-secret -n argocd -o jsonpath="{.data.admin.password}" | base64 -d; echo
k get secret argocd-secret -n argocd -o jsonpath="{.data.admin.password}" | base64 -d; echo

k patch secret argocd-secret -n argocd -p '{"stringData": {"admin.password": "'$(htpasswd -bnBC 10 "" passW0Rd | tr -d ':\n')'", "admin.passwordMtime": "'$(date +%FT%T%Z)'"}}'

brew install argocd

export ARGOCD_SERVER=localhost:8080
export
argocd login $ARGOCD_SERVER -argocd repo add  git@github.com:Johnstx/GitOps-with-ArgoCD.git --ssh-private-key-path /home/stax/.ssh/id_rsa


