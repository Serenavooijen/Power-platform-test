# DLP policies

Data loss prevention (DLP) policies act as guardrails to help prevent users from unintentionally exposing organizational data and to protect information security in the tenant. DLP policies enforce rules for which connectors are enabled for each environment, and which connectors can be used together. More information on [establishing a DLP strategy is available via Microsoft](https://docs.microsoft.com/en-us/power-platform/guidance/adoption/dlp-strategy).

## Goal for the landing zone

The DLP policies are setup in such a way that data cannot be unintentionally shared with other systems.

## Data policies

For the landing zone, the following DLP policies are setup.

### Platform Policy

A policy which applies to all environments in the platform:

- Name: 'Platform Policy'
- Default Group for Connectors: 'Blocked'
- Block all connectors which can be blocked
- All other connectors should stay in 'Non-business' for now.
- No settings for [custom connectors](https://learn.microsoft.com/nl-nl/power-platform/admin/dlp-custom-connector-parity)
- Scope: All environments

### Personal productivity

A policy which applies to the personal productivity environment:

- Name: 'Maker Policy'
- Default Group for Connectors: 'Non Business'
- Mark no connectors
- Scope: Default (personal productivity) environment

### Maker policy

A policy which applies to all maker environments in the platform:

- Name: 'Maker Policy'
- Default Group for Connectors: 'Non Business'
- Mark the Dataverse connector as 'Business'
- Scope: Specific environments used for making specific apps

### Acceptance and production environment policy

A policy which applies to all acceptance and production environments:

- Name: 'Production Policy'
- Default Group for Connectors: Non Business (this is ok, due to the Platform policy)
- Mark all used connectors as business
- Configure connectors to only allow the actions required (this is applicable for some connectors)
- Scope: Specific environments

The following is not in scope of the basic setup, but important to consider: when, at some moment in time, there are multiple production environments which use different connectors, then policies should be split for these production environments.
