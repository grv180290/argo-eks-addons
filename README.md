# argo-eks-addons
Deploy miscellaneous addons to EKS using ArgoCD

## Setup

1. Clone repo
2. Change `repoUrl` in `chart/values.yaml` to your repo


## How to setup new addon

1. create new directory in `addons`
2. add chart config
3. create new template in `chart\templates`
4. add variables to `chart/values.yaml`

## How to apply different values to environments

TODO