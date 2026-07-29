# Center of Excellence (CoE)

Microsoft gives a clear description about the purpose of the Power Platform Center of Excellence (CoE) Starter Kit:

> **Power Platform Center of Excellence (CoE) Starter Kit**
>
> The Microsoft Power Platform Center of Excellence (CoE) Starter Kit is a collection of components and tools that are designed to help you get started with developing a strategy for adopting and supporting Microsoft Power Platform, with a focus on Power Apps, Power Automate, and Power Virtual Agents.

The following resources are relevant to implement this starter kit:

- Documentation is found on Microsoft Learn.
- Details on development of the solution are found on GitHub.

## Monitoring services

The purpose of the CoE Starter Kit might suggest that it may be applicable for monitoring the health of a solution. This is not the case, given the focus on monitoring app development and not the health within an app. At HSO we use a different solution for monitoring, also because we offer monitoring services as part of our global platform services and have chosen one architecture for all our solutions.

More information on monitoring the health of the Power Platform is found here.

## Features in the CoE Starter Kit

The features in the CoE Starter Kit allow you to govern your Power Platform environments and app creation processes. This topic contains important items regarding the features of the toolkit.

### CoE Starter Kit Modules

Details about all modules are listed on Microsoft documentation; a short description of the building blocks and the advice on using these is added below.

There are the following building blocks — some are positioned as standalone add-ons and some are listed as part of separate installation instructions:

| Component | Purpose | Install this module |
|---|---|---|
| Core | Core solution for CoE Starter Kit. Contains the administrative apps and processes to sync required data | Yes |
| Governance | Audit and compliancy processes for managing apps | Yes |
| Power BI reports | Power BI Reports | Yes |
| Audit Log Data | Process to sync Microsoft 365 audit log telemetry | Depends on requirement |
| Nurture Components | Sharing Best Practices and Creating a Community | No, refer to Community |
| Administration Planner | Resource and task planning | No, this type of planning is usually part of 'Microsoft 365', 'Planning solutions' or 'Azure DevOps' |
| Communication site template | Communication site | No, refer to Community |
| Theming | Share themes for canvas apps | No, bypasses default use of canvas apps |
| Application lifecycle management | Application lifecycle management | No, these processes are usually in Azure DevOps |
| Innovation Backlog | Backlog management | No, these processes are usually in Azure DevOps or similar tools |

## Setup the CoE Starter Kit

Installation of the CoE Starter Kit is a sensitive task which will require multiple steps. Microsoft has detailed documentation on installing this CoE Starter Kit, including requirements for your environment.

This article lists important prerequisites and actions that require attention during setup.

### Prerequisites

The following prerequisites are applicable:

- An admin service account for the CoE Starter Kit (for details refer to Microsoft):
  - Used for running all the processes.
  - The Microsoft Power Platform Service admin role should be sufficient.
  - Power BI Premium is only required if using the telemetry export; this is still a preview feature.
- Three Azure AD Security Groups for 'Admins', 'Makers' and 'Users':
  - Used for communication by the system with admins/makers/users.
  - The service account should be owner, so users can be managed.
- An app registration for Microsoft Graph (for details refer to Microsoft):
  - Used to see Microsoft 365 Messages in the command center app.
- When installing the audit log components, additional requirements are applicable. These requirements apply to Microsoft 365.

### Create the environment and install the CoE Starter Kit

Use a separate 'Center of Excellence' environment for the CoE Starter Kit, following your regular naming conventions.

Microsoft has detailed documentation on installing this CoE Starter Kit, including requirements for your environment.

When installing the CoE Starter Kit, multiple actions need to be taken. All of these actions are listed on the Microsoft documentation. Assuming these will change, there are no details in this wiki page.

**Tips:** Only install the components that are required — refer to the 'CoE Starter Kit Modules' section on this page.

## Using the CoE Starter Kit

From the administrator perspective, multiple apps are available and all should be used to support specific aspects of the governance of the platform. From a maker perspective, certain apps within the CoE Starter Kit also need to be used to support the process and maintain governance.

Also keep in mind that there are background processes which might clean up environments or send notifications, based on the settings provided and installed components.

### Configure the following components

In most situations, the following processes should be setup within the CoE Starter Kit:

- Process to request new environment.
- Set up capacity soft limit for each environment.

This setup is on top of the basic setup and installation of the CoE Starter Kit (as described above).

When the Power Platform is already running for a long period of time, then additional actions may be required. These would be actions such as:

- Validating DLP policies using the 'DLP impact analysis app'.

> **Warning:** For now, there are no details on using each app. Each app and its purpose will probably be published on Consulting Source in the future.

## Considerations while implementing the CoE Starter Kit

From this point on, this article assumes that a well-informed decision is made regarding the potential need for implementing the CoE Starter Kit, and that a center of excellence will monitor this (and other) tool(s) when available.

### Implement this from the start

Usually it is a logical choice to setup the CoE Starter Kit before any other environments are setup. In practice this will almost never be the case.

The downside of this is that not all historical data will be available in the CoE environment.

### Impact on the database capacity and Power Platform limits

The processes to gather the required data for the CoE Starter Kit currently run using Power Automate Flows, therefore this will impact the following limits of the platform:

- Power Automate Request Limits
- Power Platform API Limits
- Database capacity limits

To mitigate the impact on the platform, it is advised to run the CoE solutions via a dedicated service account, so the limits are not shared with any other service account.

### Feature on roadmap to use Data Lake

In the future, Microsoft intends to change the way data is gathered with the CoE Starter Kit solution by using the data lake export feature.

Currently, this export does not offer all required data and therefore this feature is not completed.

With this feature, the impact on Power Platform limits and database capacity would be reduced.

For more information, refer to GitHub.

## CoE Starter Kit Application Life

After installation, regular maintenance is required for the Starter Kit.

### Updating the CoE Starter Kit

Microsoft recommends updating the CoE Starter Kit at least every three months.

The CoE Starter Kit must be kept up to date; this is not done automatically. Based on deployment best practices, this must be automated using (Azure DevOps) deployment pipelines.

### Extending the CoE Starter Kit

Currently, we advise against making any modifications to the CoE Starter Kit, primarily because:

- Modification might have an impact on future releases.
- Modification will not be simple due to the complexity of the solutions.
- Modifications may conflict, be overwritten, or behave unexpectedly when the solutions provided by Microsoft are updated.

Nevertheless, Microsoft provides documentation on extending the CoE Starter Kit if this is needed.

Note that previously we suggested to 'disable flows' and 'match the solution to your needs'. Our statement has changed.
