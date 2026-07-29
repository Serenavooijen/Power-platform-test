# Build TypeScript code

TypeScript compilation is natively supported in the CI pipeline via the `typeScriptFolders` parameter. The pipeline will run `npm install` and `npm run build` for each folder you specify.

## Using the native typeScriptFolders parameter

Add the `typeScriptFolders` parameter to your CI pipeline template call. Provide the root folder(s) of your TypeScript project(s) — where you would normally run `npm install` / `npm run build`:

```yaml
- template: 'CI/Main.yml@templates'
  parameters:
    solutions: 
    - '**\\*.sln'
    buildPlatform: ${{ parameters.buildPlatform }}
    buildConfiguration: ${{ parameters.buildConfiguration }}
    javascriptPath: '$(system.defaultworkingdirectory)/src/Webresources/webresources'
    pluginPath: '$(system.defaultworkingdirectory)/src/Plugins/Hso.Dataverse.General.Plugins/bin/${{ parameters.buildConfiguration }}/net462'
    nuGetConfig: '$(system.defaultworkingdirectory)/src/NuGet.config'
    typeScriptFolders: 
    - '$(system.defaultworkingdirectory)/src/Webresources/SourceCode'
```

Set `javascriptPath` to where the compiled JavaScript output lives (not the TypeScript source folder).

## Publishing web resources to the build environment

The Build-Update pipeline can also publish web resources using SPKL. Set both `webResourcesPublishSolution` and (optionally) `webResourcesPublishPath` in the Import stage:

```yaml
# Build-Update.yml — publish web resources in the Import stage
- template: 'UpdateBLD/Main-Import.yml@templates'
  parameters:
    ...
    webResourcesPublishSolution: 'HSO'
    # webResourcesPublishPath defaults to $(Build.ArtifactStagingDirectory)/WebResources
```

For full parameter details see [Setup Build-Update Pipeline](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/HSOExtension/Templates/setup-build-update-pipeline.html).
