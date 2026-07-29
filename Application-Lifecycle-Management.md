
# Application Lifecycle Management (ALM)

This page focuses on the deployment strategy of Power Platform components through solutions.

To be up-to-date, also make note of the Microsoft ALM guidelines for the Power Platform: https://docs.microsoft.com/en-us/power-platform/alm/

## Azure DevOps

For any implementation, Azure DevOps must be set up within the customer's own tenant. These services are required to automate the build and release of the Power Platform solutions.

Within Azure DevOps, at least two "Microsoft hosted" or one "Self hosted" pipeline (with at least 2 parallel jobs) are required. The free pipeline does not suffice due to the 60 minutes limit for a release.

If the customer doesn't use Azure DevOps, there are a couple of options. For more information refer to the Microsoft documentation: https://azure.microsoft.com/nl-nl/services/devops/

## Customer specific solution

An important assumption regarding the contents of this document is that during an implementation we deliver a customer specific solution instead of an App from the marketplace. This customer specific solution is often a solution that is built on top of the core solutions from Microsoft and third-party solutions.

## Power Platform Components this applies to

This document applies to all Power Platform components that can be managed through solutions, including:

- Power Apps (Canvas Apps and Model Driven Apps)
- Power Apps Portals
- Power Automate

## Quality Deployment Guidelines

### Automate everything

**Rule**

Automate as much as possible in your deployment. Aim for 100% automation with no manual steps. There are always exceptions where it is not worth automating something, but these exceptions must be very rare. When it comes to deploying customizations, manual steps are to be avoided. Also, under no circumstance should anyone perform manual customizations on a non-development environment.

**Motivation**

Automating your deployment will result in a repeatable process that eliminates human error. You will guarantee that the result of 2 deployments to different environments will be the same. Hereby you eliminate the issue of bugs appearing on one environment while they don't exist on another.

The motivation regarding never making any manual customizations is because you don't want any unmanaged customizations on a managed environment. Unmanaged customizations always overrule managed customizations and will cause unwanted results and headaches in the future. Fixing those will cost more time than you are going to save by doing unmanaged customizations.

### Deployments should be idempotent

**Rule**

Deployments should be idempotent. This means when running a deployment twice on the same environment, the result must be the same. *(See original documentation for an example image: the non-idempotent version creates a new record every time it is run, causing multiple records in the system; the idempotent version will always result in 1 record.)*


**Motivation**

Similar to the motivation above. The goal is to make your deployments repeatable and have the same result. In addition, you usually deploy to the test environment more frequently than to production and you don't want any differences between the two.

### Your source control is the single source of truth

**Rule**

Deployment packages should be based on the contents (both compiled and non-compiled content) of your source control. This means that any code or customizations that are stored in source control must be leading over what exists in your development environment.

**Motivation**

This way you will avoid the following situation: Person X performs a bugfix on a JavaScript file and doesn't put it in source control. This file is deployed to production. Two months later person Y needs to perform a change. He gets the source code from source control and performs his fix. This goes to production and now the fix that was done two months ago is undone. By always basing it on source control, if someone forgets to commit, then it won't be deployed.

It is also an issue of traceability. If you ever need to troubleshoot something, then you are sure that what is saved in source control is the actual thing that is in production. Finally, you can always look back in source control and find the state of your customizations at any point in time.

### Deployment pipeline determines the version number of your solutions

**Rule**

The version number of your solutions should be determined by the pipeline (usually Azure DevOps). The format of the version number doesn't really matter.

**Motivation**

When you ever need to know what version is on an environment, you just check the solution version, go to DevOps and check the pipeline with the same version number. Pipelines are always connected to a commit in source control and that means you can see the exact state of every component. This is very useful for troubleshooting and answering difficult questions. It also really shows the client that you are in control of what is where.

### Deploy as often as possible

**Rule**

Deploy frequently. The frequency is project specific but try to aim for nightly deployments to test and every sprint to production. Continuous deployment is the ideal goal but sometimes not realistic. Nightly deployments to test are a good and realistic aim for any project.

**Motivation**

The more often you deploy, the faster you can test your changes and the faster you get feedback. This will overall increase the quality. Also, there are fewer differences between environments which is beneficial for troubleshooting.

### Always be able to deploy an environment from scratch

**Rule**

It must be possible to create a new environment and automatically deploy the latest customizations based on source control.

**Motivation**

This forces you to store everything needed on an environment in source control. Next to that, you can quickly provision a new environment which makes your team very agile.

## Efficiency Deployment Guidelines

### Only deploy what is changed

**Rule**

There is no need to deploy changes (usually solutions) that aren't changed.

**Motivation**

Deploying takes time, especially upgrading managed solutions. Only deploying changes will save a lot of time while deploying. This can be built in with steps in your deployment pipeline.

## Security Deployment Guidelines

### Deploy using an Application User

**Rule**

Deployments should be run under an application user (clientid/clientsecret).

**Motivation**

These users are non-interactive and thus improve security as you can't manually log in and perform actions with them. Next to that they have way higher [API limits](https://docs.microsoft.com/en-us/power-platform/admin/api-request-limits-allocations#non-licensed-user-request-limits) compared to regular users.

### Setup approvals for deployment stages

**Rule**

When setting up your stages (test, acceptance, production) in your deployment pipeline, add customer approvers to the stages. This is mandatory for production, recommended for acceptance and optional for test.

**Motivation**

This will ensure that deployments are only done when it's agreed that there is a deployment. It will create involvement by and confidence from the customer. It subsequently also creates traceability that shows everyone agreed with the deployment.

## Other Deployment Guidelines

### Always deploy using Stage for Upgrade

**Rule**

When deploying solutions, always use stage for upgrade. When applying solution upgrades, it has to be done in reverse order of import.

**Motivation**

Stage for upgrade, like the name suggests, imports the solution and stages it for upgrade. It basically means that its changes will be applied (especially important for deletes) when the update is applied. Using this method, you will avoid layering & dependency issues.

### Run automated tests after deployment

**Rule**

When a deployment is finished, run automated tests on the environment to verify the quality of the deployed solution.

**Motivation**

This way you will always verify the deployed solution and you can tell if anything is wrong right after deployment.

### Tag your commits when creating a deployment package

**Rule**

When you create a deployment package, also create a tag on the commit the deployment package is based on.

**Motivation**

If you create a package to deploy and make a tag for this commit, you can always at any point in the future quickly find the correct commit that belonged to a deployment package.
