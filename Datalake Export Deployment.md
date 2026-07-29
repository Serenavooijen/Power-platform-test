# Datalake Export Deployment

Setting up the export to datalake contains several steps. On this page you can find a summary of the steps. All the steps that aren't related to the deployment template are summarized with links to find more information at.

## Creating the Azure Resources

To create and setup the Azure resources like storage account in Azure, please see the Azure Integration Best Practice.

## Setting up the link on Development

To create the link, start by installing the AppSource app and then follow the Microsoft Documentation.

## Preparing for deployment

Before you can deploy, make sure that the Azure resources for that environment are in place and all prerequisites are met.

## Creating the deployment templates

The examples contain pipeline definitions for this part. You will need to make some modifications. It is assumed you have 1 or more Synapse configuration records and they are all part of the same solution. Next to that, it assumes the regular deployment pipelines are also already in place.

### Export pipeline

You can find the export pipeline here and you need to make the following changes:

- Change the variable group 'Dataverse_Build' to the correct variable group matching the environment you are exporting from.
- Change the default name of the exported solution (HSOSynapseLink) to the name of the solution that contains the link.

After that you can create the pipeline and run it.

### Deploy Pipeline

You can find the deploy pipeline here and you need to make the following changes:

- Change the trigger to match the branching strategy you are using.
- Change the trigger path to match the location of the exported solution in source control (relative path from the root).
- Update the environments to all the environments you want to deploy to. Same way you do it in the regular deployment pipeline.

### Deploy Template

You can find the deploy template here and you need to make the following changes:

- Change the default name of the solution to the name of the solution that contains the link in these two parts:
  - The parameter 'solutionsToPack' in the PreparePackage stage
  - The parameter 'datalakeSolution' in the deployment loop
- Update the list of configurations. You need 1 item in the array for every configuration you have.
  - The name parameter is the name of your Synapse Link
  - The prefix is used for the variable groups (see below)
  - Set synapseWorkspace to true if you have a Synapse workspace associated with the link

After doing that you need to update the variable groups for all environments you want the link on. You need to add the following values. Replace `{prefix}` with what you put in the template. The examples in the table assume you have set 'DL' as prefix in the pipeline template.

| Variable | DataType | Description/Value | Example Name | Example Value |
|---|---|---|---|---|
| {prefix}_SubscriptionId | String | Guid of the Azure subscription | DL_SubscriptionId | a39de949-ae40-4fb7-9a38-c5f148528f29 |
| {prefix}_ResourceGroupName | String | Name of the resource group | DL_ResourceGroupName | rg-ce-datalakeexport-t |
| {prefix}_StorageAccountName | String | Name of the storage account | DL_StorageAccountName | stcedatalakeexportt |
| {prefix}_SynapseWorkspace | String | Name of the Synapse workspace (no need to create it if you have set synapseWorkspace to false) | DL_SynapseWorkspace | syncedatalakeexportt |

After updating the group and template, you can create the pipeline and run it.
