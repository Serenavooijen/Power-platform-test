# Setup Build Update Pipeline

This is the pipeline to move your sprint/fix solutions from Development to Build and automatically split them into different component solutions. You only need to setup this pipeline if you have a build environment.

## Setting up the solution component map

A [configuration file](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/config/SolutionComponentMap.json&version=GBexamples/v1) is used to define which solution a component is put into. Most Dataverse components are already in this file. It may happen at some point that a type is missing. If that happens, you can add them to the file. Please contact [Martijn Vermaat](mailto:mvermaat@hso.com), [Erik Pellegrom](mailto:epellegrom@hso.com) or [Robert Raaijmakers](mailto:rraaijmakers@hso.com) if you run into this issue, so we can keep this file up to date. Next to that, you can change the names of the solutions everything is placed into if you wish. The solutions will automatically be created by the pipeline if they don't exist, so no need to create the solutions in the build environment in advance.

## Setting up the pipeline

As a basis, the [Examples](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/Build-Update.yml&version=GBexamples/v1) yaml file should be used. All variables have a default value that is aimed to be correct in the majority of cases. Verify all parameters and supply a different value if required.

### Export Stage

This stage performs the export from development and commits it to the SprintSolutions repository and creates a build artifact used in the import stage.

**CIBuildDefinition** — This is the build definition ID of the CI pipeline you already setup. This default value from the variable group should always be correct.

**commitUser & commitUserEmail** — This is the user and email the commits this pipeline will use. If you manually run this, keeping the defaults will make it be the person who queued the build. If you plan on scheduling this build, you need to modify these variables. See the part about adding a schedule below for details.

**commitWorkItemsQuery** — This is currently a preview feature. This will be documented soon. Leave empty for now.

**connectionReferencesFileName** — Name of the [ConnectionReferences.json](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/config/ConnectionReferences.json&version=GBexamples/v1) file. Default will work unless you renamed this file.

**deployMissingPackages** — Set to true if you want to automatically deploy missing packages when a solution import fails because of missing dependencies.

**sprintSolutionPrefix & sprintNr** — Format for the sprint solution. Default is to take it from the versioning variable group. Format is `'{sprintSolutionPrefix}{sprintNr}'`. If there is an exact match on the format, it will export that solution.

**releaseName & fixSolutionPostfix** — Format for the fixes solution. Default is to take it from the versioning variable group. Format of the fixes solution that is used is `'{releasename}{fixSolutionPostfix}'` and then sorted descending. If at least one is found, the top result will be exported.

**syncTimeout & asyncTimeout** — Timeout values for API calls to Dataverse. Keep to default unless you have a good reason to change it.

**connectionString** — Connection string to Dataverse. Keep to default unless you have a good reason to change it.

**preExportSteps & postExportSteps** — If you have any pre or post export steps, you can add them here.

### Import Stage

This will import the solutions from the export stage and split them into the component solutions.

**CIBuildDefinition** — This is the build definition ID of the CI pipeline you already setup. This default value from the variable group should always be correct.

**syncTimeout & asyncTimeout** — Timeout values for API calls to Dataverse. Keep to default unless you have a good reason to change it.

**connectionString** — Connection string to Dataverse. Keep to default unless you have a good reason to change it.

**connectionReferencesFileName** — Name of the [ConnectionReferences.json](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/config/ConnectionReferences.json&version=GBexamples/v1) file. Default will work unless you renamed this file.

**environment** — Name of the Dataverse environment. Change it if you have a different name for your environments in ADO.

**publisherFriendlyName** — Friendly name of the publisher you want to use in case a solution must be created for splitting the components.

**publisherName** — Unique name of the publisher you want to use in case a solution must be created for splitting the components.

**tenantId** — ID of the tenant where the Power Platform environments are. You can find this in the [Azure portal](https://portal.azure.com/#settings/directory) on any account that has an account on the tenant (can be a guest account).

**pluginDaxifToolsScriptPath** — Path to the DAXIF tools scripts folder that contains the plugin sync script (`Sync_Plugins_To_Build.ps1`). When set, the pipeline will run this script to register/sync plug-ins to the build environment after solution import. Leave empty (or omit) to skip plug-in publishing. Make sure to set the solution name in that ps1 file.

The CI artifact must include the DAXIF scripts. Set `daxifPath` and `daxifContent` in your `CI.yml` to include them in the artifact:

```yaml
# CI.yml — include DAXIF tools in the artifact
- template: 'CI/Main.yml@templates'
  parameters:
    ...
    daxifPath: '$(system.defaultworkingdirectory)/src/Plugins'
    daxifContent: |
        Hso.Dataverse*/**
        !Hso.Dataverse*Tests/**
        !**/obj/**
```

Then reference the artifact path in the Import stage (the `Daxif/` prefix comes from the artifact staging layout):

```yaml
pluginDaxifToolsScriptPath: '$(System.ArtifactsDirectory)/Daxif/Hso.Dataverse.Tools/Daxif'
```

**webResourcesPublishPath & webResourcesPublishSolution** — When both are set, the pipeline will publish web resources to the build environment using SPKL after solution import.

- webResourcesPublishPath: Path to the folder containing the web resource files (JavaScript/CSS/etc.). Defaults to `$(Build.ArtifactStagingDirectory)/WebResources`.
- webResourcesPublishSolution: Name of the Dataverse solution the web resources belong to (e.g. `HSO`). Leave empty to skip web resource publishing.

## Adding a schedule

You can optionally add a schedule to this pipeline to automatically run this pipeline. In this example you want to enable a nightly deployment to your test environment. It starts with scheduling this pipeline at a certain point in time.

To do that, you add the following schedule in the pipeline just above the 'trigger: none' line. This schedule means it will trigger every night at 0:00 UTC and will run on the main branch. Next to that, it will run even if there wasn't a new commit since the last run. This is required as the only changes may be in Dataverse itself.

```yaml
schedules:
- cron: "0 0 * * *"
  displayName: Daily Midnight Run 
  branches:
     include:
     - main
  always: true
```

Your TA can advise you on whether you want to have a scheduled build. The HSO best practice is to do this. It does require the development team to work in a more structured way and this will take the team a little bit to get used to. After a while, nightly deployments will be smooth and really help the testing and planning.

Next to that, you need to update the commitUser & commitUserEmail variable in the export stage to the below values. This will make the user in the commit to be taken from the variable group. This is needed as the default values for the variables when a scheduled build is triggered are NULL.

```yaml
commitUser: '$(CommitUsername)'
commitUserEmail: '$(CommitEmail)'
```
