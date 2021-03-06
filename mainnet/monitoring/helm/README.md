# LOKI-controller


Here you will find steps for installing/updating/customizing loki using helm


### Installing

helm install --install loki loki/loki-stack  --set loki.persistence.enabled=true,nodeSelector."cloud\.google\.com/gke-nodepool"=system,loki.serviceMonitor.enabled=true --set loki.persistence.size=300Gi --set loki.persistence.storageClassName=mainnet-fast --set installCRDs=true -f loki_values_mainnet.yaml
 --namespace monitoring


### Updating

helm upgrade --install loki loki/loki-stack  --set loki.persistence.enabled=true,nodeSelector."cloud\.google\.com/gke-nodepool"=system,loki.serviceMonitor.enabled=true --set loki.persistence.size=300Gi --set loki.persistence.storageClassName=mainnet-fast --set installCRDs=true -f loki_values_mainnet.yaml
 --namespace monitoring


### Customizing

Please check the following docs to edit `loki_values_mainnet.yaml`

- https://grafana.github.io/loki/charts/
- https://povilasv.me/helm-kustomze-better-together/



# Prometheus-controller


Here you will find steps for installing/updating/customizing prometheus-controller using helm


### Installing

helm install prometheus-operator prometheus-community/prometheus-operator --namespace monitoring --set prometheus.prometheusSpec.serviceMonitorSelectorNilUsesHelmValues=false --set prometheusOperator.tlsProxy.enabled=false --set prometheusOperator.nodeSelector."cloud\.google\.com/gke-nodepool"=system --set prometheus.prometheusSpec.nodeSelector."cloud\.google\.com/gke-nodepool"=system --set alertmanager.alertmanagerSpec.nodeSelector."cloud\.google\.com/gke-nodepool"=system --set kubeDns.enabled=true --set prometheusOperator.admissionWebhooks.enabled=false  --set prometheusOperator.admissionWebhooks.enabled=false --set prometheus.prometheusSpec.replicas=1 --set alertmanager.alertmanagerSpec.replicas=1 --set installCRDs=true -f prometheus-operator-mainnet-values.yml


### Updating

helm upgrade --install prometheus-operator prometheus-community/prometheus-operator --namespace monitoring --set prometheus.prometheusSpec.serviceMonitorSelectorNilUsesHelmValues=false --set prometheusOperator.tlsProxy.enabled=false --set prometheusOperator.nodeSelector."cloud\.google\.com/gke-nodepool"=system --set prometheus.prometheusSpec.nodeSelector."cloud\.google\.com/gke-nodepool"=system --set alertmanager.alertmanagerSpec.nodeSelector."cloud\.google\.com/gke-nodepool"=system --set kubeDns.enabled=true --set prometheusOperator.admissionWebhooks.enabled=false  --set prometheusOperator.admissionWebhooks.enabled=false --set prometheus.prometheusSpec.replicas=1 --set alertmanager.alertmanagerSpec.replicas=1 --set installCRDs=true -f prometheus-operator-mainnet-values.yml


### Post-install steps

After finishing helm install. You need to change the ClusterIP service type of grafana to `Loadbalancer` in order to have your grafana accessed from the external network

kubectl edit svc prometheus-operator-grafana -n monitoring

Also, you need to install additional scraping configurations in order to prometheus to work 

kustomize build --load_restrictor=none ../ | kubectl apply -f -


### Uninstalling 

helm uninstall prometheus-operator -n monitoring


### Customizing


Please edit the file `prometheus-operator-testnet-values.yml` with the values you want. Also you can refer to the following docs:

- helm options https://github.com/helm/charts/tree/master/stable/prometheus-operator#configuration
- helm values or modifiyng prometheus-operator-testnet-values.yml https://github.com/helm/charts/blob/master/stable/prometheus-operator/values.yaml
