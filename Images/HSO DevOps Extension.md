# HSO DevOps Extension

Deployment for Dataverse is an important topic in the development process. It is important to do this in a structured way. To structure this a set of rules are described. These rules form the basis of the technical implementation.

## Prerequisites

To do proper deployment, you need to have your [environments](https://docs.hso.com/PowerPlatform/Governance/EnvironmentStrategy/) and [solutions](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/SolutionStrategy/) setup.

Additionally, there are some prerequisites at the Microsoft Entra/DevOps Organization level:

- A shared (service) account for executing central administrative tasks such as installing D365 Apps and creating connections for Power Automates in the Power Platform environment. This account should ideally have the Power Platform Administrator role managed by Privileged Identity Management (PIM) and must be secured with MFA.
- An application user for executing deployment pipelines. If environment copying needs to be performed by an automated pipeline (which is strongly advised), the deployment app registration permissions must be granted. Instructions can be found [here](https://learn.microsoft.com/en-us/power-platform/admin/powershell-create-service-principal#registering-an-admin-management-application).
- The ability to create environments, Git repositories, Azure Pipelines, and Power Platform service connections in DevOps.
- Required DevOps Extensions (to be installed by the client):
  - [Power DevOps Tools](https://marketplace.visualstudio.com/items?itemName=WaelHamze.xrm-ci-framework-build-tasks&targetId=b55ecc30-ec74-4030-8603-802cc6a4e340&utm_source=vstsproduct&utm_medium=ExtHubManageList)
  - [HSO D365 CE Pipeline Tools](https://docs.hso.com/PowerPlatform/Setup/DeploymentStrategy/Extension/#how-to-get-it)

### Power Platform - Advanced Project Booster

The deployment best practices (Azure DevOps Extension and DevOps Pipeline Templates) are part of the "Advanced Project Booster" [Development Accelerator](https://docs.hso.com/DevelopmentAccelerators/).

## Topics

The following topics are handled in the best practices:

- [Ruleset](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/)
- [Azure DevOps Extension](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/HSOExtension/Extension/)
- [Azure DevOps Pipeline Templates](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/HSOExtension/Templates/)
- [Power Apps Portals Deployment](https://docs.hso.com/PowerPlatform/PowerPages/deployment.html)
