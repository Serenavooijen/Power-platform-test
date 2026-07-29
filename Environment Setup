# Environment Setup

When setting up a new Power Platform environment (Dataverse) the following guidelines must be followed.

## Environment Naming

### Rule

If you are creating a Power Platform environment, make a plan to name each environment, with a logical rule, and a describable name of the goal to separate this environment from the other environments.

### Motivation

When having a logical rule behind the name of all the environments, they are structured in one overview, and the role of each of the environments will be clear by the name.

### Example

| Environment | Name | Goal |
|---|---|---|
| Development | DEV01 | Unmanaged development environment |
| Development | DEV02 | Second unmanaged development environment for doing Proof of Concept |
| Test | TST01 | Managed test environment |
| UAT/Acceptance | ACC01 | Managed user acceptance test environment |
| Build | BLD01 | Release build environment |
| Configuration | CFG01 | Master/Reference data environment |

### Tips

Each environment can have a different header color in order to easily distinguish the environments from each other, for example:

- Development: Blue
- Test: Red
- Acceptance: Green

## Security groups

### Rule

Each environment must have a security group. This can either be a security group per environment or a group per environment type (e.g. one security group for Development/Test, one for Acceptance, and one for Production).

### Motivation

It helps to prevent certain users from accessing environments even though they have a CE license. The available users to assign security roles to will be limited to the users who are allowed to access that environment by their security group.

It is also sensible to prevent all Global Administrators (or Power Platform Administrators) from getting the System Administrator role when a new environment is set up.

See: https://docs.microsoft.com/en-us/power-platform/admin/control-user-access

### Tips

Users don't need to be members of the environment security group in order to use the Canvas apps that have been shared to them. They won't have any access to the Power Apps Maker portal, and app usage is an altogether separate thing technically.

## Creation of environments

### Rule

You should block the creation of Power Platform environments for everyone.

### Motivation

Every environment takes up at least 1 GB of capacity, which can lead to storage capacity problems in the future. See Microsoft Power Platform settings.

## Solutions

As defined within general best practices, a PowerApps solution may be split into functional/technical components or be defined as a total solution for a customer. Details can be found in Power Platform - Solutions.

A Power Platform solution must always be deployed as a managed solution (see Deploying Managed Solution).

## Environment default language

### Rule

It is recommended to set English as the default language of the environment and install other languages as an additional option.

### Motivation

- English is the original product language of Microsoft Dynamics 365 CE. All system labels and entity names were developed in English.
- Microsoft documentation, community resources, and support cases are primarily available in English.
- Technical entity and field names differ from the English documentation, which causes confusion.
- New features and updates are always released in English first.
- For integrations with other systems, the English base layer is used, ensuring consistency.
- Developers and consultants almost always work with English terminology.
- Not all non-English labels and translations are complete or up to date.

---


