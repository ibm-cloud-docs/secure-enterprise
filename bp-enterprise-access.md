---

copyright:
  years: 2023, 2024
lastupdated: "2024-09-18"

subcollection: secure-enterprise

keywords: enterprise, enterprise account, enterprise iam, enterprise template, iam template

---

{{site.data.keyword.attribute-definition-list}}

# Best practices for assigning access in an enterprise
{: #access-enterprises}

While you're deciding how to [set up your enterprise and account structure](/docs/secure-enterprise?topic=secure-enterprise-enterprise-best-practices) within the enterprise, begin planning how to assign access to identities in your organization. These best practices provide you with the basic building blocks to enable successful and secure app development in {{site.data.keyword.cloud}}.
{: shortdesc}

To learn more about organizing resources and assigning access in child accounts or nonenteprise accounts, see [Best practices for organizing resources and assigning access](/docs/account?topic=account-account_setup).

## How enterprise-managed IAM access works
{: #how-enterprise-iam}

Centrally manage identity and access management (IAM) for your organization by using IAM templates to assign access and manage security settings. Create templates for IAM resources in your [enterprise account](x9863395){: term} to standardize access groups, trusted profiles, account security settings, and resource permissions across accounts in your organization. After you assign an IAM template, the child accounts that you select are assigned an enterprise-managed IAM object with the associated attributes, like policies and action controls. Action controls determine whether administrators of IAM services in child accounts can modify the enterprise-managed object in their account.

Users in a child account can determine that an IAM resource comes from an enterprise-managed IAM template by the [enterprise-managed]{: tag-cyan} tag on the resource in the console. The API response for enterprise-managed IAM objects in child accounts includes a `“template”` section.

Depending on how you configure the action controls for a template, child accounts might modify the IAM resource that you assigned. In this case, only that instance of the resource is impacted. An instance of the same IAM resource in a different account isn't affected.
{: note}

One advantage of IAM templates is the reduced time and effort to manage access in your organization. For example, instead of creating an access group with the same permissions in each account, create one access group template at the enterprise level, and assign that access group template to child accounts. The assignment creates the access group and its associated policies in each child account, saving you from manually creating hundreds of policies. Learn about other strategies for [Reducing time and effort to manage access](/docs/secure-enterprise?topic=secure-enterprise-access-enterprises&interface=ui#bp-enterprise-access-include-limit-policies).

IAM templates help prevent policy drift by using the practice of immutable infrastructure. Immutable infrastructure is a DevOps practice that helps ensure all enterprise-managed IAM resources remain in a consistent state, reducing the risk of configuration errors. With immutable infrastructure, any changes to a template are made by creating a new instance or version of the template, rather than modifying an existing one. This means that the history of the template can be easily traced, as each instance or version represents a specific point in time. Tracking changes through the creation of new instances provides an audit trail that can help identify and address any security issues, making your IAM resources more reliable and secure. For more information about auditing enterprise IAM events, see [Monitoring enterprise IAM templates](/docs/secure-enterprise?topic=secure-enterprise-monitor-enterprise-iam-templates).

With enterprise-managed IAM, you can grow your organization at scale. By assigning instances of templates as needed, you can avoid the complexity and risk of modifying existing instances. This way, it's easier to assign existing templates to new accounts.

Test a new template version in select accounts before you assign it to wider audiences. You can roll back to a past version if the new version doesn't behave the way that you want.
{: tip}

## When should I use an IAM template?
{: #when-to-use-template}

To know when to use a template to assign access or manage child account settings in your enterprise, consider the following factors:

Number of child accounts
:   If you have many child accounts, it can be time-consuming and error-prone to manually configure access policies and settings for each account. In this case, by using IAM templates, you can save time and ensure consistency across all accounts.

Common access requirements
:   If all or most of the child accounts require the same access policies, it makes sense to use a template to apply these policies uniformly. This ensures that all accounts have the necessary permissions to perform their intended tasks.

Security requirements
:   If you have strict security requirements, such as compliance with industry regulations or company policies, IAM templates can ensure that all child accounts are configured to meet those requirements and stay compliant. This can help reduce the risk of security breaches and unauthorized access.

Changes to access policies
:   If you need to update access policies for multiple child accounts, IAM templates can make the process more efficient. Instead of manually updating each account, you can update the template and apply the changes to all accounts at once.

Secure by default
:   If you need to ensure that new accounts are configured to your standards by default, you can assign an IAM template to an account group. Assigning a template at the account group level ensures that new accounts that you create within that account group, or existing account that you move to it, are automatically assigned the same template.

## Access group templates
{: #ag-enterprise-templates}

An access group template is a predefined access group with a set of associated access policies that can be applied to one or multiple child accounts or [account groups](x8622525){: term}. Access group templates can include permissions for different services and resources, and the level of access for the group. By using access group templates, enterprise administrators can ensure that child accounts with common access requirements are configured with the same access policies, reducing the risk of misconfigurations and unauthorized access.

For more information, see [Creating enterprise-managed access group templates](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create).

## Trusted profile templates
{: #tp-enterprise-templates}

With trusted profile templates, you can automatically grant federated users access to child accounts with conditions based on SAML attributes from your corporate directory. You can skip the account invitation process and define conditions that map the people in your organization to the right account. This way, the right access is already set up for them when they apply a trusted profile.

For more information, see [Creating enterprise-managed trusted profile templates](/docs/secure-enterprise?topic=secure-enterprise-tp-template-create).

## Settings templates
{: #settings-templates}

Enterprise-managed settings templates ensure that IAM settings, like [restricting domains for account invitations](/docs/account?topic=account-restrict-acct-invite) and [setting limits for login sessions](/docs/account?topic=account-iam-work-sessions&interface=ui), are consistent across multiple accounts. You can assign a settings template to existing accounts and to accout groups to help streamline the process of creating new accounts that are secure by default. By selecting an account group, you assign a settings template to existing and future child accounts of that group. Administrators can apply the appropriate template to an account group to ensure that new accounts are configured consistently and in compliance with organizational policies and best practices. This can save time and reduce the risk of misconfiguration or security vulnerabilities.

For more information, see [Creating enterprise-managed settings templates](/docs/secure-enterprise?topic=secure-enterprise-settings-template-create).

## Access policy templates
{: #policy-templates}

Access policy templates define a policy without requiring a subject, and you can use them to grant access to multiple subjects.

Spend less time on configuring individual policies and use access policy templates to quickly grant the right access in access group and trusted profile templates. For more information, see [Creating access policy templates](/docs/secure-enterprise?topic=secure-enterprise-policy-template-create).



## Authorization policy templates
{: #authorization-policy-templates}

Authorization templates make it easy to create predefined permission sets that allow one service to access another. They also automatically authorize dependent services, ensuring that services have the access they need.

In an authorization policy, the source service gains access to the target service based on assigned roles. While the target service is always within the account where the authorization is created, source services can be from the same or different accounts. Authorization templates standardize authorization policies across your enterprise, ensuring consistent and secure configurations while minimizing unauthorized access.



## Action controls
{: #action-controls}

Action controls are a mechanism to define the operations that administrators of IAM services in child accounts can take on the enterprise-managed IAM resources that you assign to their account. For example, by default an action control prevents access group administrators from adding access policies to an enterprise-managed access group.

Action controls are available only for access group templates.
{: note}

For access group templates, the best practice is to restrict changes to policies, but to allow access group administrators to add members. This way, you can create role-based access group templates with the right access for those roles and delegate managing membership to the access group administrator. Learn more about the available action controls for access group [members](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create&interface=ui#members-action-controls), [dynamic rules](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create&interface=ui#dynamic-rules-action-controls), and [access policies](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create&interface=ui#access-ag-action-controls).


## How can I grant access to child account resources with IAM templates?
{: #assign-resources-templates}

Resources that users provision in your enterprise reside in child accounts. The best way to organize resources in child accounts is by using access management tags. When you assign IAM templates across multiple accounts, the access policies must contain a resource pattern that's applicable to all child accounts that you target. For example, you can't specify a resource group in an enterprise-managed IAM policy because the GUID that identifies a resource group is unique. However, you can specify a pattern that matches common access management tags in your organization.

Some services, like account management services, don't have provisionable resources, so you don't need to scope policies for those services to specific resources.

Use IAM templates to grant access in child accounts to resources like all of a service's resources, all resources with a specific tag, or all resources in the account. Review the following sample access policies to help you determine how you might want to grant access in access group and trusted profile templates.

### Sample access policies for account management services
{: #ent-acct-management}

Account management roles are a great candidate for enterprise-managed access group and trusted profile templates because there aren't specific resources to scope the policy to.

- A policy that grants a platform administrator role on the group of **All IAM Account Management services**. This policy is appropriate for IAM administrators in your organization.

### Sample access policy for all resources in the account
{: #ent-entire-account}

- A policy that grants a platform administrator role on the entire account, **All IAM-enabled services**. The users with this access can perform any platform actions on any resource in the entire account and management actions, such as managing resource groups and attaching and detaching tags in the account.

### Sample access policy for a resource group only
{: #ent-rg-only}

- A policy with a viewer role on **Resource group only**. Users with this policy have visibility to view all resource groups in an account. This policy is required to create instances of any service in a resource group in addition to a role on the service that they want to create an instance of.

### Sample access policy for all of a service's resources
{: #ent-all-resources}

- A policy that grants a platform administrator role on the **IBM Cloud Kubernetes Service**. Users with this policy can access all instances of the service. Users with an administrator role that is assigned on any resource can also grant access to that resource.

### Sample access policy for a specific group of resources
{: #ent-specific-resources}

You can create policies with more granularity by using access management tags.

- A policy with an administrator role on all IAM-enabled services. Scope the policy to specific resources by selecting the attribute for access management tags. Enter the `project:chatbot` tag, which gives users administrator access in child accounts to all of the resources with that tag. This way, they have the access that they need for that specific project.

{{../account/bp_account.md#compare-accessgroups-trustedprofiles}}

{{../account/bp_account.md#limit-policies}}

For more information about IAM access and the available features, see [How {{site.data.keyword.cloud_notm}} IAM works](/docs/secure-enterprise?topic=secure-enterprise-iamoverview).


## How can I grant permissions to satisfy service dependencies across my enterprise?
{: #assign-authorization-policy-templates}

Use IAM authorization policy templates to ensure that services in each account have access to the dependencies that they need to function. Review the following sample authorization policies to guide you in setting up these relationships.

### Allow a centralized resource instance access to many sub-account resources
{: #sample-policy-central}

Create an authorization template to define the source account and targeted resources

- Starting with the **Source** of the authorization set the **Source account** using **Specific account** to indicate the account that contains the centrally managed resource instance. The **Service** and **Service instance** should also be added to refine access granularity.
- For the **Target** resource, set the **Service** and potentially the **Resource type** of the resource being accessed.
- Lastly, select the **Roles** that grant the least privileged access the **Source** service instance needs against the **Target** resource.

Assigning the template results in the creation of the corresponding authorization policy in each assigned sub-accounts.

### Set up pre-defined service to service access relationship within sub-accounts
{: #sample-policy-s2s}

Create an authorization template that specifies the target and source service and resources. You can omit any account IDs. This way, the unique account IDs of each assigned account is used in the policy.

- Set the **Source account to Assigned accounts(s)**.
- Set the applicable **Service** and add any relevant **Resources**
- For the Target resource, set the Service and the Resource type of the resource being accessed.
- Select the **Roles** that grant the least privileged access that the **Source** needs on the **Target** resource.

When you assign the authorization template to child accounts, you create a corresponding authorization policy in each assigned child account.


## Use cases for assigning enterprise-managed IAM templates
{: #usecases-templates}

Review the following use cases to develop a customized plan that suits the needs of your enterprise. For each use case, it's recommended to use enterprise-managed IAM templates to maintain consistency and compliance by centrally managing IAM for an organization.

### Multiple accounts require common access policies
{: #accounts-common-access}

Developers and administrators in my enterprise's child accounts have different access needs. Administrators need to configure Container Registry and create and manage Kubernetes clusters. Developers need to build and deploy apps by using Kubernetes. You can assign users service access roles to namespaces within clusters in accounts across your organization so that users have the minimum privileges they need to do their job. Make sure that your organization is using a consistent naming pattern for namespaces so that your policies work as you intend.

I create an access group template for each job role:

- Create an access group template for developers. Assign a policy with **Writer** service access role for the Kubernetes namespace `test` in all clusters.
- Create an access group template for administrators and assign two policies. First, assign a policy with the **Manager** role on the Kubernetes service. Next, assign a policy with the administrator role on the Container Registry service.

For each access group template, set the action control for adding members to Yes so that access group administrators in child accounts can add the right members to the enterprise-managed access group in each account.

When you assign access to a namespace, the policy applies to all current and future instances of the namespace in any cluster in the account that you authorize.
{: note}

### A few users need temporary access to all accounts
{: #access-all-accounts}

I need to give compliance focals responding to audit evidence requests from a team of auditors access to all of my existing accounts so that they can view billing resources and generate reports.

- Create a trusted profile template that has Viewer access on the Billing service in the accounts where I assign the template. I create conditions based on attributes from my identity provider (IdP). The attributes target the compliance focals, who are normally not members of any particular account. If a user meets the conditions, they can apply the trusted profile in each account.

The compliance focals can collect the evidence that the team of auditors needs to accomplish their yearly audit across all accounts in the enterprise by applying the trusted profiles. When they're finished with the audit, I can remove the template assignment and reassign it the following year when compliance focals need access again.

### Enforce MFA for new and existing accounts
{: #settings-all-accounts}

My company has a new compliance regulation that requires all new and existing accounts to use multifactor authentication (MFA).

- Create a settings template that defines the level of MFA that my company requires. Assign the settings template to all account groups.

All existing accounts now require users to log in with MFA. Any time that a new account is created within an account group, it inherits the settings template. This way, all new accounts automatically comply with the company compliance regulations.

### Assigning access in multiple environments by using tags
{: #am-tags-enterprise}

Some users in my enterprise's child accounts need access to resources in [different environments](/docs/enterprise-account-architecture?topic=enterprise-account-architecture-principles#dev-and-prod) within account groups. Account groups in my enterprise have two child accounts. Development and test environments reside in one account, and production environments are in a separate account. Resource administrators in my enterprise use [access management tags](/docs/account?topic=account-tag&interface=ui#create-access-console) to group `env:dev`, `env:test`, and `env:prod` resources in their accounts. They also group resources by using the tags `resource:storage`, `resource:containers`, `resource:security`. This way, enterprise administrators can create more granular policies for different environments in the enterprise.

I create an access group template for each type of user that exists in child accounts that contain development and test environments:

- Create an access group template for users who need to debug code. Assign a policy with a reader role on all IAM-enabled services. Scope the policy to specific resources by selecting the attribute for access management tags. Enter the tags `env:dev` and `resource:storage`, which gives child account users in the enterprise-managed access group reader access to `storage` resources in the development environment.
- Create an access group template for users who need to push code to development and test environments. Assign a policy with a writer role on all IAM-enabled services. Scope the policy to specific resources by selecting the attribute for access management tags. Enter the tags `env:dev`, `env:test`, `resource:storage`, and `resource:containers`, which gives child account users in the enterprise-managed access group administrator access to `storage` and `containers` resources in development and test environments.

### Allow a central backup service to backup Cloud Object Storage across your enterprise

To allow an Enterprise administrator to centrally manage backups, child accounts must authorize the enterprise's central backup service to interact with their resources.

- Create an authorization template that specifies the source service as a backup service instance in a particular enterprise account. Select **Source account > Specific account** and enter the account ID where you have the backup service instance.
- Then go to **Service > Service instance** and select **IBM Cloud Backup for VPC** instance. For the Target set the resource as **Cloud Object Storage** without specifying a specific account.

When this template is applied to child accounts, an authorization policy is automatically created in each, granting the **IBM Cloud Backup for VPC** instance access to all **Cloud Object Storage** resources. For a more detailed example, including role definitions, see [Establishing service-to-service authorizations for the Backup service.](/docs/vpc?topic=vpc-backup-s2s-auth&interface=ui)

## Enterprise-managed IAM limitations
{: #enterprise-limitations}

You can't assign an IAM template to the enteprise account, only child accounts of the enterprise.

| Resource                               | Max  |
|----------------------------------------|------|
| Access group templates per enterprise account | 100 |
| Account settings templates per enterprise account | 10 |
| Policy templates per enterprise account | 500 |
| Trusted profile templates per enterprise account | 100 |
| Versions per enterprise-managed template | 100 |
{: caption="Table 1. Enterprise-managed IAM account limits" caption-side="top"}

For more information, see [{{site.data.keyword.Bluemix_notm}} IAM limits](/docs/account?topic=account-cloudaccess#iam_limits).
