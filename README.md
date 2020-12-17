# pocket-core-internal-deployments
Pocket Core Internal Infrastructure


This repository is synchronized with the `pocket-infra` GKE cluster. We use the GitOps operator Flux and kustomize for YAML customization.

### Kustomize
To learn more about kustomize visit: https://github.com/kubernetes-sigs/kustomize/tree/master/docs

### Flux
To learn more about flux visit: https://docs.fluxcd.io/en/1.19.0/

### Note
Keep in mind that all changes made to this repo will affect the `pocket-infra` GKE cluster.

