# Copying Environments

A copy / restore environment template is included with the pipeline templates. You can find it [here](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=%2FCopy%2FMain-CopyEnvironment.yml&version=GBreleases%2Fv1&_a=contents).

> **Warning:** When you are making full copies, keep in mind that data is copied and the legal consequences that may have. As HSO we should not make full copies from production to a sandbox environment to avoid data privacy issues.

## Granting permissions to the app registration to allow copying and restoring

In order to run this pipeline to actually copy or restore, you need to grant the deployment app registration permissions. You can find the instructions [here](https://docs.microsoft.com/en-us/power-platform/admin/powershell-create-service-principal#registering-an-admin-management-application). You probably need to ask an admin from the customer to do this.

If you don't have the permissions, you can still partially use the pipeline. You can run the pipeline with CopyType 'none' and perform the copy/restore manually. This way you can still run the pre/post actions automatically.

> **Warning:** When you grant the app registration this permission, you basically make this app registration Power Platform tenant admin. This means it can perform these (create/copy/restore/reset) actions on all environments, even the ones the app registration doesn't have any security roles on or is not added as a user to. It won't grant access to any data though. Make sure that access to the secret of this app registration is very restricted. A good practice would be to have the customer enter the secret in Azure DevOps / KeyVault and don't allow anyone from HSO to access it.

## Creating the pipeline

If your repository doesn't contain the following two files, copy them from the examples:

- [Pipeline](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/Copy.yml&version=GBexamples/v1&_a=contents)
- [Post Copy Template](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/Templates/PostCopySteps.yml&version=GBexamples/v1&_a=contents)

Next you create a new pipeline like you did with the [CI, export and deployment pipelines](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/HSOExtension/Templates/v1/#pipelines).

## Adding the correct environments for copying and restoring

Your environments may be different, so you need to update the [source and target environments](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/Copy.yml&version=GBexamples/v1&line=19&lineEnd=36&lineStartColumn=1&lineEndColumn=1&lineStyle=plain&_a=contents) to match the environments you setup.

You can opt in to remove the development and production environments as target environments to not accidentally copy or restore to those. That would also mean if you really need to restore a backup on those environments, you first need to update the pipeline. It's up to you what you prefer.

## Fixing the Tenant ID

Next you need to update the [Tenant ID](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/Copy.yml&version=GBexamples/v1&line=62&lineEnd=63&lineStartColumn=1&lineEndColumn=1&lineStyle=plain&_a=contents) to the tenant of the customer the Power Platform environments reside in. You can find the ID in the [Azure portal](https://portal.azure.com/#settings/directory). Alternatively you can ask the customer's IT department for the ID.

## Adding your custom steps

Very likely you will need to add custom steps to this pipeline for additional post copy actions. This is completely customer specific. This could be setting up reference data, environment variables, core essentials licenses or other things. You can add them in the correct order in the post copy template. Another good starting point is the list of extra steps you may have added in the post deployment part of the regular deployment template.
