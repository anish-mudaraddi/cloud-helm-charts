# Cert-manager

Cert-manager is a tool to manage certs.

Our chart configures cert-manager (as subchart) and includes pre-configured issuers including staging and production letsencrypt - to enable you to setup verified HTTPS certs for your web-apps

# Installation

```bash
helm repo add cloud-charts https://stfc.github.io/cloud-helm-charts/
helm repo update
helm install cert-manager cloud-charts/cert-manager -n cert-manager --create-namespace
```

# Configuration

## Enabling letsencrypt issuers 

To enable letsecrypt issuers, you need to add:

```yaml
cert-manager:
  
  # for testing your networing - PLEASE USE THIS TO TEST FIRST! 
  # this will prevent the ENTIRE department getting rate-limited!
  le-staging:  
    enabled: true

  # prod issuer
  le-prod:
    enabled: true
```


## Using letsencrypt ingress 

To enable letsencrypt issuer - you need to add an annotation to ingress resources and enable tls

> [!CAUTION]
> This is just an example - read the documentation on the helm chart your trying to install to see how to configure nginx ingress. 
> You might need to make your own - see [Ingress Controller Docs](https://kubernetes.io/docs/concepts/services-networking/ingress/) 

```yaml
ingress:
  annotations:
    # add the annotation
    cert-manager.io/cluster-issuer: "letsencrypt-prod" # or letsencrypt-staging or self-signed
    hosts:
      - name: myservice.example.com
        path: /
        port: http
    # specify tls and secret name
    tls:
      - secretName: my-le-cert
        hosts:
          - myservice.example.com
```
