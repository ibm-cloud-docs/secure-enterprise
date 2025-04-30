---

copyright:

  years: 2022, 2025

lastupdated: "2025-04-30"

keywords: deployable architecture, deployment architecture, da, module, infrastructure as code, what is, stack, variation

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# What are modules and deployable architectures?
{: #understand-module-da}

Creating secure, compliant, and scalable application infrastructure can be difficult to set up and costly to maintain. Instead of figuring out how to assemble a compliant infrastructure architecture on your own, you can take advantage of modules and deployable architectures. Modules and deployable architectures can help you to create a framework around how resources are deployed in your organization's accounts. By working with these reusable configurations, you can define the standard for deployment once and ensure that it is easily repeatable for each member of your organization.

For example, think about an architect who is building an apartment complex. These designs are typically executed in a modular way. There are patterns for standard one-bedroom, two, or three-bedroom apartments. The builder can combine the standard apartments, each functional in their own way, into a larger, more complex, but functional living arrangement. IBM applied this same analogy to deploying solutions on the cloud. Rather than your organization spending months figuring out how to get services and software to work together, you can use {{site.data.keyword.cloud_notm}}'s well-architected patterns. Each pattern is packaged as composable, automated building blocks known as modules and deployable architectures.


## What is a module?
{: #what-is-module}

A module is a stand-alone unit of automation code that can be reused by developers and shared as part of a larger system. Similar to Node.js or Python packages, modules are a convenience to developers who are managing related resources. While it is possible to use modules alone, they're more powerful when you combine them to build a deployable architecture. Modules that are created by {{site.data.keyword.cloud_notm}} are made available in the [{{site.data.keyword.IBM_notm}} Terraform modules public GitHub org](https://github.com/terraform-ibm-modules/){: external}. For example, the [{{site.data.keyword.redhat_openshift_full}} VPC cluster on {{site.data.keyword.cloud_notm}} module](https://github.com/terraform-ibm-modules/terraform-ibm-base-ocp-vpc){: external} installs and configures a Red Hat OpenShift cluster on {{site.data.keyword.cloud_notm}}.






## What is a deployable architecture?
{: #what-is-da}

A deployable architecture is cloud automation for deploying a common architectural pattern that combines one or more cloud resources. It is designed to provide simplified deployment by users, scalability, and modularity. A deployable architecture incorporates one or more modules. Deployable architectures are coded in Terraform, which you configure with input variables to achieve the behavior that you want. To create a more complex deployable architecture, you can stack deployable architectures together without editing Terraform code to do so. 

![A deployable architecture, which contains modules](images/deployable-architecture.png){: caption="Deployable architecture that contains modules" caption-side="bottom"}

An example deployable architecture is [Cloud automation for {{site.data.keyword.secrets-manager_short}}](https://cloud.ibm.com/catalog/7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3/architecture/deploy-arch-ibm-secrets-manager-6d6ebc76-7bbd-42f5-8bc7-78f4fabd59). This deployable architecture provisions an {{site.data.keyword.secrets-manager_full_notm}} instance as a modular solution. You can use this architecture to securely manage secrets with your {{site.data.keyword.cloud_notm}} account. Cloud automation for {{site.data.keyword.secrets-manager_short}} has a narrow scope. It only deploys a {{site.data.keyword.secrets-manager_short}} instance, though the deployable architecture can also optionally create an {{site.data.keyword.keymanagementservicelong_notm}} key ring and key to encrypt data if one does not exist. 

Deployable architectures can be broader in scope, like [VPC landing zone](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-vpc){: external}. VPC landing zone provisions several virtual private clouds in a hub-and-spoke networking pattern that is connected by a transit gateway. It includes a number of supporting services that are used for the monitoring and security of the workloads that run on the VPCs.

Deployable architectures that are built and maintained by experts at {{site.data.keyword.cloud_notm}} are made available to you in the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog#reference_architecture){: external}. If you choose to create your own version of those deployable architectures, or build one from scratch, you can onboard your deployable architecture to a private catalog and share your ready-to-deploy solution with your organization through the catalog.

### What does it mean to stack deployable architectures? 
{: #stacked}

[Experimental]{: tag-purple}

For a more complex use case, you can stack architectures together to form a complete, end-to-end solution for deploying a complex application or infrastructure. Unlike individual modules or deployable architectures, which provide specific functionality, stacking combines multiple deployable architectures to form a comprehensive solution that can be easily deployed and managed. Just as modules can be combined to create a deployable architecture, deployable architectures can be stacked to create a more extensive solution. Stacking includes linking the architectures together to create a complex deployable architecture. This linking is achieved by specifying references in each deployable architecture's inputs by using a reference notation. You do not need to be an expert in Terraform, or have any Terraform coding skills, to stack architectures together and deploy them.

As an example, consider [Cloud automation for {{site.data.keyword.secrets-manager_short}}](https://cloud.ibm.com/catalog/7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3/architecture/deploy-arch-ibm-secrets-manager-6d6ebc76-7bbd-42f5-8bc7-78f4fabd5944-global?format=terraform&kind=terraform&version=v1.19.0#about){: external}. That deployable architecture includes the [{{site.data.keyword.secrets-manager_short}} module](https://github.com/terraform-ibm-modules/terraform-ibm-secrets-manager){: external}, which creates a {{site.data.keyword.secrets-manager_short}} instance. If you only require an instance of {{site.data.keyword.secrets-manager_short}}, that deployable architecture is a suitable choice. However, securely managing secrets is only one piece of maintaining security in the cloud. What about running compliance scans so you always know the posture of your resources? Or receiving notifications for critical events in your {{site.data.keyword.cloud_notm}} account? [{{site.data.keyword.cloud_notm}} Essential Security and Observability Services deployable architecture](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-core-security-svcs-0294f96e-7314-48d1-a710-c08a541b2119-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2cjaGlnaGxpZ2h0cw%3D%3D){: external} leverages the full range of security services from {{site.data.keyword.cloud_notm}}. It was created by stacking Cloud automation for {{site.data.keyword.secrets-manager_short}} along with deployable architectures that provision other {{site.data.keyword.cloud_notm}} services, like {{site.data.keyword.compliance_short}} and {{site.data.keyword.en_short}}. That deployable architecture provides a more comprehensive security solution that {{site.data.keyword.secrets-manager_short}} can't provide on its own.

![Stacking deployable architectures](/images/da-structure.svg){: caption="Stacking deployable architectures to build an end-to-end solution" caption-side="bottom"}

You can stack deployable architectures in an {{site.data.keyword.cloud_notm}} project and publish them together to a catalog without many extra validation steps. Or, you can stack architectures as you onboard a solution to a private catalog. From there, you can share it with others to save them the time of rebuilding it on their own.

This complex solution derives much of its cost, compliance, support, and quality assurances from its included deployable architectures. However, that complex solution has a unique version, description, and architecture diagram. If an update is made to a deployable architecture that's stacked with others in a catalog, the entire solution must be updated by the onboarder to use the latest version of that component deployable architecture. Updating the entire solution to a new version helps ensure that the latest update to a single architecture functions correctly within the broader solution. 

However, each deployable architecture has independent configuration states, allowing each one to be deployed, updated, or undeployed independently. For example, you already know that the {{site.data.keyword.cloud_notm}} Essential Security and Observability Services deployable architecture includes the Cloud automation for {{site.data.keyword.secrets-manager_short}} deployable architecture, among others. If Cloud automation for {{site.data.keyword.secrets-manager_short}} is updated, then {{site.data.keyword.cloud_notm}} Essential Security and Observability Services needs to be updated to use the latest version of {{site.data.keyword.secrets-manager_short}}. However, every architecture that's included in {{site.data.keyword.cloud_notm}} Essential Security and Observability Services doesn't need to be redeployed by the user consuming that solution. When the user updates their project to use the latest version, only {{site.data.keyword.secrets-manager_short}} needs to be redeployed.

![Updating a deployable architecture within a broader solution](/images/da-component-stack.svg){: caption="Updating a deployable architecture within a broader solution" caption-side="bottom"}

### What's included in a deployable architecture? 
{: #whats-included}

A deployable architecture might include variations, have dependencies, or be stacked together to create more complex solutions.

Variations
:  A variation is a type of deployable architecture that applies differing capabilities or complexity to an existing deployable architecture. For example, there might be a Quick start variation to your deployable architecture that has basic capabilities for simple, low-cost deployment to test internally. And, you might have a Standard variation that is a bit more complex that is ready for use in production.

Required architectures
:  A deployable architecture can contain inputs that require the outputs from other deployable architectures to successfully deploy. This relationship between a deployable architecture and another architecture it requires is commonly referred to as a dependency. You can meet dependencies in two ways: by stacking deployable architectures or by onboarding a deployable architecture to a private catalog and extending it to include other architectures.

   ![A deployable architecture with a dependency on another deployable architecture](images/deployable-architecture-extension.png){: caption="Deployable architecture with a dependency on another deployable architecture" caption-side="bottom"}

Optional architectures
:   Deployable architectures are designed to be flexible, so you can easily stack architectures together to build a more complete solution. However, a generally useful deployable architecture may not include the specific options that you need to meet a complex use case. For example, which database goes with your Java application? Do you need event notifications, a message queue, or some other optional service that’s not included by default? Optional deployable architectures can be stacked along with required deployable architectures to solve this customization problem for users.

Swappable architectures 
:   Whether an architecture is required to satisfy a dependency, or it’s included as an optional extension of an architecture, users might require more flexibility. You might know your Java application works with an optional database - but which database do your users require? Specifying swappable architectures for users helps them customize the end-to-end solution to meet their specific needs. After you stack any optional or required architectures together, you can group architectures as swappable with one another. The consuming user can then select which required architecture they want to use to satisfy a dependency, or select an optional architecture if they want to extend their use case.

Optional and swappable architectures can be added as you [onboard a deployable architecture to a private catalog](/docs/secure-enterprise?topic=secure-enterprise-extend-da&interface=ui). Currently, [stacking deployable architectures in a project](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui) does not support optional or swappable architectures.
{: important}

## What are projects and how do they work with deployable architectures?
{: #what-are-projects}

An {{site.data.keyword.cloud_notm}} project is a management tool that is designed to organize and provide visibility into a real-world project that exists in your organization. A project manages all of the configured instances of a deployable architecture and the resources that are related to the real-world reasons that they are deployed. Projects store versioned deployable architecture instances and organize the instances and resources into environments to help improve visibility into the development lifecycle. An environment is a group of related deployable architecture instances that share values for easier deployments. For example, development, test, or prod.

Projects are responsible for ensuring that only approved deployable architectures can be deployed. Additionally, they can help to ensure that the architectures and the resources that they created are up-to-date, compliant, and that drift does not occur over time. For example, you might have an account management application project. This project is designed to manage all of the resources that the account management application needs to deploy into a development, test, or production environment. Each environment has the same variables, such as a region or prefix, but has different values. When a deployable architecture is assigned to an environment through a project, their input values can automatically reference any of the environment's properties that have the same name. While {{site.data.keyword.cloud_notm}} projects are easy to create and update, they are not templatized or optimized for replication or sharing.

## How do I know which solution to create?
{: #which-component}

If you plan to create your own solution, the scope, coupling, whether it's deployable, and the purpose of your solution should all be taken into account. For guidance and use cases to help you decide what you plan to build, see [Planning and researching for designing an architecture](/docs/secure-enterprise?topic=secure-enterprise-starting-da-process) and [How do I decide what kind of component to create](/docs/secure-enterprise?topic=secure-enterprise-choose-plan-process).

The following table provides a high-level overview of why you might want to create the different components.

| Purpose | Recommended method | Why? |
|:--------|:----------------------|:-----|
| Creating a library of sharable automation components | Create a module | Modules provide reusable, curated automation to speed up the process for those who are creating and configuring deployable architectures. |
| Ensuring that your organization's cloud environment is secure and compliant | Create a deployable architecture | Deployable architectures are packaged in a way that you can define a secure and compliant deployment once and ensure that all members of your organization are repeating the deployment in the same way. |
| Architecting your own solutions | Stack deployable architectures together | By combining architectures, you can create a more complex end-to-end solution for your organization. |
{: caption="Understanding for automated deployments use-cases" caption-side="top"}
