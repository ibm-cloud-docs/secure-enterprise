---

copyright:
  years: 2022, 2026


lastupdated: "2026-01-07"


keywords: best practice projects, manage projects, project, environment

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}


# Best practices for organizing and managing projects
{: #best-practices-projects}

These best practices provide you with the basic building blocks to manage successful and secure [projects](#x2035151){: term} in {{site.data.keyword.Bluemix}}. Projects are beneficial to regulated enterprises as a way to best manage code-based deployments while maintaining compliance and collaborating with team members across accounts.
{: shortdesc}

## Creating a primary project account
{: #enterprise-home-account}

A key benefit of {{site.data.keyword.cloud_notm}} projects is the ability to centrally manage and collaborate your Infrastructure as Code deployments. If you are using an enterprise, establishing a primary or home account to store all of your projects helps manage and track your projects in one location.

The usefulness of establishing a primary account depends on your enterprise structure. Projects can be created in an account and deploy resources to other accounts. Users can view only the projects that are within the account that they're currently logged in to. In addition, reports for multiple projects can be generated only for projects within the same account. This is another useful benefit of establishing a common home account for all projects in an enterprise or all projects with a similar line of business. By keeping your project in a primary account, and deploying to separate accounts for each environment (dev, test, and production) to simplify enterprise management.

To make the account easier to identify the purpose and projects within the account by all users, give the account a human-readable name. For example `Front-end UI team` and `Back-end API team`.

## Defining an authentication method
{: #best-practice-auth}

When you configure your deployable architecture, you are required to add an authentication method. The authentication method identifies the target account where resources are deployed and authorizes the deployment. You can choose to authenticate through a trusted profile or an existing secret.

### Using trusted profiles
{: #best-practice-tp}

Some services cannot fully configure and deploy architectures by using trusted profiles. For more information, see [Known issues and limitations for projects](/docs/secure-enterprise?topic=secure-enterprise-known-issues#auth-known-issue).
{: important}

You can deploy an architecture in your own account or in another account by using trusted profiles. Depending on your organization, deploying an architecture might require access to another account by using a trusted profile and coordinating with administrators in multiple accounts. If the {{site.data.keyword.cloud_notm}} Projects service in another account needs access to your account to deploy an architecture, use trusted profiles and service IDs to authorize deployments in your account. For more information about creating a trusted profile for your project, see [Using trusted profiles to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-tp-project).

### Using {{site.data.keyword.secrets-manager_full}}
{: #best-practice-secrets}

When deploying Infrastructure as Code (IaC), there are often secrets that are required to configure the infrastructure, like API keys, SSH keys, and SSL certificates. In these cases, it's recommended to store these secrets within a [{{site.data.keyword.secrets-manager_short}}](https://cloud.ibm.com/catalog/services/secrets-manager#about) instance. Projects directly support referencing API keys that are stored in {{site.data.keyword.secrets-manager_short}} as an input to a deployable architecture. For more information, go to [Using an API key with {{site.data.keyword.secrets-manager_short}} to authorize a project to deploy](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).

Create a [{{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance&interface=ui) in your primary project account that you can use for all projects within that account before you create your project.
{: important}

There are a few different secret types that you can create. Use an arbitrary secret instance to store API Keys for your project. For more information, see [Creating arbitrary secrets in the UI](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui#arbitrary-ui).

Typically a single {{site.data.keyword.secrets-manager_short}} instance is used for all projects in an account. Secrets in that instance can be organized into secrets groups that align with access restrictions. For example, you might want to use a secrets group per project or for a set of related projects.

## Controlling deployments by using environments
{: #best-practice-env}

Within a project, you can group related configurations together by using an environment. An environment can also contain properties such as input values and authentication details. These properties are automatically added to a configuration when you select an environment, which helps ensure accurate deployments to your target account. When you edit a configuration, you can select an environment for the configuration to use in the **Define details** section.

### Benefits to using environments
{: #benefits-env}

Environments make it easier to control deployments. By specifying an environment and adding properties, you know that the same values are shared across configurations that are using that environment. Within a configuration, you can override any values that are automatically provided by an environment.

Environments provide a way to group related configurations together within a project. Let's say you have a set of configurations that you want to deploy to the same target account that's your development account. You can create a Development environment and add the authentication details for the target account to that environment. The authentication method is added to each configuration that's using the Development environment.

Though you can create as many environments as you want, it's recommended that you keep the number of environments in your project low. Using a standard set of environments for your deployments across accounts makes it easier to configure and deploy architectures.

For more information, see [Creating an environment](/docs/secure-enterprise?topic=secure-enterprise-create-env).

## Organizing your configurations
{: #best-practice-config}

Consider organizing all of the related configurations into a single project. This way, you can manage the deployments from one location and help ensure they're secure and compliant. This might consist of one or more deployable architectures to create the necessary infrastructure, which then needs to be replicated to support multiple regions and environments such as development, test, and production.

Use a naming convention for your configurations so that users can understand the function of each configuration. For example, in a deployment that uses a VPC Base deployable architecture and a Kubernetes Cluster deployable architecture that relies on the VPC Base, you might name your configurations as follows:

| Name   | Deployable architecture | Environment | Notes |
|--------|-------------------------|-------------|-------|
| Dev-VPC-Global | VPC Base               | Development | Create the base VPC for the development environment |
| Dev-Kub-Dallas | Kubernetes Cluster | Development | Create the cluster in Dallas for development |
| Dev-Kub-London | Kubernetes Cluster | Development | Create the cluster in London for development |
| Prod-VPC-Global | VPC Base               | Production | Create the base VPC for the production environment |
| Prod-Kub-Dallas | Kubernetes Cluster | Production | Create the cluster in Dallas for production |
| Prod-Kub-London | Kubernetes Cluster | Production | Create the cluster in London for production |
| Prod-Kub-Tokyo | Kubernetes Cluster | Production | Create the cluster in Tokyo for production |
| Prod-Kub-Sydney | Kubernetes Cluster | Production | Create the cluster in Sydney for production |
{: caption="Configuration name examples" caption-side="top"}

You can duplicate the development configuration in the `project.json` file and modify it as needed to quickly creating production deployments from their tested development deployments.
{: tip}

## Creating access groups for projects
{: #access-project-bp}

Access to projects is controlled by Identity and Access Management (IAM). It's recommended to create two or three access groups per project and assign users that work on the project to one of those access groups. For example, you might create a *ProjectName*-Reader access group that provides read-only access to the project for users who need to monitor costs or availability and a *ProjectName*-Writer access group for operations users who need to make changes to a project and deploy resources. If needed, you can create two writer access groups, one for users who can add configs and complete input values and a second for users who can deploy resources.

| Access group | Roles |
|--------------|-------|
| *ProjectName*-Reader | Reader, Viewer |
| *ProjectName*-Writer | Manager, Operator |
{: caption="Project access groups and roles" caption-side="top"}

To create new projects, users must be assigned specific access. For more information, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).
{: note}

## Monitoring needs attention items
{: #need-att-bp}

Needs attention items are best used to monitor validation, approvals, failures, and version updates. By checking the needs attention items regularly, you can ensure that your project and configuration are up-to-date and compliant.

## Adding tags to projects
{: #project-tags}

You can apply tags to organize, track, and manage your projects. It can be useful to add tags to related projects, or even a tag to identify temporary projects, such as infrastructure used for a customer demo or a prototype that's no longer needed. This allows temporary projects to be easily located and managed.

Tags are not case-sensitive, and the maximum length of a tag is 128 characters. The permitted characters are A-Z, 0-9, spaces, underscore, hyphen, period, and colon.

Resources in a project are automatically given service tags with the project ID and configuration ID with which they are associated. For more information, see [Tracking usage and spend for projects](/secure-enterprise?topic=secure-enterprise-project-usage-spend).
{: note}

## Undeploying resources created by projects
{: #project-resources-destroy}

When you deploy your configuration, resources that are created can be managed as a group within your project. These resources are created based on the Terraform plan and can be managed individually in your {{site.data.keyword.bpshort}} workspace.

Although you can destroy individual resources from the {{site.data.keyword.bpshort}} workspace, doing so is not recommended for resources that are created by using a project, as it leads to drift. Instead, you can undeploy all of the resources that are associated with a configuration at once from the project UI with a single click. Doing so removes the deployment from whatever target environment that your configuration was deployed to. Undeploying resources without deleting the configuration can be helpful if you need to deploy your configuration again in the future.

By default, when you delete a project or a configuration, any resources that were deployed are undeployed automatically. It's recommended to keep this setting enabled, but you can disable it by opening your project and going to **Manage** > **Settings**. If you disable that setting, resources remain deployed when you delete a configuration or a project, but you lose the ability to easily manage those resources within the project. Deployed resources can continue accruing costs to your target account if they remain available after a project or configuration is deleted. For more information, see [Undeploying resources](/docs/secure-enterprise?topic=secure-enterprise-remove-resources&interface=ui).
