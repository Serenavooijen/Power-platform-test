# Managed Environments

Microsoft offers extra functionality to enhance the governance and security capabilities within the Power Platform. This page will go over the current state of features, the pros and cons of each feature and if the feature should be used.

General consensus is that Managed Environments should be used on each project or at least discussed.

## Managed Environment licensing

Licensing of Managed Environments doesn't influence projects where either Dynamics or Power Apps licenses are used. However, scenarios where a PoC or a small but high potential solution — e.g. Workplace management — is created in combination with SharePoint, Managed Environments has big impacts. It dictates that each user has to have a Dynamics, Power Apps Premium (previously Per User) or a Power Apps Per App plan license.

Another thing is that the owner of Power Automate Flows needs to have a Dynamics 365 Enterprise or a Power Automate Premium license, or the Flows need to have a Power Automate process license per flow. This means for projects without a Dynamics application, the service account needs to have a Power Apps Premium license (to be allowed to create Canvas and Model-Driven apps) and a Power Automate Premium license.

## Managed Environment features

| Feature | Description | Pros | Cons | Advice |
|---|---|---|---|---|
| Environment groups | It allows you to make groups of environments where each environment will have the exact same settings on PPAC that cannot be changed on a per environment basis | Allows to lock environment level settings; gives one overview of all applied settings; able to create a default group where every newly created environment should be added to, helping in having a baseline of settings across all your environments | - | Should be used |
| Limit sharing | - | Allows you to limit sharing across Canvas Apps, Power Automate and Agents | Limit only applies to Power Automate Solution Aware Flows; it doesn't unshare when the setting is more strict than the current users the component is shared with | Should be used |
| Weekly usage insights | - | Gives a condensed view of usage data, possible orphaned components and the most popular components | Tenant-level analytics need to be turned on | Yes, but only when there is an active monitoring strategy/associated goal |
| Data policies | Allows an admin to see all the DLP policies applied to the managed environment | When having a lot of environments and/or DLP policies, this gives a clearer picture | - | Depending on the organization this will be a big benefit. It will be on for each Managed Environment |
| Pipelines in Power Platform | - | Solution deployment is managed by the platform | Does not allow for complex deployment structures as needed for the HSO Deployment Best Practice | For smaller projects or citizen devs perhaps interesting, but not interesting for current HSO projects |
| Maker welcome content | The first time someone navigates to make.powerapps.com or the Copilot Studio they will see a customized welcome text | Allows the organization to redirect to internal (naming) conventions; allows for adding a read more link to dedicated content or an internal community/MS Teams Team | - | Not a big improvement. It is very easy for new makers to see it as a generic pop-up they will just close and not read. The goal of the Welcome Screen should also be achieved via employee training |
| Solution checker | Triggers a check on every custom solution import | Ensures a check on usual anti patterns on every custom solution import; ensures a check that no restricted components or entities are used so the solution stays license compliant | - | Would recommend using it, as it gives a base line level of insights in the usage of anti patterns, don't see the results as law. Together with PP Pipelines this would make more sense if, let's say, Citizen Devs do their own deployments to e.g. Test |
| IP Firewall | - | Security wise it offers basic additional benefits; easier to setup than conditional access policies | Working from home can become complicated as all ranges should be whitelisted | Shouldn't be just used without a strategy to keep the IP-list up to date. Using a VPN to connect with the customer environment would resolve the IP-list downside |
| IP Cookie binding | The cookie for the session will be connected to the IP that has been used to log in | Added security | It's needed to log in again if anything with the internet connection has changed | Yes — really good feature security wise ✅ |
| Customer Managed Key (CMK) | With this feature you can use your own encryption key from the Azure Key Vault | Added security | Optionally an additional license is needed — check this when you plan on implementing this feature; risk of a malicious actor that uses an Azure key vault key and afterwards revokes the access, basically locking all CMK enabled environments out of accessing the database | For certain customers this makes a lot of sense, but the management of the key should be completely done by the customer! HSO shouldn't take the responsibility for this feature ❗ |
| Lockbox | An interface for the customer to approve or reject a data access request by Microsoft | Everything will be tracked and stored for reviews and audits | This does not work for Cards for Power Apps, GPT AI features and Agent Builder, Lifecycle Services (Finance, Project Operations and Supply Chain Management) and Maker Welcome Content; features powered by Azure OpenAI Service are excluded, unless it says otherwise | Should be on — now the route is receiving an email asking for copy permission, this feature allows traceability |
| Extended backup | This is a feature to extend the retention period beyond the seven days for managed production environments that don't have Dynamics 365 applications | You could set the backup to 28 days instead of the 7 days | This is only possible through PowerShell to increase from 7 to 28 days at maximum | Generally not relevant — we should always turn 'Enable Dynamics 365 apps' on during environment creation (N.B. does require a D365 license to be able to do so!) |
| DLP for desktop flow | Applies Data Loss Prevention to desktop flows to prevent sensitive data from leaking through desktop processes | Prevents data leaks | Requires additional configuration | YES! ✅ Regular DLP's secure Cloud Flows, but why would your Desktop Flows still be allowed to access the secured data? |
| Export data to Azure Application Insights | With this feature, you can export data from Power Platform admin center to Application Insights | You can get more insights from the applications data | It will export a lot of data, so it can be tricky to extract good insights and can cost quite some money | Yes, but you need to take the amount of data and the associated cost into account. And there should be a monitoring strategy so this feature will actually be used instead of using up space and costing money |
| Administer the catalog | You can create a catalog for templates and components, which you can easily share with other people. This way everyone has the same templates and components and the same version | Everyone will be using the same versions of templates and components; you can update the versions of templates and components for everyone | It does require some work to keep everything up to date | No. Interesting perhaps for Citizen Devs, but only marginally. Downside regarding Citizen Devs is that they need to be properly licensed, so O365 or M365 licenses are not enough |
| Default environment routing | This feature makes it possible for Power Platform admins to direct new and existing makers to their own personal developer environment | By default, all the developer environments created will be managed | The created Developer environments are managed and, thus, the Power Apps Developer Plan is not sufficient and proper licensing is needed | Very interesting for larger organizations that want to have control of where their employees are building solutions |
| Create an app description with copilot | - | Saves time | The description could be less accurate | No, not interesting |
| Virtual Network support for Power Platform | Ensures that all communication from and to the platform is executed over a private network | Developers are not able to communicate with public components; public actors are not able to communicate with the platform | Service Bus is not allowed to be communicated to (for now); will be fairly expensive as VNet support is only accessible from the premium Service Bus offering | Makes a lot of sense to the right type of customer |
| Conditional access on individual apps | You can use this to apply Microsoft Entra conditional access policies to individual apps created using Power Apps | Higher application security; it's possible to have multiple authentication contexts on an app | Authentication contexts that are set on an app aren't moved with apps in solutions and moved across environments; has to be set via PowerShell | Interesting feature, but annoying way of administering and managing it. It seems like a good alternative to the IP Firewall if the customer doesn't have a VPN |
| Dataverse long term data retention | - | Data stays within Dataverse; data stored in long term data retention takes up around 50% less space; retention policies are solution aware; Fabric can be used for more complex queries | Data cannot be transferred back to the uncompressed state; deploying a solution aware policy does not enable the related child tables for said policy; you cannot export the data from within a Model-Driven App; attachments won't be visible on the data (Power Automate should be used for this); max of 5 users can query retained data at the same time; 100 queries per day are allowed per environment; queries are allowed on 1 table at a time (joins and aggregates are not possible); lookup data is denormalized to an ID and name, so updates to the related record most likely won't be visible in the name field of the retained lookup data | Whenever old data should be retained this can be a very viable cost efficient solution |
| Control which apps are allowed in your environment | When a user or an application retrieves Dataverse data, the ID of the application being used is sent with the token. This ID is checked if it is blacklisted. This feature enables you to control what is blacklisted/whitelisted | Ensures that only known applications are allowed to access Dataverse data; you can also link security roles to specific applications so only privileged users are allowed to run certain applications | - | It makes sense to turn on. Rather have an internal employee knock frustratingly on your door to request access for their application, than having an unnoticed security leak where someone creates their own application user which acts as a backdoor |
| Create and manage masking rules | This feature masks columns based on field-level security based on regular expressions | Ensures that only authorised users are able to see record specific confidential information | - | It can be really valuable, however, the use case has to present itself. If needed, it should be used |

