# LOKI-controller


Here you will find steps for installing/updating/customizing loki using helm


### Installing


```
helm repo add grafana https://grafana.github.io/helm-charts
```

```
helm repo update
```

Please customize the variable below for your own usage. For more information, please see [this link](https://grafana.com/docs/loki/latest/installation/helm/)


```
helm install --install loki loki/loki-stack  --set loki.persistence.enabled=true,nodeSelector."cloud\.google\.com/gke-nodepool"=system,loki.serviceMonitor.enabled=true --set loki.persistence.size=150Gi --set loki.persistence.storageClassName=loki-storage --set installCRDs=true -f loki_values_mainnet.yaml
 --namespace monitoring
```


#### Post-installation steps

Proceed to install k8s objects needed for the stack to work with:

``` 
kustomize build --load_restrictor=none ../  | kubectl apply -f - 
```

After installing the helm loki stack. You need to wait at least 4-8 mins until loki creates all the tables, the time will vary depending of the disk type and machine type selected. 

After the tables are all created, which you can verify by checking the logs of the loki pod. You need to create 2 datasources in order to start using loki:


##### Adding loki datasources
 
Go to grafana datasources, select add datasources. select loki as the database and fill the following values: 

```
name: Loki
URL: http://loki:3100
```

This will add loki as the datasource. you should test and verify all is fine before adding it.
 
Once is added. you can go to explorer tab, and do a quick query to the logs of any pod existent in the k8s cluster


##### Add prometheus loki as datasource 


Add a new datasource, select prometheus as database and fill the corresponding values:


```
name: Prometheus-loki 
URL: http://loki:3100/loki
```

This will let you access a prometheus endpoint in loki which let you count the incidences of the logs and lets you graph and set alarms on them
 

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
