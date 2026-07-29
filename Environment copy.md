# Environment Copy

When copying a Power Platform environment multiple aspects should be taken into account.

## How to copy when F&O environments are used

Please refer to [this guide](https://docs.hso.com/Dynamics365CrossPlatform/OnePlatform/implementation.html#environment-refresh-scenarios) when you want to copy an environment and F&O is used.

## How to copy when Customer Insights - Data is used

Please refer to [this guide](https://docs.hso.com/Customer-Engagement/CustomerInsights-Data/cidatasetup.html) when Customer Insights - Data is used.

[Copy Power Platform Environments](https://docs.hso.com/PowerPlatform/Governance/EnvironmentStrategy/environment-copy.html#copy-power-platform-environments)

## Rule

[Rule](https://docs.hso.com/PowerPlatform/Governance/EnvironmentStrategy/environment-copy.html#rule)

- A copy must not be made to a development environment.
- A live environment may be copied to an Acceptance or Test environment to guarantee a deployment; however, this might cause conflicts and should therefore be executed carefully.
- Make sure to anonymize data, change system configuration (to external URLs) to comply with GDPR.
- The customer should be responsible for production copies or else the copies and anonymization should be automated through Azure DevOps.

## Motivation

[Motivation](https://docs.hso.com/PowerPlatform/Governance/EnvironmentStrategy/environment-copy.html#motivation)

You must always have a working Acceptance or Test environment to perform testing. Given that solutions are deployed "managed", you cannot perform any customizations when copying to the development environment.

For a full description on how to create a back-up, and what guidelines there are, see "[Copy Production Environment](https://docs.hso.com/PowerPlatform/Governance/EnvironmentStrategy/environment-copy.html#copy-production-environment)".

## Example

[Example](https://docs.hso.com/PowerPlatform/Governance/EnvironmentStrategy/environment-copy.html#example)

A CE related example: ClickDimensions and Dynamics Portals. Both use specific environment configuration and therefore do not work when performing a copy, so additional steps are required. In addition, if integrations with other applications are applicable, refreshing the UAT environment might result in inconsistencies with other applications.

## Copy Production Environment

[Copy Production Environment](https://docs.hso.com/PowerPlatform/Governance/EnvironmentStrategy/environment-copy.html#copy-production-environment)

When creating a copy of production to another environment, the customer needs to be compliant to local laws and regulations, such as GDPR.

An advice to implement this process can be given to the customer. Make sure the customer (and project manager) are notified when this process is set in place. The customer is always responsible for its own data.

Before starting to make a copy, investigate the impact on other databases that are connected by integrations.

Whenever a copy is made from production, make sure all the data on the target environment is anonymized, the audit logs are cleared after anonymization, the settings are changed, and that the customer and project manager have signed for approval. It is advisable to automate this process.

### Tips

HSO Innovation delivers two solutions that can be used for Data Protection and bulk anonymization or randomization of personal sensitive data:

- [Dynamics Data Masking](https://innovation-product-documentation.azurewebsites.net/DMA.html): Can be used for non-production environments only. It is a technical tool to support bulk anonymizations and randomization of personal sensitive data in, for example, a TST environment. This solution is available on [AppSource](https://appsource.microsoft.com/en-us/product/dynamics-365/dynamics_software.dynamicsdatamasking?tab=Overview).
- [Dynamics Data Protection](https://innovation-product-documentation.azurewebsites.net/DPR.html): Can be used in a production environment. It is a business application to support the processes of a Data Protection Officer, such as creating Data Consents, recording Data Policies, following up the "right to be forgotten", anonymization of personal data on request, and much more. This solution is available on [AppSource](https://appsource.microsoft.com/en-us/product/dynamics-365/dynamicsgdpr?tab=Overview).

### Steps to execute

1. Navigate to the admin center and select the production environment.
2. Select 'create copy' and fill out the form (target environment, name, etc.).
3. Make sure that the right security group is selected.
4. Start the copy and wait till it's complete.
5. Log in to the environment with the System Administrator or System Customizer role and review all necessary components and records (see: 'What to review').
6. Change configuration data where needed (see: 'Configuration changes').
7. Scramble or delete all personally identifiable data (see: 'What to scramble').
8. Remove all audit logs (Settings -> Auditing -> Audit Log Management).
9. Disable maintenance mode (admin center).
10. Give the necessary users access.

### What to review

Some records need to be disabled or changed to prevent e-mail from being sent, Outlook records from being synced, or production APIs from being called.

For a full list of items that can/should be disabled, see: https://docs.microsoft.com/en-us/power-platform/admin/copy-environment#next-steps-after-copying-an-environment

Next to that, please also check components like:

- Service Endpoints
- Environment Variables
- Power Automate Connections
- Logic App Connections
- etc.

### Configuration changes

When there are configuration records that contain environment specific data, this data needs to be updated. This can be done afterwards (when the backup is done) by an automated task through Azure DevOps.

With Configuration/Environment Variables, records to think about can be: link to SharePoint, link to production API, username/password for external APIs, e-mail addresses for outgoing e-mail, etc.

See for the best practice regarding Configuration data: PP-014 Reference Data Management.

### What to scramble

Scramble all personal data according to GDPR:

- https://gdpr-info.eu/art-4-gdpr/ (art. 4 paragraph 1)
- https://www.itgovernance.eu/blog/en/the-gdpr-what-exactly-is-personal-data

Also scramble all data that is crucial for the organization. This is data that can harm the organization when it is out in the public (e.g. sales figures, customers, etc.).

### Tips

To automate the steps above, think about creating a copy environment Azure Pipeline by using the task from the HSO Azure DevOps Build Tools task "[Copy Environment](https://docs.hso.com/PowerPlatform/Governance/DeploymentStrategy/Extension/)" (the documentation is not ready yet, but the task is part of the Extension).
