# Setup PowerPages deployment

Setting up the deployment of a Power Pages Portal requires several steps. This page contains a summary of the steps.

## Prerequisites

The following prerequisites are applicable:

- The [Dataverse deployment templates](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/HSOExtension/Templates/v1/), including the service connection to OneHSO and environments, are setup.
- The [Initial setup of the portal](https://docs.hso.com/PowerPlatform/PowerPages/initial-setup.html) is completed for each target environment.

## Variable Groups

Portals deployment requires additional variables, these must be set up before the deployment can work.

### Dataverse_Global

In this variable group the following variables must be added:

- TenantId

### Dataverse_PowerPages

In this variable group the following variables must be added:

- PortalId
- PortalName

## Create a Power Pages repository

Create a git repository in Azure DevOps for source control for your Power Pages configuration.

Note that you should also [grant build service access to the repository](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/HSOExtension/Templates/v1/#grant-build-service-access-to-repositories) to make sure that the content may be stored in source control.

## Install the deployment template

The [examples-v1 repository](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=%2F&version=GBexamples%2Fv1-powerpages&_a=contents) contains pipeline definitions for this part. Copy the following files to the repository created in the previous step:

- build/Pipelines/Export-Portal.yml
- build/Pipelines/Deploy-Portal.yml
- build/Pipelines/Templates/Deploy-Portal.yml
- config/portal/SystemSettings_FileTypeRestriction.json
- config/portal/SystemSettings_NoFileTypeRestriction.json

You may need to make the following modifications:

- build/Pipelines/Export-Portal.yml: Define the right variable group for the export environment.
- build/Pipelines/Export-Portal.yml: You can export multiple Power Pages at once by providing multiple IDs. In that case you must also add multiple ID variables in the PowerPages variable group.
- build/Pipelines/Templates/Deploy-Portal.yml: You can deploy multiple Power Pages at once by providing multiple names. You can also specify the deploymentProfileSubFolder where the deployment profile per portal is located.
- build/Pipelines/Templates/Deploy-Portal.yml: You can set the value enableMaintenanceMode to true, this adds manual steps during the pipeline run to enable and disable the maintenance mode of the portal.
- config/portal/SystemSettings_FileTypeRestriction.json: Set the allowed file types based on the Dataverse environment configuration.
- config/portal/SystemSettings_NoFileTypeRestriction.json: Set the allowed file types based on the Dataverse environment configuration.

You should make the following modifications:

- build/Pipelines/Deploy-Portal.yml: Define the target environments used.

## Export pipeline

Create a new pipeline in Azure DevOps which references the following file:

- build/Pipelines/Export-Portal.yml

## Deploy pipeline

Create a new pipeline in Azure DevOps which references the following file:

- build/Pipelines/Deploy-Portal.yml

### Tips

Before you can perform a deployment, the deployment profiles must be setup and stored in your repository. You can use the following folder structure:

- `PowerPages/DeploymentProfiles/{EnvironmentName}.deployment.yml` — the filename of the deployment profile should end with `deployment.yml`.

In case you have multiple Power Pages, you can create a deployment profile per portal and per environment by creating a subfolder for each portal. E.g.:

- `PowerPages/DeploymentProfiles/YourPortalName/{EnvironmentName}.deployment.yml`

More information on setting up a deployment profile can be found [here](https://docs.hso.com/PowerPlatform/PowerPages/deployment.html).
