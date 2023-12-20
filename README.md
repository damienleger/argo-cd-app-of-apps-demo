# TESTS

Install ArgoCD

```bash
# bootstrap
kind create cluster
helm repo add argocd https://argoproj.github.io/argo-helm
helm upgrade --install argocd argocd/argo-cd -n argocd --create-namespace

# expose argocd UI
kubectl -n argocd port-forward service/argocd-server --address 0.0.0.0 8080:443

# display UI admin/<password>
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

Tests in chronogical order

(commit 5c7f60575f46a3fad904bb28250a8a642b810e89)

`kubectl create -f app-of-apps.yaml -n argocd`

app-of-apps view

![app-of-apps view]("screenshots/2023-12-20 14_17_51-app-of-apps - Application Details Tree - Argo CD — Firefox Nightly.png")

apps defined in app-of-apps not sync (autosync on app-of-apps.yaml not enabled)

![home view]("screenshots/2023-12-20 14_19_41-Applications Tiles - Argo CD — Firefox Nightly.png")

after manual sync of app-of-apps

![after manual sync]("screenshots/2023-12-20 14_19_41-Applications Tiles - Argo CD — Firefox Nightly.png")

after update of helm chart version in cert-manager.yaml (commit b64dab3a69bbdced9b8c4dcb97f22cda11c31e7b)
no automatic sync of cert-manager application is done

![after update of cert-manager helm chart version]("2023-12-20 14_31_15-Applications Tiles - Argo CD — Firefox Nightly.png")

after enabling of autosync in app-of-apps.yaml (commit ec6b5aac9a749913ede52df08eb1524354e103c6)

`kubectl apply -f app-of-apps.yaml -n argocd`

cert-manager is updated automatically, everything is synced

![cert-manager is updated]("2023-12-20 14_34_34-Applications Tiles - Argo CD — Firefox Nightly.png")
