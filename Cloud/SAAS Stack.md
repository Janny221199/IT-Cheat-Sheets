TODO: Terraform

# Gitlab Access Tokens

## Gitlab Main Account Access Tokens

| Scopes          | Token-Name                                     | Usage                                                                                                                                       |
| --------------- | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `read_api`      | Maven Dependency Token Local Development macOS | local `settings.xml` in `.m2` on MacBook ([Maven](SaaS%20Local%20Development.md#Maven))                                                     |
| `read_registry` | `[APPLICATION]` Container Registry for K8S     | in [docker-config-secret.yml](SaaS%20Stack.md#docker-config-secret.yml) docker-config-secret.yml for K8S to access Gitlab container registy |

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

- eventually increase Quota (Disk Storage and in-use regional external IPv4)

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
   example in Hetzner:
   ![](attachments/Pasted%20image%2020240824192141.png)
1. Add Domain Names in google managed certificate in kubernetes yml for gateway and frontend and website (TODO: outsource not in manifest file because gateway must be dynamic: https://chat.openai.com/share/26932bd3-d743-4c3a-b396-3dd2556fe0c0)

# ArgoCD

## Namespace
```cmd
kubectl create namespace argocd
```

## Install

1. create file locally `argocd-resource.yaml`:
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

...wait until everything is set up successfully (check with `kubectl get all -n argocd`)

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
...save password in password manager


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
- set database access with username (`sanker`) and password and provide it in backend configuration

## Network access
- for local / staging: allow all IPs `0.0.0.0/0`
- for production: TODO: enable Peering or Private Endpoint


# Twilio
- create Message Service for sending sms with an alphanumeric sender (create message service with alpha sender for staging, production and local)
TODO insert image


# Configure Configuration / Secret Files for Kubernetes

## java config and config encryption key 
- create secure config encryption key for staging AND production and save in password manager
- adjust java spring config values and use encryption key for secrets
![](attachments/Pasted%20image%2020240824152703.png)
## argo-repo-secret.yml
- create a file `argo-repo-secret.yml`
- create new ssh key: `ssh-keygen -t rsa -b 4096`
- add the ssh key in the Gitlab Second "Public" Account (because this account is added to the K8S Manifest projects and has only limited permissions)
- without expiry date
- name: `[APPLICATION]-argocd-manifests` -> use a different key for each application to be able to revoke some later
![](attachments/Pasted%20image%2020240824143436.png)

- create a file `argo-repo-secret.yml`
```yaml
# TODO: dont have plain text secrets in File. use e.g. sealed SecretsapiVersion: v1
apiVersion: v1
kind: Secret
metadata:
	name: private-repo-creds
	namespace: argocd
	labels:
		argocd.argoproj.io/secret-type: repo-creds
stringData:
	type: git
	url: git@gitlab.com:sanker-io/
	sshPrivateKey: |
		-----BEGIN OPENSSH PRIVATE KEY-----
		XXX
		-----END OPENSSH PRIVATE KEY-----

  

# Public Key:
# ssh-rsa XXX salespects-argocd-manifests
```
- save file in password manager (as long as it is not a sealed secret)

## docker-config-secret.yml
- encode `auth` base64: `echo -n "jan-eric@sanker.io:[GITLAB_READ_REGISTRY_ACCESS_TOKEN]" | base64`
- -> use a different access token for each application to be able to revoke some later
- create file `dockerconfigjson.json` with json object with credentials:
```json
{
	"auths":{
		"registry.gitlab.com":{
			"username":"jan-eric@sanker.io",
			"password":"[GITLAB_READ_REGISTRY_ACCESS_TOKEN]",
			"email":"jan-eric@sanker.io",
			"auth":"[BASE64_ENCODED_AUTH_OBJECT_FROM_STEEP_BEFORE]"
		}
	}
}
```

- encode json object: `cat dockerconfigjson.json | base64` -> make sure the quotation marks are not removed (thats why we are using the file, because when we would provide the json object directly, bash could not handle the quotation marks)
- create file `docker-config-secret.yml`
```yaml
# TODO: dont have base64 encoded secrets in File. use e.g. sealed Secrets
apiVersion: v1
kind: Secret
metadata:
name: docker-config-secret
type: kubernetes.io/dockerconfigjson
data:
.dockerconfigjson: [BASE64_ENCODED_JSON_OBJECT_FROM_STEP_BEFORE]
```
- save file in password manager (as long as it is not a sealed secret)

## general-secret-staging.yml and general-secret-production.yml

- create file `general-secret-staging.yml` and `general-secret-production.yml` (add or remove data based on application requirements)
```yaml
apiVersion: v1
kind: Secret
metadata:
	name: general-secret-[staging/production]
	namespace: [staging/production]
type: Opaque
data:
	CONFIG_ENCRYPT_KEY: [BASE64_ENCODED_VALUE]
	CONFIG_PASSWORD: [BASE64_ENCODED_VALUE]
	CONFIG_SERVER_URL: [BASE64_ENCODED_VALUE]
	GOOGLE_API_KEY: [BASE64_ENCODED_VALUE]
	RABBITMQ_PASSWORD: [BASE64_ENCODED_VALUE]
```
- save file in password manager (as long as it is not a sealed secret)


# Deploy Services
- deploy in that order and wait for some services. e.g. wait until discovery, config, zipkin and rabbitmq are up and running

## Staging
- keep in mind that some services require other services. So do not start all at the same time. Also Google Autopilot cannot start pods if you start too many at the same time 
- website gateway only available when Pods, Ingress and certificates are successfully created. certificates need some time, check status with `kubectl get ManagedCertificate -n [staging/production]`
- list most common items: `kubectl get all -n [staging/production]`
- get logs: `kubectl logs -n [staging/production] [POD_NAME]`
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

## Create Admin Account
1. create / register Account via Postman (because for frontend service start we should provide productId as ENV, so we should do first steps via API)
	1. change Postman Host, Port, service-name suitable for live requests
	   ![](attachments/Pasted%20image%2020240826224428.png)
	   2. register user (with secure password)
   2. Login and set jwt and userId as Postman Variable
   3. change role in MongoDB Atlas
	    ![](attachments/Pasted%20image%2020240826224615.png)



## Create Prices
1. Create ProductGroup
   ![](attachments/Pasted%20image%2020240826224917.png)
2. Create Product(s) (with scopes)
   ![](attachments/Pasted%20image%2020240826225238.png)
3. Create monthly / yearly Price(s)
   ![](attachments/Pasted%20image%2020240826225418.png)
   ![](attachments/Pasted%20image%2020240826225548.png)
4. provide Product Group as frontend env (`DEFAULT_PRODUCT_GROUP_ID` in project `frontend-manifests`)
   ![](attachments/Pasted%20image%2020240826225747.png)

## set MongoDB search indexes
- let database collections be created at first after the initial startup of the services

- e.g.: salespects contact search:
1. within "Browse Collections" click on "Search"  
2. click "Create Index"  
3. select "Json Editor"
   ![](attachments/Pasted%20image%2020240826222821.png)
4. select collection "salespects.contacts"  
5. specify index name "contacts-index" (as specified in config `mongo.collections.contacts.search-index`)  
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

![](attachments/Pasted%20image%2020240826222937.png)
![](attachments/Pasted%20image%2020240826223051.png)


## Log Access for Pods for other IAM Users for the Staging Environment
### RBAC + IAM Access
1. User in Google IAM hinzufügen mit der Rolle `Kubernetes Engine Cluster Viewer`:
   ![](attachments/Pasted%20image%2020240830021734.png)
2. create file `staging-logs-role.yml` and adjust user accounts:
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: staging
  name: pod-reader-staging
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods", "pods/log"]
  verbs: ["get", "watch", "list"]
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: pod-reader-binding-staging
  namespace: staging
roleRef:
  kind: Role
  name: pod-reader-staging
  apiGroup: rbac.authorization.k8s.io
subjects:
# Google Cloud user account
- kind: User
  name: kirkasius@gmail.com # rene
```
3. apply file `kubectl apply -f staging-logs-role.yml`

### Access Logs
1. login to cluster via gcloud: `gcloud container clusters get-credentials [CLUSTER_NAME] --region [REGION] --project [PROJECT_NAME]`
2. list all pods: `kubectl get pods -n staging` or `kubectl get all -n staging`
3. copy Pod name
4. view logs: `kubectl [POD_NAME] logs  -n staging` 
(if there are several pods per deployment, you would probably have to look at the logs of the deployment and adjust the role authorization as well: `kubectl logs [DEPLOYMENT_NAME] -n staging`)

