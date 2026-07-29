# Deploy Webresources

> **Tip:** This pipeline depends on the Set V1 Templates. Make sure to follow the steps in this guide first.

## Sparkle.XRM (SPKL)

In the best practices on Script Development (PP-017), SPKL is used to publish webresources. This guide on how to automate this in your pipeline assumes that you use this setup.

## Creating the Pipeline

To deploy the webresources from your development source control to a certain target environment, you can use the following step.

Before you can use this template, you need to make sure that (if you use TypeScript) the JavaScript is already compiled. You can find an example in the Build Typescript guide for this.

Add a reference to the templates repository:

```yaml
resources:
  repositories: 
    - repository: templates
      type: git
      name: 'HSO Best Practices/PP-099-PipelineTemplates'
      endpoint: OneHSO
      ref: refs/heads/releases/v1
```

Make sure that you checkout both the Template-project, as well as your own code in the pipeline:

```yaml
steps:
- checkout: self
  clean: true
  persistCredentials: true
- checkout: templates
```

The template to deploy the webresources looks as follows:

```yaml
- template: 'CI/Template-PublishWebresources.yml@templates'
  parameters:
    repoPath: '$(Build.SourcesDirectory)/PP-099-PipelineTemplates'
    spklDir: '$(system.defaultworkingdirectory)'
    spklConfigDir: '$(system.defaultworkingdirectory)/Field Service/TypescriptSolution/'
    spklProfile: 'deploy'
    clientId: $(ClientId)
    clientSecret: $(ClientSecret)
    url: $(Url)
```

### Parameter Explanation

| Parameter Name | Required (Yes/No) | Description |
|---|---|---|
| repoPath | Yes | The path to the pipelines repository. Always equal to the place that you checked out the pipelines repo in (`'$(Build.SourcesDirectory)/PP-099-PipelineTemplates'`). |
| spklDir | Yes | Where your spkl.exe (available after npm install) is located. This searches recursively, so passing `'$(system.defaultworkingdirectory)'` will always work. |
| spklConfigDir | Yes | The folder where your spkl.json configuration file is located. |
| spklProfile | No | The profile in the spkl.json that you want to use. If none specified, it will use the default profile. |
| clientId | Yes | The Client ID used to log in to Dynamics. |
| clientSecret | Yes | The Client Secret used to log in to Dynamics. |
| url | Yes | The URL to the target environment. |

## Complete example

Below, you will find a complete example of the whole pipeline.

```yaml
trigger:
- master
pool:
  vmImage: windows-latest
resources:
  repositories: 
    - repository: templates
      type: git
      name: 'HSO Best Practices/PP-099-PipelineTemplates'
      endpoint: OneHSO
      ref: refs/heads/releases/v1
variables:
- group: Dataverse_Development
- group: Dataverse_Global
steps:
- checkout: self
  clean: true
  persistCredentials: true
- checkout: templates
- task: Npm@1
  displayName: 'NPM Install'
  inputs:
   command: 'custom'
   workingDir: '$(system.defaultworkingdirectory)/Field Service/Script-Development'
   customCommand: 'install'
- task: Npm@1
  displayName: 'Build TypeScript'
  continueOnError: true
  inputs:
   command: 'custom'
   workingDir: '$(system.defaultworkingdirectory)/Field Service/Script-Development/SourceCode'
   customCommand: 'run dist'
- template: 'CI/Template-PublishWebresources.yml@templates'
  parameters:
    repoPath: '$(Build.SourcesDirectory)/PP-099-PipelineTemplates'
    spklDir: '$(system.defaultworkingdirectory)'
    spklConfigDir: '$(system.defaultworkingdirectory)/Field Service/Script-Development/HelperTools/HelperTools'
    clientId: $(ClientId)
    clientSecret: $(ClientSecret)
    url: $(Url)
```
