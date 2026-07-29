# Setup Early Bound Pipeline

This is a scheduled pipeline that runs every night (schedule can be modified if needed in the yaml file) and generates early bound classes & TypeScript typing. This pipeline should be setup if you have either plug-ins or TypeScript (or both). The advantage of using this pipeline is that your early bound classes and typings are always more or less up to date with the Dataverse configuration. Next to that, if somebody deletes a column or removes a column from a form that is used in plug-ins or scripts, then your CI build will automatically start failing. This helps with detection of bugs.

## Setting up the pipeline

As a basis, the [Examples](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=%2Fbuild%2FPipelines%2FGenerate-Early-Bound.yml&version=GBexamples%2Fv1) yaml file should be used. This template requires quite a few modifications in some areas.

### Building the tools project

The first part that requires modification is the path to the tools project. This step makes sure the tools are compiled. Update the 'projects' input to point to the tools csproj like the format below.

```yaml
- task: DotNetCoreCLI@2
  displayName: Build Tools project
  inputs:
    command: build
    projects: '$(Build.SourcesDirectory)/<repositoryname>/<path to tools csproj folder>/*.csproj'
    arguments: '-c Debug
```

### Generating C# Early Bound Classes

The second part is the generation of the C# early bound classes. Delete this call to the template if you don't use plugins. Updating this one is quite simple as you use the variable from the above modification.

```yaml
- template: 'Templates/EarlyBoundGeneration/Main-Daxif.yml'
  parameters: 
    commitUser: '$(CommitUsername)'
    commitUserEmail: '$(CommitEmail)'
    dataverseUrl: '$(Url)'
    dataverseClientId: '$(ClientId)'
    dataverseClientSecret: '$(ClientSecret)'
    relativeToolsFolder: '<path to tools csproj folder>'
    srcRepoPath: '$(Build.SourcesDirectory)/<repositoryname>'
```

You take the first part and put it into the 'srcRepoPath' variable, then take the rest minus the `*.csproj` and put it into the 'relativeToolsFolder' input.

### Generating TypeScript typings

The last part to modify is for TypeScript. Delete this if you don't use TypeScript. Modification works the same way as for the C# template.

```yaml
- template: 'Templates/EarlyBoundGeneration/Main-XrmDefinitelyTyped.yml'
  parameters: 
    commitUser: '$(CommitUsername)'
    commitUserEmail: '$(CommitEmail)'
    dataverseUrl: '$(Url)'
    dataverseClientId: '$(ClientId)'
    dataverseClientSecret: '$(ClientSecret)'
    relativeToolsFolder: '<path to tools csproj folder>'
    srcRepoPath: '$(Build.SourcesDirectory)/<repositoryname>'
```