## Lockbox

### What is it?

Customer Lockbox for Power Platform gives the customer's organization explicit control over the rare cases where a Microsoft support engineer needs to access their environment data to resolve a support incident. Instead of Microsoft having implicit standing access, a formal access request is routed to a designated approver in the customer's organization. The approver can accept or deny the request within a defined time window; if no action is taken, the request automatically expires and access is denied, similar to requesting a role in Azure.

Lockbox is part of the Managed Environments feature set and requires a Power Platform or Dynamics 365 enterprise license.

**Note:** Lockbox does not apply to all workloads. The following are currently excluded: Cards for Power Apps, GPT AI features and Agent Builder, Lifecycle Services (Finance, Project Operations and Supply Chain Management), Maker Welcome Content, and features powered by Azure OpenAI Service (unless explicitly stated otherwise).

### Implications

- **Audit trail** — Every access request, approval, and denial is logged and available for compliance reviews and audits. These audit requests can be viewed in Microsoft 365 Defender.
- **Approval window** — Approvals are, by default, active for 8 hours after approval. If the approvers do not approve the request within four days, the approval expires.
- **Operational impact** — If an approver is unavailable and a request expires, Microsoft support will not be able to access the environment. Ensure that all Power Platform administrators are brought up to speed on this process.
- **Scope** — Lockbox is a tenant level policy, and controls access at the environment level. Environments that are not Managed Environments are not covered.
- **Excluded workloads** — See the note above; sensitive workloads relying on Azure OpenAI or Lifecycle Services fall outside the Lockbox boundary and require a complementary governance approach. Besides that, emergency scenarios (such as major service outages requiring immediate attention), external legal demands for data and access, and manual review of customer data shared for Copilot AI features do not trigger lockbox requests.

