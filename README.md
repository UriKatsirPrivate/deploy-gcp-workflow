# deploy-workflow
GitHub action to deploy a GCP workflow

## Prerequisites
[setup-gcloud GitHub action](https://github.com/google-github-actions/setup-gcloud)


## Inputs (all inputs are required)
1. project-id: This project will be used to deploy the workflow
2. workflow-name: The name of the workflow
3. workflow-definition-file-name: Path to the .yaml file containing the workflow definition.
4. service-account-name: The name of the service account (format should be {service account name}@{project id}.iam.gserviceaccount.com).
5. workflow-region: The workflow will be deployed to this region] (defaults to us-central1)

## Usage
* see [Creating encrypted secrets for a repository](https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository) 
    for instructions how to securely pass service account credentials to a Github action

```yaml
jobs:
  job_id:
    permissions:
      contents: 'read'
      id-token: 'write'

    steps:
    # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
    - uses: actions/checkout@v3
    
    # Authenticate to GCP
    - id: 'auth'
      uses: 'google-github-actions/auth@v0'
      with:
        workload_identity_provider: 'projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/my-provider'
        service_account: 'my-service-account@my-project.iam.gserviceaccount.com'

    - name: Deploy workflow
        uses: UriKatsirPrivate/deploy-workflow@v0
        with:
            project-id: <See inputs section>
            workflow-name: <See inputs section>
            workflow-definition-file-name: <See inputs section>
            service-account-name: <See inputs section>
            workflow-region: <See inputs section>
```
* see [this file](https://github.com/UriKatsirPrivate/deploy-workflow/blob/main/.github/workflows/main.yml) for a working sample.