# argo-eks-addons
Deploy miscellaneous addons to EKS using ArgoCD

## Setup

1. Clone repo
2. Change `repoUrl` in `chart/values.yaml` to your repo

## useful commands

Log into argocd from the cli:
```
export ARGOCD_SERVER=`kubectl get svc argo-cd-argocd-server -n argocd -o json | jq --raw-output '.status.loadBalancer.ingress[0].hostname'`
export ARGOCD_PWD=$(aws secretsmanager get-secret-value --secret-id devops-eks-argocd | jq -r '.SecretString')
argocd login $ARGOCD_SERVER --username admin --password $ARGOCD_PWD --insecure
```

List argocd applications:
```
argocd app list
```

Sync argocd applications:
```
argocd app sync add-ons
```


## Sample application definition

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: add-ons
  namespace: argocd
spec:
  project: default
  syncPolicy:
    automated:
      prune: true
  source:
    repoURL: https://github.com/Pacobart/argo-eks-addons.git
    targetRevision: HEAD
    path: addons
    helm:
      release: add-ons
      values: |
        region: us-west-2
        account: XXXXXXXXXXXX
        clusterName: devops-eks
        
  destination:
    server: https://kubernetes.default.svc
    namespace: argocd
```
## How to setup new addon

1. create new directory in `addons`
2. add chart config
3. create new template in `chart\templates`
4. add variables to `chart/values.yaml`

## How to apply different values to environments

TODO