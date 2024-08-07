TODO: Terraform

# Gitlab Access Tokens

## Gitlab Main Account Access Tokens

| Scopes | Token-Name | Usage |
| ---- | ---- | ---- |
| `read_api` | Maven Dependency Token Local Development macOS | local `settings.xml` in `.m2` on MacBook ([Maven](SaaS%20Local%20Development.md#Maven))   |

TODO: add following access tokens
![](attachments/Pasted%20image%2020240717152206.png)

## Gitlab Second "Public" Gitlab Account Access Tokens
(because this account is added to the K8S Manifest projects and has only limited permissions)

| Scopes             | Token-Name                                                                                        |  Usage  |
| ------------------ | ------------------------------------------------------------------------------------------------- | --- |
| `write_repository` | write repository for gitlab CICD (MANIFESTS_TOKEN) to adjust K8S Manifests of a different project |  in gitlab main account as (global) CICD Variable `MANIFESTS_TOKEN` |

# Accounts

create required Accounts

- Google
- MongoDB
- Cloudflare
- Mail Server (Hetzner / AWS)
- Paypal
- Stripe
- Twilio

# GCP Project

TODO:

# GCP Billing

TODO:

# GKE Cluster
TODO: adjust network settings

1. Create Project
2. Enable Kubernetes API

Autopilot:
```cmd
gcloud container --project "salespects" clusters create-auto "autopilot-cluster-1" --region "europe-west3" --release-channel "regular" --network "projects/salespects/global/networks/default" --subnetwork "projects/salespects/regions/europe-west3/subnetworks/default" --cluster-ipv4-cidr "/17" --services-ipv4-cidr "/22"
```

# DNS and IPs

1. create static IPs: each for staging and production
names are important because they are referenced within the kubernetes manifests
```cmd
gcloud compute addresses create gateway-static-ip-staging --project=salespects --global  
gcloud compute addresses create frontend-static-ip-staging --project=salespects --global  
gcloud compute addresses create website-static-ip-staging --project=salespects --global  
gcloud compute addresses create gateway-static-ip-production --project=salespects --global
gcloud compute addresses create frontend-static-ip-production --project=salespects --global
gcloud compute addresses create website-static-ip-production --project=salespects --global
```

2. Add IPs in DNS (TODO: CLI cmd)
3. Add Domain Names in google managed certificate in kubernetes yml for gateway and frontend (TODO: outsource not in manifest file because gateway must be dynamic: https://chat.openai.com/share/26932bd3-d743-4c3a-b396-3dd2556fe0c0)

# ArgoCD

## Namespace
```cmd
kubectl create namespace argocd
```

## Install

1. create file `argocd-resource.yaml`:
```yaml
controller: # monitor resources for controller
  resources:
    requests:
      cpu: 0.5
      memory: 512Mi
      ephemeral-storage: 10Mi
    limits:
      cpu: 0.5
      memory: 512Mi
      ephemeral-storage: 10Mi
server:
  resources:
    requests:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 10Mi
    limits:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 10Mi
repoServer:
  resources:
    requests:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 256Mi
    limits:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 256Mi
dex:
  resources:
    requests:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 256Mi
    limits:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 256Mi
redis:
  resources:
    requests:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 10Mi
    limits:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 10Mi
applicationSet:
  resources:
    requests:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 10Mi
    limits:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 10Mi
notifications:
  resources:
    requests:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 10Mi
    limits:
      cpu: 0.25
      memory: 512Mi
      ephemeral-storage: 10Mi
```

2. run:
```cmd
helm repo add argo https://argoproj.github.io/argo-helm
helm -n argocd install argocd argo/argo-cd -f argocd-resource.yaml
```


## UI
### Port Forwarding
only accessible locally
```cmd
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
access via `https://localhost:8080`

### LoadBalancer
!NOT HTTPS SECURED! (Not Recommended)
```cmd
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
get external IP for argocd-server

### Ingress 
TODO: https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/
TODO: set argocd subdomain and allow only https

## First steps

### Get ArgoCD Default Password
```cmd
argocd admin initial-password -n argocd
```
(username: admin)

### ArgoCD Login
```cmd
argocd login URL
```
with port forwarding: `argocd login localhost:8080`

### ArgoCD Change Password
```cmd
argocd account update-password
```



# Sealed Secrets (Maybe)
not used atm because we store the secret file somewhere else and upload them manually at system init

TODO: adjust tutorial and how to create sealed secrets

1. install kubeseal locally
```cmd
brew install kubeseal
```

maybe `export USE_GKE_GCLOUD_AUTH_PLUGIN=True` and reconnect to cluster

2. Install Sealed Secrets Controller
```cmd
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets https://bitnami-labs.github.io/sealed-secrets
helm install sealed-secrets -n kube-system sealed-secrets/sealed-secrets
```


# MongoDB
- enable network access and database access
## Database access
- set database access with username and password and provide it in backend configuration

## Network access
- for local / staging: allow all IPs `0.0.0.0/0`
- for production: TODO: enable Peering or Private Endpoint


# Twilio
- create Message Service for sending sms with an alphanumeric sender (create message service with alpha sender for staging, production and local)

# Deploy Services
- deploy in that order and wait for some services. e.g. wait until discovery, config, zipkin and rabbitmq are up and running

## Staging
### ArgoCD
```cmd
kubectl create ns staging
kubectl -n staging apply -f docker-config-secret.yml
kubectl apply -f argo-repo-secret.yml
kubectl apply -f general-secret-staging.yml
kubectl apply -f Services/apps/zipkin-application-staging.yml
kubectl apply -f Services/apps/rabbitmq-application-staging.yml
kubectl apply -f Services/apps/config-application-staging.yml
kubectl apply -f Services/apps/discovery-application-staging.yml
kubectl apply -f Services/apps/notification-application-staging.yml
kubectl apply -f Services/apps/team-application-staging.yml
kubectl apply -f Services/apps/payment-application-staging.yml
kubectl apply -f Services/apps/user-application-staging.yml
kubectl apply -f Salespects/apps/salespects-application-staging.yml
kubectl apply -f Services/apps/gateway-application-staging.yml
kubectl apply -f Salespects/apps/frontend-application-staging.yml
kubectl apply -f Salespects/apps/website-application-staging.yml
```

### Manuell Kustomize
```cmd
kubectl create ns staging
kubectl -n staging apply -f docker-config-secret.yml
kubectl apply -f argo-repo-secret.yml
kubectl apply -f general-secret-staging.yml
kubectl apply -k Services/manifests/zipkin-manifests/overlays/staging
kubectl apply -k Services/manifests/rabbitmq-manifests/overlays/staging
kubectl apply -k Services/manifests/config-manifests/overlays/staging 
kubectl apply -k Services/manifests/discovery-manifests/overlays/staging 
kubectl apply -k Services/manifests/notification-manifests/overlays/staging
kubectl apply -k Services/manifests/team-manifests/overlays/staging
kubectl apply -k Services/manifests/payment-manifests/overlays/staging
kubectl apply -k Services/manifests/user-manifests/overlays/staging
kubectl apply -k Salespects/manifests/salespects-manifests/overlays/staging
kubectl apply -k Services/manifests/gateway-manifests/overlays/staging
kubectl apply -k Salespects/manifests/frontend-manifests/overlays/staging
kubectl apply -k Salespects/manifests/website-manifests/overlays/staging
```


## Production
### ArgoCD
```cmd
kubectl create ns production
kubectl -n production apply -f docker-config-secret.yml
kubectl apply -f argo-repo-secret.yml
kubectl apply -f general-secret-production.yml
kubectl apply -f Services/apps/zipkin-application-production.yml
kubectl apply -f Services/apps/rabbitmq-application-production.yml
kubectl apply -f Services/apps/config-application-production.yml
kubectl apply -f Services/apps/discovery-application-production.yml
kubectl apply -f Services/apps/notification-application-production.yml
kubectl apply -f Services/apps/team-application-production.yml
kubectl apply -f Services/apps/payment-application-production.yml
kubectl apply -f Services/apps/user-application-production.yml
kubectl apply -f Salespects/apps/salespects-application-production.yml
kubectl apply -f Services/apps/gateway-application-production.yml
kubectl apply -f Salespects/apps/frontend-application-production.yml
kubectl apply -f Salespects/apps/website-application-production.yml
```


### Manuell Kustomize
```cmd
kubectl create ns production
kubectl -n production apply -f docker-config-secret.yml
kubectl apply -f argo-repo-secret.yml
kubectl apply -f general-secret-production.yml
kubectl apply -k Services/manifests/zipkin-manifests/overlays/production
kubectl apply -k Services/manifests/rabbitmq-manifests/overlays/production
kubectl apply -k Services/manifests/config-manifests/overlays/production 
kubectl apply -k Services/manifests/discovery-manifests/overlays/production 
kubectl apply -k Services/manifests/notification-manifests/overlays/production
kubectl apply -k Services/manifests/team-manifests/overlays/production
kubectl apply -k Services/manifests/payment-manifests/overlays/production
kubectl apply -k Services/manifests/user-manifests/overlays/production
kubectl apply -k Salespects/manifests/salespects-manifests/overlays/production
kubectl apply -k Services/manifests/gateway-manifests/overlays/production
kubectl apply -k Salespects/manifests/frontend-manifests/overlays/production
kubectl apply -k Salespects/manifests/website-manifests/overlays/production
```



# Configure
## Create Prices
1. Create ProductGroup
2. Create Product(s)
3. Create monthly / yearly Price(s)
4. provide Product Group as frontend env

## set MongoDB search indexes

- e.g.: salespects contact search:
1. within "Browse Collections" click on "Search"  
2. click "Create Index"  
3. select "Json Editor"  
4. select collection "salespects.contacts"  
5. specify index name "contacts-index" (as specified in config mongo.collections.contacts.search-index)  
6. set following Json and save:

```json
{
  "analyzer": "caseInsensitiveWildcard",
  "searchAnalyzer": "lucene.keyword",
  "mappings": {
    "dynamic": true
  },
  "analyzers": [
    {
      "name": "caseInsensitiveWildcard",
      "tokenFilters": [
        {
          "type": "lowercase"
        }
      ],
      "tokenizer": {
        "type": "keyword"
      }
    }
  ]
}
```
