# Pocket Core Internal Infrastructure


This repository is synchronized with the `pocket-infra` GKE cluster. We use the GitOps operator Flux and kustomize for YAML customization.


## Usage

For installing all the components described below, please give a check to the file [usage.md](usage.md)

Below you can see the name of the modules that we used with our k8s cluster


## CI/CD and kustomize

### Kustomize

To learn more about kustomize visit: https://github.com/kubernetes-sigs/kustomize/tree/master/docs

### Flux

To learn more about flux visit: https://docs.fluxcd.io/en/1.19.0/


## Monitoring

### Prometheus operator 

To learn more about grafana and prometheus installation visit: https://github.com/helm/charts/tree/master/stable/prometheus-operator 


### Loki 

To learn more about loki please visit: https://grafana.com/docs/loki/latest/installation/helm/

## Networking

### gloo 

To learn more about gloo please visit: https://docs.solo.io/gloo-edge/latest/getting_started/

### cert manager

To learn more about cert-manager please visit: https://cert-manager.io/docs/installation/kubernetes/


### Note

Keep in mind that all changes made to this repo will affect the `pocket-infra` GKE cluster.

### Directory Structure
``` 
.
├── README.md
└── mainnet
    ├── base
    │   ├── deployment.yaml
    │   ├── kustomization.yaml
    │   ├── service.yaml
    │   └── volume-claim.yaml
    ├── cert-manager
    │   └── cert-issuer.yaml
    ├── certificate.yaml
    ├── cluster-certificate-issuer.yaml
    ├── flux-patch.yaml
    ├── helm
    │   └── mainnet.yaml
    ├── kustomization.yaml
    ├── monitoring
    │   ├── blackbox.yaml
    │   ├── helm
    │   │   ├── README.md
    │   │   ├── loki.yaml
    │   │   ├── loki_values_mainnet.yaml
    │   │   └── prometheus-operator-mainnet-values.yml
    │   ├── kustomization.yaml
    │   ├── prometheus-additional.yaml
    │   └── tendermint-servicemonitor.yaml
    ├── overlays
    │   ├── chains.json
    │   ├── kustomization.yaml
    │   ├── networking
    │   │   ├── gateway-peers.yaml
    │   │   ├── gateway-pocket.yaml
    │   │   ├── gateway-proxy-prom-svc.yaml
    │   │   ├── kustomization.yaml
    │   │   ├── upstreams-grafana.yaml
    │   │   ├── upstreams-pocket-core.yaml
    │   │   ├── virtualservices-grafana.yaml
    │   │   └── virtualservices-pocket-core.yaml
    │   ├── pocket-core-4
    │   │   ├── config.json
    │   │   ├── kustomization.yaml
    │   │   ├── node-key-secret.enc.yaml
    │   │   ├── node-key-secret.yaml
    │   │   ├── node_key.json
    │   │   ├── patch.yaml
    │   │   ├── priv-val-key-secret.enc.yaml
    │   │   ├── priv-val-key-secret.yaml
    │   │   └── priv_val_key.json
    │   └── pocket-core-5
    │       ├── config.json
    │       ├── kustomization.yaml
    │       ├── node-key-secret.enc.yaml
    │       ├── node-key-secret.yaml
    │       ├── node_key.json
    │       ├── patch.yaml
    │       ├── priv-val-key-secret.enc.yaml
    │       ├── priv-val-key-secret.yaml
    │       └── priv_val_key.json
    ├── storage-class.yaml
    └── vpa.yaml
```

### Infrastructure architecture

![Infrastructure diagram](./assets/pokt-network-infrastructure-overview.png)
