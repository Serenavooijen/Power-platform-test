# Setup Deploy Pipeline

This is the pipeline that deploys your solution to managed environments. It consists of a main pipeline which defines to which environments there will be a deployment and a template which defines what happens per environment.

## Setting up the deploy template

We start with the [template](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/Templates/Deploy.yml).

### Prepare package

This stage creates the build package (artifact) that is used in all stages after that.

**CIBuildDefinition** — This is the build definition ID of the CI pipeline you already setup. This default value from the variable group should always be correct.

**connectionString** — Connection string to Dataverse. Keep to default unless you have a good reason to change it.

**configPath** — Path to the folder that contains all [configuration files](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/config&version=GBexamples/v1).

**deployMissingPackages** — Set to true if you want to automatically deploy missing packages when a solution import fails because of missing dependencies.

**environmentVariablesFileName** — Path to the [configuration file](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/config/EnvironmentVariableConfig.json&version=GBexamples/v1&_a=contents) that contains environment variables.

**referenceDataPath** — Path to the folder where all reference data was exported to.

**solutionsPath** — Path to the folder where the solutions were exported to.

**solutionsToPack** — List of all the solutions that will be used in the deployment stages.

**thirdPartySolutionPath** — Path to the folder that contains all third party solution zip files.

**solutionArtifact** — Name of the build artifact. The name doesn't matter, as long as it's the same name as the one used in the deployment stages.

### Deployment stages

For each environment supplied by the main pipeline a stage will be created. Only modify the inputs where needed.

**connectionString** — Connection string to Dataverse. Keep to default unless you have a good reason to change it.

**solutionArtifact** — Name of the build artifact. The name doesn't matter, as long as it's the same name as the one used in the prepare package stage.

**syncTimeout & asyncTimeout** — Timeout values for API calls to Dataverse. Keep to default unless you have a good reason to change it. The asyncTimeout may need to be increased if you have really big solutions.

**environment** — Name of the environment. Supplied by main pipeline. Do not modify.

**powerAutomateOwner** — Owner of the Power Automate connections. This must be an interactive user.

**runAutomatedTests** — Whether to run the automated SpecFlow tests. Supplied by main pipeline. Do not modify.

**powerAutomateSolution** — Unique name of the solution that contains your Power Platform components. Same as the one used in the export stage.

**publisher** — Unique name of the publisher of all solutions being imported.

**solutionsToImport** — A list of solutions that must be imported with stage for upgrade. They are imported in the order they are supplied.

**solutionsToApply** — A list of solutions that must be applied after they are staged for upgrade. This is the same list as the solutionsToImport in the opposite order.

**autoDetectImportType** — A boolean value that determines if the import type (Update, Upgrade or Stage for upgrade) must be automatically detected. Default is false.

**deleteSolutionSteps, postDeploymentSteps, preDeploymentSteps, referenceDataSteps, thirdPartySolutionSteps** — Optional extensions of the deployment pipeline depending on project requirements. These are lists of steps.

**extraDeployEnvironmentJobs** — Optional extensions of the deployment pipeline, similar to above but contain complete jobs.

**dependencyInfo** — This can be used if you want to change the dependency graph of the different jobs that run within the stage. Below are the default dependencies. This can be used when you, for example, have an additional job that you want to place in the middle.

```yaml
 - AutomatedTests: 'ReferenceData'
   ApplySolutions: 'ImportSolutions'
   DeleteSolutions: 'ApplySolutions'
   ImportSolutions: 'ThirdPartySolutions'
   ThirdPartySolutions: 'PreDeploy' 
   PreDeploy: ''
   PostDeploy: 'DeleteSolutions'
   ReferenceData: 'PostDeploy'
```

## Setting up the main pipeline

As a basis, the [Examples](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=/build/Pipelines/Deploy.yml&version=GBexamples/v1&_a=contents) yaml file should be used.

You need to add a section like below for each environment you want to deploy to.

```yaml
- name: 'Dataverse_Test'
  dependsOn: 
  - PreparePackage
  displayName: 'Test'
  runTests: false
```

**name** — This must match the environments you setup in ADO.

**dependsOn** — Here you define on which stage this stage depends. You can add multiple if you want. Use 'PreparePackage' if you want it to start directly after the Prepare Package stage.

**displayName** — Display name to show for the stage in ADO while running the pipeline.

**runTests** — Whether you want to run the SpecFlow tests. If you have SpecFlow, typically only the test environment has this set to true.

## Automating the trigger

Best practice is to change the 'trigger: none' to automatically trigger this pipeline when there is a change in the solutions folder. This means the pipeline automatically triggers after an export of the solutions. You can do this by changing the trigger to:

```yaml
trigger:
  batch: false
  branches:  
    include: 
    - main 
  paths:  
    include:
    - solutions
```

In case you name the default branch something else than 'main', you need to change that. In case you exported your solution to a different folder path than 'solutions', you need to change that into the path the solutions are exported to.
