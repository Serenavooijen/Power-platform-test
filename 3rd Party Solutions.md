# 3rd Party Solutions

This document will describe the guidelines regarding using 3rd party solutions on the Power Platform. When applicable, detailed documents are referenced.

The main focus will be: what 3rd party solutions are allowed, what 3rd party solutions are not allowed, and what to check before using new 3rd party solutions.

## Working with 3rd party solutions

### Rule

Using third-party solutions for Dynamics 365 is not allowed, unless these are on the pass list of solutions in appendix A. This also includes open source solutions.

When open source components are used, we must comply with the license agreement for that solution. In addition, we must clone the used source code into a general HSO repository.

### Motivation

HSO is responsible for the maintenance of the complete solution. If we cannot modify the source code, we cannot guarantee that the solution will be supported in the future. In addition, we must also comply with license agreements for third party solutions.

## Using 3rd party webservices

### Rule

Using third-party public webservices for Power Platform is not allowed, unless these are on the pass list of solutions in chapter 3.

### Motivation

HSO is responsible for the maintenance of the complete solution, therefore we check if the third party is reliable before we allow this to be used within our applications.

### Example

For VAT number validation we should use the public services provided by the EU.

## Using 3rd party solutions not on the pass list

### Rule

When you want to use a component that isn't on the pass list, you must contact someone to validate if the solution may be used. If the solution may be used, it will be added to the pass list.

Before a solution will be added, it needs to comply with the following requirements:

- The source code must be available and/or maintained by a paid vendor (with continued support).
- Server-side code can be compiled to the latest Dynamics 365 SDK without modifications.
- Client-side code complies with the Dynamics 365 SDK and doesn't use deprecated features.
- HSO Support agrees to maintain this new third-party solution.

| Country | Person |
|---|---|
| DE | Andy Schwarz |
| IND | ?? |
| International (INT) | Guillermo Luengo |
| Innovation (NN) | Albert Ritmeester |
| NL | Erik Pellegrom / Martijn Vermaat |
| UK | Jack McNay |
| USA | ?? |

### Motivation

This rule is required to ensure that only approved solutions that HSO can support appear on the pass list.

## Pass / Block List

The pass list can be different per country. Every country determines which solutions are approved for usage and can be supported from within that country. The below list shows all known 3rd party solutions that are approved per country. Please take note that some solutions can be approved in one country, but can be on the block list of another country. This is a matter of experience and supportability per country and can have all kinds of reasons.

Legend:
- Empty means the solution hasn't been decided upon yet, and you should contact your local lead (see section above).
- ✅ (green checkmark) means the solution is approved.
- ❌ (red cross) means the solution is not allowed.
- ❓ (orange question mark) means you have to contact your local lead (see section above).

### Solutions

The following solutions may be used.

| Publisher | Solution(s) | Notes | DE | IND | INT | NL | UK | USA | NN |
|---|---|---|---|---|---|---|---|---|---|
| Microsoft | Any Microsoft Solution | | | | | ✅ | | | ✅ |
| HSO Innovation | Dynamics Advanced Field Service | | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| HSO Innovation | Dynamics Customer Engagement Essentials | Previously known as core essentials | | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| HSO Innovation | Dynamics Customer Location | | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| HSO Innovation | Dynamics Data Protection | | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| HSO Innovation | Dynamics Data Masking | | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| HSO Innovation | Dynamics Document Manager | | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| HSO Innovation | Dynamics Online Product Catalog | | | | | ✅ | | | ✅ |
| HSO Innovation | Dynamics Tender Management | | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| ClickDimensions | ClickDimensions | | | | | ✅ | | | |
| MsCrmAddons.com | DocumentCorePack | | | | | ✅ | | | |
| Demianrasko (github) | Dynamics 365 Workflow Tools | Open source, license requires notification | | | | ✅ | | | |
| Quadira | Advanced Forms for CE | https://quadira.com/advanced-forms-for-crm-ce/ | | | | ✅ | | | |

The following solutions are available, but have other alternatives.

| Publisher | Solution(s) | Notes | DE | IND | INT | NL | UK | USA | NN |
|---|---|---|---|---|---|---|---|---|---|
| XpertDoc | SmartFlows / Xperido | Document Core Pack is the preferred solution | | | | ✅ | | | |
| MsCrmAddons.com | Attachmentextractor | HSO Document Manager is preferred. | | | | ✅ | | | |
| Microsoft Labs | Attachment Management | Not secure and the HSO document manager is an alternative. | | | | ✅ | | | |

### External Services

The following external services may be used.

| Publisher | Solution(s) | Notes | DE | IND | INT | NL | UK | USA | NN |
|---|---|---|---|---|---|---|---|---|---|
| https://postcode.nl | Address Validation (NL) | REST API, Paid service | | | | ✅ | | | |
| https://vies.eu | VAT Validation (EU) | REST API, Free service, with limits | | | | ✅ | | | |
| https://nl.iban.com | IBAN / BIC Validation | REST API, Paid service | | | | ✅ | | |