**Note:** Lockbox does not trigger for emergency scenarios, external legal demands for data, and manual review of customer data shared for Copilot AI features.

### How to enable it

1. Ensure all environments you wish to have lockbox active for are Managed Environments (required prerequisite).
2. Navigate to the Tenant Settings in the Power Platform admin center.
3. Find and enable "Customer Lockbox".
4. Communicate the approval process and SLA expectations to the customer's Power Platform administrator(s).

**Recommendation:** Enable Lockbox, especially for customers in regulated industries. Combine it with an internal runbook that describes how approvers should evaluate and respond to access requests.

### Workflow

1. The customer's organization has an issue with the Power Platform and opens a Support Request through Microsoft Support.
2. The Microsoft engineer attempts to troubleshoot the problem through standard tools and telemetry. If the engineer needs to access customer data for further investigation, they start an internal approval process for access to customer data, regardless of whether the lockbox policy is enabled.
3. If lockbox is enabled for this tenant, and the environment that is currently investigated is a managed environment, the process generates a lockbox request. The designated approvers receive an email notification about the pending data access request from Microsoft.
4. The approver signs into the Power Platform admin center and approves the request. If the approver rejects the request, or does not approve within 4 days, the request expires and the Microsoft engineer gets no access.
5. After an approval for the request, the Microsoft engineer gets elevated permissions to the customer's data. After 8 hours, the access is automatically revoked.

*Source: Microsoft Learn*
