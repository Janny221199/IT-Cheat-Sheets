# General Setup

## Local File Setup
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
## Github
### setup public image image repoitory
- named `XXX-images`
- upload marketing files
- create and upload logos
- generate and upload favicons (generated e.g. via https://realfavicongenerator.net/)

## Gitlab
- upload projects to a private repository
- if required add Team Members as Developer or less
- if required set ENV variables for CI/CD Pipeline
### Configuring normal Gitlab repository
1. create project and upload code
   ![](attachments/Pasted%20image%2020260716162555.png)
2. create branch `development`
   ![](attachments/Pasted%20image%2020260716162828.png)
3. protect main and development branch
   ![](attachments/Pasted%20image%2020260716162952.png)
   ![](attachments/Pasted%20image%2020260716163045.png)
4. Add the repository XXX to access `java-utils` CI_JOB_TOKEN within the `java-utils` repo (for a Spring Boot Microservice)
   ![](attachments/Pasted%20image%2020260716163337.png)
### Configuring `*-manifests` Gitlab repository
1. create project and upload code (see above)
2. add the second account `@Jes221199` as Maintainer without expiry date (for argocd access)
   ![](attachments/Pasted%20image%2020260716163757.png)
3. Allow Maintainers to push into main 
   ![](attachments/Pasted%20image%2020260716164022.png)
   ![](attachments/Pasted%20image%2020260716164041.png)
4. Add the normal code repository to projects that can access the manifest repo via CI_JOB_TOKEN
   ![](attachments/Pasted%20image%2020260716164537.png)

### Gitlab Access Tokens

#### Gitlab Main Account Access Tokens

| Scopes          | Token-Name                                     | Usage                                                                                           |
| --------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `read_api`      | Maven Dependency Token Local Development macOS | local `settings.xml` in `.m2` on MacBook ([Maven](../SaaS/SaaS%20Local%20Development.md#Maven)) |
| `read_registry` | `[APPLICATION]` Container Registry for K8S     | in terraform variable for K8S to access Gitlab container registry                               |

#### Gitlab Second "Public" Gitlab Account Access Tokens
(because this account is added to the K8S Manifest projects and has only limited permissions)

| Scopes             | Token-Name                                                                                        | Usage                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `write_repository` | write repository for gitlab CICD (MANIFESTS_TOKEN) to adjust K8S Manifests of a different project | in gitlab main account as (global) CICD Variable `MANIFESTS_TOKEN` |
| `read_repository`  | read repository for config repo and notification templates                                        | TODO XXX                                                           |

