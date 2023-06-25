TODO: Terraform

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

1. create static IPs: each api gateway and production for staging and production
names are important because they are referenced within the kubernetes manifests
```cmd
gcloud compute addresses create api-gateway-static-ip-staging --project=salespects --global  
gcloud compute addresses create frontend-static-ip-staging --project=salespects --global  
gcloud compute addresses create api-gateway-static-ip-production --project=salespects --global
gcloud compute addresses create frontend-static-ip-production --project=salespects --global
```

2. Add IPs in DNS (TODO: CLI cmd)
3. Add Domain Names in google managed certificate in kubernetes yml for gateway and frontend (TODO: outsource not in manifest file because gateway must be dynamic: https://chat.openai.com/share/26932bd3-d743-4c3a-b396-3dd2556fe0c0)

# ArgoCD

## Namespace
```cmd
kubectl create namespace argocd
```

## Install
TODO: maybe with helm
with GKE Autopilot it might fail at first (`Does not have minimum availability`), because it must assign initial resources. just wait 5 minutes

### with UI, SSO, multi cluster
```cmd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### without UI, SSO, multi cluster
```cmd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml
```

## UI
### LoadBalancer
!NOT HTTPS SECURED!
```cmd
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
get external IP for argocd-server

### Ingress 
TODO: https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/
TODO: set argocd subdomain and allow only https

### Port Forwarding
only accessible locally
```cmd
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
access via `https://localhost:8080`

## First steps
```cmd
argocd admin initial-password -n argocd
```

login
```cmd
argocd login URL
```
with port forwarding: `argocd login localhost:8080`

change credentials
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



# Deploy Services

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
kubectl apply -f Services/apps/gateway-application-staging.yml
kubectl apply -f Services/apps/notification-application-staging.yml
kubectl apply -f Services/apps/team-application-staging.yml
kubectl apply -f Services/apps/payment-application-staging.yml
kubectl apply -f Services/apps/user-application-staging.yml
kubectl apply -f Salespects/apps/salespects-application-staging.yml
kubectl apply -f Salespects/apps/frontend-application-staging.yml
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
kubectl apply -k Services/manifests/gateway-manifests/overlays/staging
kubectl apply -k Services/manifests/notification-manifests/overlays/staging
kubectl apply -k Services/manifests/team-manifests/overlays/staging
kubectl apply -k Services/manifests/payment-manifests/overlays/staging
kubectl apply -k Services/manifests/user-manifests/overlays/staging
kubectl apply -k Salespects/manifests/salespects-manifests/overlays/staging
kubectl apply -k Salespects/manifests/frontend-manifests/overlays/staging
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
kubectl apply -f Services/apps/gateway-application-production.yml
kubectl apply -f Services/apps/notification-application-production.yml
kubectl apply -f Services/apps/team-application-production.yml
kubectl apply -f Services/apps/payment-application-production.yml
kubectl apply -f Services/apps/user-application-production.yml
kubectl apply -f Salespects/apps/salespects-application-production.yml
kubectl apply -f Salespects/apps/frontend-application-production.yml
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
kubectl apply -k Services/manifests/gateway-manifests/overlays/production
kubectl apply -k Services/manifests/notification-manifests/overlays/production
kubectl apply -k Services/manifests/team-manifests/overlays/production
kubectl apply -k Services/manifests/payment-manifests/overlays/production
kubectl apply -k Services/manifests/user-manifests/overlays/production
kubectl apply -k Salespects/manifests/salespects-manifests/overlays/production
kubectl apply -k Salespects/manifests/frontend-manifests/overlays/production
```
