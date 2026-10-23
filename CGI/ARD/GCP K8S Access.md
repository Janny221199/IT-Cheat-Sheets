Login with Google Account
```zsh
gcloud auth login
```

Get K8S Configs (One-Time or update)
```zsh
gcloud container clusters get-credentials ard-infra --region europe-west3 --project ard-infra-deo
gcloud container clusters get-credentials ard-dev --region europe-west3 --project ard-dev
gcloud container clusters get-credentials ard-test --region europe-west3 --project ard-test-deo
gcloud container clusters get-credentials ard-qa --region europe-west3 --project ard-qa
gcloud container clusters get-credentials ard-prod --region europe-west3 --project ard-prod
gcloud container clusters get-credentials ard-common --region europe-west3 --project ard-common
```

Select Kubernetes Cluster
```zsh
kubectx
```

Connect to Bastion Host
```zsh
gcloud compute ssh bastion --project=ard-common --zone=europe-west3-a --tunnel-through-iap -- -4 -L 8888:localhost:3128 -N -q -f
```

Set environment variable HTTPS_PROXY for using the Bastion Host
```zsh
HTTPS_PROXY=127.0.0.1:8888 kubectl get pods
```