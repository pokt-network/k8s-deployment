# Networking


## Gloo

To learn more about gloo please visit: https://docs.solo.io/gloo-edge/latest/getting_started/

> kubectl create ns gw-mainnet

> helm install gloo-devnet gloo/gloo --namespace gw-mainnet --values mainnet/helm/mainnet.yaml

 
## Cert-manager

To learn more about cert-manager please visit: https://cert-manager.io/docs/installation/kubernetes/

For installing cert-manager, proceed with:


> kubectl create ns cert-manager 

> helm repo add jetstack https://charts.jetstack.io

> helm repo update

> kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.1.0/cert-manager.crds.yaml

> helm install cert-manager jetstack/cert-manager --namespace cert-manager  --version v1.1.0 --set installCRDs=true

In case you need dns verification functionality see:

helm upgrade --install    --wait  --version v1.1.0 --set extraArgs={--dns01-recursive-nameservers-only=true} --set installCRDs=true   --namespace cert-manager   "cert-manager"   jetstack/cert-manager


Please verify that you have cert-manager installed correctly by doing:

> kubectl get pods --namespace cert-manager


### Customize your certificates for your domains

After the cert-manager installation is done, you need to edit the file [certificate.yaml](../certificate.yaml) which has the ssl certificates and issuer

For more info, please refer to their official docs 
