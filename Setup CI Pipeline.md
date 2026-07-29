# Setup CI Pipeline

This is the pipeline that runs on a commit. The purpose is to compile and unit test all components. It should also be used as a policy on the repository to verify pull requests.

## Why is this mandatory?

In the v1 templates this part is mandatory as it was assumed there is always at least some compiled components (TypeScript, Plug-ins, PCF, PowerShell, etc). We noticed that mainly in the maker space this is not true. In the v2 version the aim is to make this pipeline optional.

## Setting up the pipeline

As a basis, the [Examples](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/CI.yml) yaml file should be used. All variables have a default value that is aimed to be correct in the majority of cases. Verify all parameters and supply a different value if required. The trigger is setup to trigger on all commits.

**solutions** — This variable defines all the solutions that should be compiled. The default is 'every solution in the git repo'. If there are none, nothing will be built.

**buildPlatform & buildConfiguration** — Platform and configuration to be used during compilation. The default is correct in 99% of the cases. Unless the TA has a good reason to deviate from this, leave this as is.

**configPath** — Path to where all the [configuration files](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/config&version=GBexamples/v1&_a=contents) are located. This one is needed in all Dataverse implementations in some way.

**javascriptPath** — Folder path to where the JavaScript files are located. Check the form if this is needed. This is practically always needed for biz apps implementations. Always use the [TypeScript](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-017-Script-Development) template. Point this to where the generated JavaScript files are, not the TypeScript files. Use `typeScriptFolders` (see below) to compile TypeScript as part of the CI pipeline.

**typeScriptFolders** — List of root folder paths where TypeScript source code is located. For each folder the pipeline will run `npm install` and `npm run build`. This is the recommended way to compile TypeScript in the CI pipeline. Leave empty (or omit) if you don't have TypeScript. See [build-typescript.md](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/HSOExtension/Templates/build-typescript.html) for more details and examples.

**nuGetConfig** — Path to the NuGet.config file path in case you are using custom NuGet feeds (like the HSO public feed). If the form requires plug-ins and the [template](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-016-Plugin-Development?path=/NuGet.config) is used, you need to refer to this file. Usually placed in the 'src' folder.

**pluginPath** — Folder path to the plug-ins DLL. Use the form response to determine if you need this. Always use the [PP-016 template](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-016-Plugin-Development) for this. After implementing the plug-in template, set the folder path correctly.

**powershellCmdletPath** — Folder path where the custom PowerShell Cmdlets DLL resides. Leave empty if there is no such project. Most implementations don't need this.

**powershellScriptPath** — Path where the custom PowerShell scripts are placed that are used in the deployment. Set to an empty string if you don't want this. It's advised to have [the folder with a placeholder md file](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Scripts/Scripts.md&version=GBexamples/v1&_a=contents) setup so you can just put scripts there and have them included whenever the first script is needed. If you do that, you can just place the scripts and it won't be needed to update the pipeline anymore.

**specflowPath** — Folder path to the automated testing DLL. Check the form if you need this. Leave empty if you don't use automated testing or don't use SpecFlow.

**testAssemblies** — Filter criteria for unit tests. Default will be good for most implementations. Review the default value and update it if needed.

**preCopySteps & postCopySteps** — You can add additional steps in the pipeline just before and after the copying of files. The best way to do that is to create extra yaml files and call those. The path is a relative path to the template file from the location of this yaml file. Templates can have parameters. The template needs to have 1 or more steps. A simple example can be found [here](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/HSOExtension/Templates/build-typescript.html).
