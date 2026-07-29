# Landing Zone

When setting up the landing zone we assume there is a center of excellence available in the organization to govern the [landing zone of the Power Platform](https://docs.hso.com/PowerPlatform/Governance/). This center of excellence can either be the customer's own IT department, HSO or the customer's IT partner.

## Environment strategy

Define the [environment strategy](https://docs.hso.com/PowerPlatform/Governance/EnvironmentStrategy/) as part of the landing zone.

### Maturity model

Take into account the maturity model to define the right strategy.

### Rename default Power Platform environment

Rename the Default created environment to 'Personal Productivity', so it matches the purpose of the environment.

### Setup the required environments

Set up the required environments as part of the landing zone.

## Azure AD Configuration

Power Platform requires Azure AD. For instructions on setting up Azure AD, refer to our [Azure Cloud Foundation Landing Zone](https://docs.hso.com/Azure/CloudFoundation/AzureLandingZones/).

This article only covers additional configuration related to the Power Platform landing zone.

### Power Platform Licenses

Licenses for the Power Platform are managed via Azure AD (except for App Passes). The licenses which are managed via Azure AD must be managed according to the policies set within the organization.

### Environment Groups

As defined in the [environment strategy](https://docs.hso.com/PowerPlatform/Governance/EnvironmentStrategy/), an Azure AD Security Group must be created per environment.

Note that, for the default environment, this cannot be set.

### Power Platform Administrator roles

To manage the Power Platform, [roles within Azure AD](https://learn.microsoft.com/en-us/power-platform/admin/use-service-admin-role-manage-tenant) are required.

- Only the Azure AD administrator may have Microsoft 365 Global admin.
- Only the Power Platform Administrators may have Power Platform admin.
- Only Dynamics 365 admins may have Dynamics 365 admin.
- Only the Power BI admin may have Power BI admin.
- Developers and Users may not have any of the Azure AD roles.

### Power Platform service account

When running background processes, such as [Power Automate](https://docs.hso.com/PowerPlatform/PowerAutomate/), a licensed service account is required.

Note that this account must not be used for [deployment](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/) and the [center of excellence starter kit](https://docs.hso.com/PowerPlatform/Governance/CenterOfExcellence/).

### Power Platform Tenant settings

As part of the landing zone, the following default [tenant settings](https://learn.microsoft.com/en-us/power-platform/admin/tenant-settings) must be configured within the Power Platform admin center.

- [Environment assignments](https://learn.microsoft.com/en-us/power-platform/admin/control-environment-creation):
  - Production: Only specific admins
  - Trial: Only specific admins
  - Developer: Only specific admins
- Capacity assignments: Only specific admins
- [Tenant level analytics](https://learn.microsoft.com/en-us/power-platform/admin/tenant-level-analytics#how-do-i-enable-tenant-level-analytics): Enabled
- [Boost conversations for Power Virtual Agents](https://learn.microsoft.com/en-us/power-virtual-agents/nlu-boost-conversations): Disabled
- [Copilot](https://learn.microsoft.com/en-us/power-apps/maker/canvas-apps/ai-overview): Enabled

### Automatic license assignment opt-out

Microsoft automatically assigns a security role when a user is assigned a license; this may give a user more authorization than intended within a specific app. To mitigate security incidents, [opt out for automatic license-based user role management](https://learn.microsoft.com/en-us/power-platform/admin/opt-out-automatic-license).

### Tenant isolation

To restrict access from other tenants, perform the following setup:

- Enable the setting for '[Cross-tenant inbound and outbound restrictions via tenant isolation](https://learn.microsoft.com/en-us/power-platform/admin/cross-tenant-restrictions)'.

## Basic Data Loss Prevention (DLP) Policies

Setup the [data loss prevention (DLP) policies](https://docs.hso.com/PowerPlatform/Governance/PoliciesGuidelines/dlp.html) as part of the landing zone.

## Solution Strategy

Define the [solution strategy](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/SolutionStrategy/) as part of the landing zone.

### Setup the required solution(s)

Set up the required solution(s) and solution publisher(s) as part of the landing zone.

## Deployment Strategy

Define the [deployment strategy](https://docs.hso.com/PowerPlatform/Governance/ApplicationLifecycle/) as part of the landing zone.

### Configure deployment

Configure the deployment according to the strategy.
