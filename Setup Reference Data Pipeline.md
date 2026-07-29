# Setup Reference Data Pipeline

This is the pipeline that deploys reference data only to a set of environments.

## Setting up the deploy template

We start with the [template](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/ReferenceData.yml&version=GBexamples/v1&_a=contents).

### Adding the correct environments

The environments you need to setup depend on your project. This basically should include all environments, except the environment that is used as a source of the reference data.

A good start is to grab the environment setup of your regular deployment pipeline and add extra environments to this if needed. Usually the only extra environment is when you use a build environment and that's the development environment. Below is how it's supposed to be setup and an explanation of the inputs.

```yaml
- name: 'Dataverse_Test'
  condition: 'succeeded()'
  displayName: 'Test'
  #variableGroup: 'Use if environment name is not the same as the name of variable group'
  dependsOn: 
  - BuildPackage
```

**name** — This must match the environments you setup in ADO.

**dependsOn** — Here you define on which stage this stage depends. You can add multiple if you want. Use 'BuildPackage' if you want it to start directly after the Prepare Package stage.

**displayName** — Display name to show for the stage in ADO while running the pipeline.

**condition** — Condition that decides if a stage must be deployed. `'succeeded()'` is the default, which means that it will deploy if the previous stage succeeded.

**variableGroup** — You can enable this and set it to the value of your variable group that belongs to your environment, in case the value for the name input is different.

### Setting the other inputs correctly

**CIBuildDefinition** — This is the build definition ID of the CI pipeline you already setup. This default value from the variable group should always be correct.

**solutionArtifact** — Name of the build artifact. You can put whichever name you want here.

**fileName** — Filename of the json file that contains the reference data configuration, without extension. If you have a different name than the default (ReferenceData), then you need to set the value.

**preReferenceDataSteps & extraReferenceDataSteps** — List of additional steps before or after performing the reference data import. Should be the same as the extra steps added in the regular deployment pipeline.

## Adding automated trigger

It's possible to add a trigger to automatically trigger this pipeline when the reference data is updated. This can be done by changing the trigger to the value below. Change the branch (main) to your default branch. Change the path (data/ReferenceData) to the location you export your reference data.

```yaml
trigger:
  batch: false
  branches:  
    include: 
    - main 
  paths:  
    include:
    - data/ReferenceData
```
