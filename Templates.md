# Templates

We have developed some deployment templates to use within your projects. They follow the best practices and give you a good basis to implement your deployment process. It will fit the needs for most projects and has several extension points.

The templates are in the [OneHSO DevOps organization](https://dev.azure.com/OneHSO/HSO%20Best%20Practices) in the PP-099-PipelineTemplates repository. The templates and examples are in different branches according to the structure below. A certain version of the examples always uses that same version of the templates. It's recommended to always use the latest version available.

*(Image: branch/version structure diagram)*

### Power Platform - Advanced Project Booster

The deployment best practices (Azure DevOps Extension and DevOps Pipeline Templates) are part of the "Advanced Project Booster" [Development Accelerator](https://docs.hso.com/DevelopmentAccelerators/).

## Examples

[This](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=%2F&version=GBexamples%2Fv1&_a=contents) is basically an example setup of your repository. If you are starting new, then you should copy all contents of this repository into your repository and work from there. It contains the pipelines and folder structure that matches the pipelines. You can modify everything here to suit your project's needs.

## Templates

[These](https://dev.azure.com/OneHSO/HSO%20Best%20Practices/_git/PP-099-PipelineTemplates?path=%2F&version=GBreleases%2Fv1&_a=contents) are the actual templates that are used and they are named either 'Main', 'Main-xxxx' or 'Template-xxxx'. The 'Main' and 'Main-xxxx' are meant to be used from your pipelines, but the 'Template-xxxx' are for internal use. When the templates are updated, there won't be any breaking changes on 'Main' or 'Main-xxxx'. There may be breaking changes on the 'Template-xxxx' files. In case of a breaking change on 'Main' or 'Main-xxxx' a new version is created.

## Deployment Process

The templates contain a total of 5 pipelines. 3 of them are each responsible for a part of the deployment process and 2 are supporting pipelines. Depending on if you use a build environment, the process is a little different.

### Source control is the single source of truth

Source control (git) is the single source of truth. Everything that is deployed is done based on what is in source control. Doing this creates a history of everything that you do. You are completely in control. You know what is changed in every deployment, you can see history of components and you can create your deployment package of any version ever deployed. It doesn't happen very often you need this, but when you do it really helps.

### Process without Build environment

*(Image: process diagram without a build environment)*

1. The export pipeline exports the solutions (both managed and unmanaged) and reference data from the development environment. Reference data is optional, so that can be skipped if your project doesn't have any reference data.
2. After everything is exported, the solutions are extracted using the solution packages and everything is committed into source control.
3. The deployment pipeline creates a package out of source control that contains everything that is needed to deploy to an environment. This includes solutions, reference data, configuration files, automated testing and more.
4. The package is deployed to each environment (steps 4, 5, 6).

### Process with a Build environment

*(Image: process diagram with a build environment)*

1. Sprint solutions are exported from the development environment.
2. The sprint solutions are committed to source control. They are placed into a separate repository as it's only for traceability purposes.
3. The sprint solutions are imported into the build environment.
4. The sprint solutions are split into the different component solutions.
5. The export pipeline exports the solutions (both managed and unmanaged) and reference data from the build environment. Reference data is optional, so that can be skipped if your project doesn't have any reference data.
6. From here it is basically the same as without a build environment. After everything is exported, the solutions are extracted using the solution packages and everything is committed into source control.
7. The deployment pipeline creates a package out of source control that contains everything that is needed to deploy to an environment. This includes solutions, reference data, configuration files, automated testing and more.
8. The package is deployed to each environment (steps 8, 9, 10).

## Pipelines in detail

### CI Pipeline

This is a supporting pipeline that will run on a commit. It compiles your source code and runs unit tests. It's recommended to run this pipeline as part of pull request validation.

### Generate Early Bound

This pipeline is to generate early bound classes for your solution. As everybody always does it differently, this is a simple and consistent way of doing it. It will check in the updated models into the branch you run it on. If you don't have C# code, you don't need this pipeline.

### Build-Update

This pipeline is for moving your sprint solutions from Development to Build. You don't need this pipeline if you don't use a build environment.

*(Image: Build-Update pipeline diagram)*

### Export

This pipeline is to export your solutions & reference data and check them into source control.

*(Image: Export pipeline diagram)*

This pipeline has variables to control whether solutions and/or reference data must be exported. If, for example, your project doesn't use reference data, set this parameter to 'false' but keep the reference data stage. This way it's easy to add reference data whenever your project needs it in the future.

Setting the solution export to 'false' is useful if you want to publish an update of reference data, but don't want to update solutions. Simply create a new branch based on your production branch, run the export pipeline with only reference data and then the deploy pipeline. This will then deploy the reference data and run automated tests to verify everything still works. On a positive test run you can then safely deploy to production.

### Deploy

This pipeline is for deploying your solutions to test, acceptance and production (and any other environments you may have).

*(Image: Deploy pipeline diagram)*

#### Prepare Package

This step just copies files from your CI build and source control into a package with a specific structure. The purpose is that the actual deployment steps always use a set folder structure, even if your project structures git differently than default. Secondarily, you can always view an artifact in DevOps to see the contents of the package that is deployed.

#### Run Custom Steps

In the flowchart you see 'Run Custom Steps' multiple times. These are extension points you can put your own project specific steps. Many pipelines have extension points to inject additional steps. If there are additional extension points needed, just suggest an improvement using the button to the right.

#### Pre Deploy

Here you can run some conversions and cleanups that must run before solution deployment.

#### Third Party Solutions

Here you can import/apply any third party solutions.

#### Import Solutions

Here your solutions are imported. Before every solution import, it will check for running solution imports. This is done in case Microsoft runs solution imports. It will wait until 5 minutes after the last solution export. It won't wait if the last import was from yourself.

#### Apply Solutions

Same as importing solutions, but then for applying solutions. Make sure the order of applying is reversed (so starting with the last imported solution).

#### Delete Solutions

Here you can delete any solutions that are no longer needed.

#### Post Deployment

Here you can run some conversions and cleanups that must be run after solution deployment. Next to that, your connection references are connected, your Power Automates activated and system settings are set.

#### Reference Data

Here your reference data will be imported.

#### Automated Testing

Here your SpecFlow tests are run.

## Getting Started

To get started, copy the contents of the example into your repository. Or parts of it, if you already have an existing project. Next, follow the setup instructions for the version you are using.
