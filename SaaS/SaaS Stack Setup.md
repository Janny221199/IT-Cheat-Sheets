# Local File Setup
- create a folder structure for the SaaS project similar to other projects in 
```
~/Development/SaaS/XXX
├── config-repo
├── frontend
├── frontend-manifests
├── XXX
├── XXX-images
├── XXX-manifests
├── XXX-manifests.code-workspace
└── terraform
```
- copy and refactor those projects for the new SaaS application
# Github
## setup public image image repoitory
- named `XXX-images`
- upload marketing files
- create and upload logos
- generate and upload favicons (generated e.g. via https://realfavicongenerator.net/)

# Gitlab
- upload projects to a private repository
- if required add Team Members as Developer or less
- if required set ENV variables for CI/CD Pipeline
## Configuring normal Gitlab repository
1. create project and upload code
   ![](attachments/Pasted%20image%2020260716162555.png)
2. create branch `development`
   ![](attachments/Pasted%20image%2020260716162828.png)
3. protect main and development branch
   ![](attachments/Pasted%20image%2020260716162952.png)
   ![](attachments/Pasted%20image%2020260716163045.png)
4. Add the repository XXX to access `java-utils` CI_JOB_TOKEN within the `java-utils` repo (for a Spring Boot Microservice)
   ![](attachments/Pasted%20image%2020260716163337.png)
## Configuring `*-manifests` Gitlab repository
1. create project and upload code (see above)
2. Add the normal code repository to the projects Job Permission Token, so it can access the manifest repo via CI_JOB_TOKEN and update it. Also tick the checkboxes so they can push to the manifest repository within the pipeline
   ![](attachments/Screenshot%202026-07-25%20at%2021.35.56.png)

## Gitlab Access Tokens

| Scopes                       | Token-Name                                                      | Type                                                                                                                                                                                                                                                                                                                                                | Usage                                                                                                                                 |
| ---------------------------- | --------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `read_api`                   | Maven Dependency Token Local Development macOS For All Projects | Legacy Token (not fine-grained), because locally I want to access all projects which use the gitlab maven registry                                                                                                                                                                                                                                  | local `settings.xml` in `.m2` on MacBook ([Maven](../SaaS/SaaS%20Local%20Development.md#Maven))                                       |
| `Container Repository: Read` | `[APPLICATION]` Container Registry for K8S                      | Fine-Grained Token, with only the groups the application needs to access the container registry (docker images). For Orderant: `sanker-io/services` and `sanker-io/orderant`. The sub-groups are okay here, because non-K8S-deployed projects within the group do NOT have container images<br>![](attachments/Pasted%20image%2020260725195927.png) | as terraform secret for K8S to access Gitlab container registry in `environments/shared.secrets.tfvars`                               |
| `Code: Read, Download`       | `[APPLICATION]`  Config Repo                                    | Fine-Grained Token, with only the applications config-repo ![](attachments/Pasted%20image%2020260725200858.png)                                                                                                                                                                                                                                     | as terraform secret in secret-map for config service to access the applications config-repo in `environments/ENV/secrets.auto.tfvars` |
| `Code: Read, Download`       | `[APPLICATION]` Kustomize Manifests                             | Fine-Grained Token, with only the applications `*-manifests` repositories, the application require ![](attachments/Pasted%20image%2020260725211104.png)                                                                                                                                                                                             | as terraform secret for ArgoCD to access Gitlab Manifest Projects in `environments/shared.secrets.tfvars`                             |

# AWS
## Root Account
1. create Root User account, provide card details and so on.
2. enable MFA
3. select Region Frankfurt

## Account Name
1. Account Settings
   ![](attachments/Pasted%20image%2020260718181555.png)

## IAM Access to Billing Information
so we can do that as a non root user
1. Account Settings
   ![](attachments/Pasted%20image%2020260718181644.png)

## Configuring Account Alias
for login url
1. go to IAM (NOT IAM Identity Center)
2. on the right side set an Account Alias
   ![](attachments/Pasted%20image%2020260718182721.png)

## Billing Preferences 
1. Account Settings
2. Billing Preferences
3. Activate CloudWatch Billing Alerts AND AWS Free Tier Alerts AND PDF Invoice Delivery
   ![](attachments/Screenshot%202026-07-18%20at%2018.20.34.png)

## Monthly Budget Alerts
1. Account Settings
2. Budgets
3. Create a "Monthly Cost Budget" with the specific amount and provide emails (also `jan-eric@sanker.io`!!)
   ![](attachments/Screenshot%202026-07-18%20at%2018.23.52.png)

## IAM Account
for normal usage (do NOT use root account)
1. go to IAM (NOT IAM Identity Center)
2. create Group for Admins
   ![](attachments/Pasted%20image%2020260718182942.png)
3. create Admin User (I do without Identity Center, because I do not need SSO)
   ![](attachments/Screenshot%202026-07-18%20at%2018.35.29.png)
4. add to the Admins Group
5. enable MFA


# GCP
for Google Login
## Create GCP Project
1. create project for environment `XXX-ENV` within sanker.io Organization, make sure that project name equals project id
   ![](attachments/Pasted%20image%2020260720161301.png)

## Create GCP oAuth App 
1. Google Auth Platform: Create App
   ![](attachments/Pasted%20image%2020260720162159.png)
   ![](attachments/Pasted%20image%2020260720162238.png)
   ![](attachments/Pasted%20image%2020260720162329.png)
2. Add Branding (for local App domains can be empty)
   ![](attachments/Pasted%20image%2020260720163212.png)
   ![](attachments/Pasted%20image%2020260720163232.png)
3. Audience, set to `External` (for Production: Publish App; for Local: add Test Users)
   ![](attachments/Pasted%20image%2020260720163509.png)
4. Clients (download json file and store in keypass for prod and staging); NO "Authorized JavaScript origins" required, since it is all backend-based; ONLY Web App is enough since our iOS, Android, macOS etc. apps use our AS-WEB Console
   ![](attachments/Pasted%20image%2020260720163857.png)
5. add client-id and client-secret to config-repo
6. Data Access: "Add or remove scopes" -> `/auth/userinfo.email`, `/auth/userinfo.profile`, `openid`, (ggf. add more required scopes for the app e.g.: drive...)
   ![](attachments/Pasted%20image%2020260720164417.png)
7. if required enable Google APIs for specific scope. e.g enable Google Drive API for using google drive based actions