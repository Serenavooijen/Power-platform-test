# Setup Export Pipeline

This pipeline is used to export your managed solutions and reference data, then commits them into source control. This is a mandatory pipeline.

## Setting up the pipeline

As a basis, the [Examples](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/Export.yml&version=GBexamples/v1) yaml file should be used. All variables have a default value that is aimed to be correct in the majority of cases. Verify all parameters and supply a different value if required.

### Source environment

First thing to verify is the [variable group](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/Export.yml&version=GBexamples/v1&line=55&lineEnd=56&lineStartColumn=1&lineEndColumn=1&lineStyle=plain&_a=contents) that is used as source environment. By default this is 'Dataverse_Build', change it to 'Dataverse_Development' if you don't use a build environment.

### Exporting Solutions

This stage exports the solutions.

**syncTimeout & asyncTimeout** — Timeout values for API calls to Dataverse. Keep to default unless you have a good reason to change it.

**connectionString** — Connection string to Dataverse. Keep to default unless you have a good reason to change it.

**artifactName** — Name of the build artifact that is generated. Keep to default unless you have a good reason to change it.

**configPath** — Path to the folder that contains all [configuration files](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/config&version=GBexamples/v1).

**powerAutomateSolution** — Unique name of the solution that contains your Power Platform components. Used to verify cloud flows and their connection references.

**solutionsPath** — Path to the folder where the solutions must be exported to. Keep to default unless you have a good reason to change it.

**solutionsToExport** — List of the solutions to export. Enable and update the list if your solutions are different than the default.

**publishWebresources** — Optionally, you can choose to publish the Web Resources you might have in your project to the development environment, before running the export steps. This ensures that the code in the solution is up-to-date with the main branch of your repository, and no local changes are pushed to the other environments.

To enable this, add the following parameter to the Export pipeline:

`publishWebresources: 'true'`

**Note:** If you use the Build environment, make sure that you have a default SPKL profile defined in your `spkl.json` file that points to the Web Resources solution on your build environment. For example:

```json
{
  "webresources": [
    {
      "profile": "default,debug",
      "root": "../../webresources/",
      "solution": "HSOWebresources",
      "autodetect": "yes",
      "deleteaction": "no",
      "files": []
    },
    {
      "profile": "development",
      "root": "../../webresources/",
      "solution": "Sprint2401",
      "autodetect": "yes",
      "deleteaction": "no",
      "files": []
    }
  ]
}
```

SPKL will pick up the top profile, pointing to your HSOWebresources (rename this if you use another naming convention, for example with a customer prefix) to update the solution on your Build environment.

### Exporting Reference Data

This stage exports reference data. If you don't have reference data (unlikely), you can change the default value of the 'exportReferenceData' parameter to false.

**connectionString** — Connection string to Dataverse. Keep to default unless you have a good reason to change it.

**configPath** — Path to the folder that contains all [configuration files](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/config&version=GBexamples/v1).

**referenceDataPath** — Path to the folder where all reference data must be exported. This folder shouldn't contain anything else than reference data as the export will delete the folder and recreate the contents.

**postDataExportSteps** — Extra steps you may have after exporting the reference data.

## Adding a schedule

You can optionally add a schedule to this pipeline to automatically run this pipeline. In this example you want to enable a nightly deployment to your test environment. It starts with scheduling this pipeline at a certain point in time. Your TA can advise you on whether you want to have a scheduled build. The HSO best practice is to do this. It does require the development team to work in a more structured way and this will take the team a little bit to get used to. After a while, nightly deployments will be smooth and really help the testing and planning.

First you need to update the commitUser & commitUserEmail variable in the export stage to the below values. This will make the user in the commit to be taken from the variable group. This is needed as the default values for the variables when a scheduled build is triggered are NULL.

```yaml
commitUser: '$(CommitUsername)'
commitUserEmail: '$(CommitEmail)'
```

Now the schedule setup depends on if you have a build environment or not.

### With build environment

If you have a build environment, your schedule will already be in the build update pipeline. You want this pipeline to trigger when the build update pipeline completes. You do that by updating the 'resources' part of the pipeline to the part listed below.

```yaml
resources:
  pipelines:
  - pipeline: buildUpdatePipeline
    source: 'Build Update' 
    trigger: true 

  repositories: 
  - repository: templates
    type: git
    name: 'HSO Best Practices/PP-099-PipelineTemplates'
    endpoint: OneHSO
    ref: refs/heads/releases/v1
```

You need to change the source to the display name of your build update pipeline.

### Without build environment

In this case you need to add the schedule here like below. To do that, you add the following schedule in the pipeline just above the 'trigger: none' line. This schedule means it will trigger every night at 0:00 UTC and will run on the main branch. Next to that, it will run even if there wasn't a new commit since the last run. This is required as the only changes may be in Dataverse itself.

```yaml
schedules:
- cron: "0 0 * * *"
  displayName: Daily Midnight Run 
  branches:
     include:
     - main
  always: true
```
