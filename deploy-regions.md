---

copyright:

  years: 2023, 2025

lastupdated: "2025-04-24"

keywords: multiple regions, deploy, projects

subcollection: secure-enterprise

content-type: tutorial
account-plan: paid
completion-time: 20m

---

{{site.data.keyword.attribute-definition-list}}

# Using projects to deploy a deployable architecture to multiple regions
{: #deploy-regions}
{: toc-content-type="tutorial"}
{: toc-completion-time="20m"}

This tutorial walks you through how to use [projects](#x2035151){: term} to deploy two slightly different configurations of the same deployable architecture to two different regions.
{: shortdesc}

Imagine you are a member of the _Example Corp_ enterprise. Your infrastructure architect discovered the VSI on VPC landing zone deployable architecture and [customized it to fully meet your business needs](/docs/secure-enterprise?topic=secure-enterprise-basic-custom). However, you need to deploy the deployable architecture to multiple regions for local data storage and performance or high availability reasons.

This tutorial uses a fictitious scenario to help you learn and understand how to use projects to deploy to multiple regions. As you complete the tutorial, adapt each step to match your organization's needs.

## Before you begin
{: #regions-prereqs}

1. [Set up your {{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-account-getting-started).

1. [Create a customized deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-basic-custom) called `Example Corps' infrastructure` and onboard it to a private catalog called `Example Corp catalog`. 

1. Make sure you have the following access roles to create a project and permission to create the project tool resources within the account:
    * The Editor role on the {{site.data.keyword.cloud_notm}} Projects service.
    * The Editor and Manager role on the {{site.data.keyword.bplong}} service
    * The Viewer role on the resource group for the project

    For more information about access and permissions, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).

1. Set up an authentication method. You can use an API key stored in {{site.data.keyword.secrets-manager_short}} or a trusted profile to authorize a deployment to your target account: 
    1. [Create a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance&interface=ui) in your {{site.data.keyword.cloud_notm}} account. To create a secret, you must have the Writer role or higher on the {{site.data.keyword.secrets-manager_short}} service. After you create your secret instance, make sure that you select **Other secret type** to add an arbitrary secret. For information about creating an arbitrary secret, see [Creating arbitrary secrets in the UI](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). Your arbitrary secret must contain the API key. The API key must be created in the target account that you want to deploy to. For more information, go to [Using an API key with Secrets Manager to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).

    1. Create a trusted profile in the account you want to deploy to with the ability to create a service ID, create and delete API keys for the service ID, and deploy the architecture. For more information, go to [Using trusted profiles to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-tp-project).

1. Create two SSH keys, one in the `us-south` region and one in the `us-east` region. For more information, go to [Getting started with SSH keys](/docs/vpc?topic=vpc-ssh-keys&interface=ui).

## Create a project 
{: #project-create}
{: step}

Create a project where you can configure and deploy Example Corps' infrastructure. 

1. In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **[Projects](/projects/)**. 
1. Click **Create**. 
1. Name your project `Example Corp infrastructure dev`. 
1. Add the following description to your project: `Project to manage the different configurations and deployments of Example Corps' infrastructure.`
1. Select **Dallas** as the region where the project data is stored.
1. Keep `Default` for the resource group.
1. Click **Create**.

## Create an environment in your project
{: #env-create}
{: step}

Now that your project is created, you're ready to create an environment to share values across configurations for easier deployments. The properties that you add to an environment are automatically added to configurations that are using that environment. For more information, see the [benefits to using environments](/docs/secure-enterprise?topic=secure-enterprise-best-practices-projects#best-practice-env). In this tutorial, you add the authentication method that you previously set up in the [before you begin](#regions-prereqs) steps. 

1. In the **Example Corp infrastructure** project that you just created, select **Manage** > **Environments**.
1. Click **Create**.
1. Name your environment `Example Corp infrastructure dev`.
1. Click **Add** > **Add manually…**
1. Select **Authentication** for the category. 
1. Specify the authentication method that you set up in the [before you begin](#regions-prereqs) steps. You can use an API key stored in {{site.data.keyword.secrets-manager_short}} or a trusted profile.
1. Depending on which method you choose, either select the secret that contains your API key or provide the trusted profile ID. 
1. Click **Save**. 

## Add a deployable architecture to a project
{: #regions-project}
{: step}

Before you can configure Example Corps' infrastructure, you need to find the deployable architecture in `Example Corp catalog` and add it to the `Example Corp infrastructure` project.

1. In the **Example Corp infrastructure** project that you just created, select **Configurations** > **Create**.
1. Use the menu to open the private catalog called `Example Corp catalog`. 
1. Search for **Example Corps' infrastructure** and select the deployable architecture from the search results.
1. Select **Add to project**.
1. Change the configuration name to `example-corp-us-south` to indicate that you want to deploy the configuration in the US southern region.
1. Select **Example Corp infrastructure dev** as the environment.
1. Click **Add**.

You successfully added the deployable architecture to a project and are ready to define the configuration.

## Configure the deployable architecture
{: #configure-architecture}
{: step}

1. In the **Details** section, review the information and make sure the `Example Corp infrastructure dev` environment is selected.
1. From the **Security** section, confirm the correct authentication method is selected based on what you added to the environment.
1. During validation, a Code Risk Analyzer scan is run on your architecture. In the Security and compliance controls area, select **Architecture default** to use the default controls that the owner of the deployable architecture added when they onboarded it. For more information about using **Architecture default** controls, see [Configuring the architecture](/docs/secure-enterprise?topic=secure-enterprise-config-project#how-to-config).
1. From the **Configure architecture** section, enter values for the required input variables for the deployable architecture configuration:
    
    1. Enter the name of an existing key in the `existing_ssh_key_name` input, for example, `existing-ssh-key-dallas`.
    1. Select **us-south** as the region to deploy the resources.
    1. Enter `us-south` as the prefix to use for naming conventions.

1. Click **Save**.
1. Click **Validate**. The modal that is displayed provides more details about your in-progress validation.

   If the validation fails, you can [troubleshoot the failure](/docs/secure-enterprise?topic=secure-enterprise-ts-na-failures). Or, an administrator on the {{site.data.keyword.cloud_notm}} Projects service can review the results through the {{site.data.keyword.bpshort}} service and [override the failure and approve](/docs/secure-enterprise?topic=secure-enterprise-approve-failed-validation) the configuration to deploy anyway. However, make sure that the pipeline failed due to the Code Risk Analyzer scan and not because of a validation or plan failure. It is not recommended to override a failure that is flagged due to a validation or plan failure as the configuration cannot deploy successfully. For more information about security and compliance in projects, see [Achieving continuous compliance as an enterprise](/docs/secure-enterprise?topic=secure-enterprise-continuous-compliance).

During the configuration and deployment process, monitor your [**Needs attention** items](/docs/secure-enterprise?topic=secure-enterprise-needs-attention-projects). The widget reflects any issue that occurs in your configurations.
{: remember}

## Approve and deploy your first configuration
{: #regions-first-deploy}
{: step}

As an Editor on the {{site.data.keyword.cloud}} Projects service, you can approve the configuration changes and deploy the configuration. It can be beneficial to deploy your first configuration to make sure your changes work as expected. Then, if the deployment is successful, you can continue to create your second configuration.

You must address any outstanding **Needs attention** items on the **Overview** tab before you can approve and deploy your configurations.
{: tip}

1. From the `Example Corp infrastructure` project, select the **Configurations** tab.
1. Click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for `example-corp-us-south` > **View last validation**. 
1. Add a comment with more details about the approval, and click **Approve**.
1. From the **Configurations** tab in your project, click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for `example-corp-us-south` > **Deploy** and wait for the deployment to finish.



## Add and configure the second deployable architecture
{: #configure-second-architecture}
{: step}

Now that you configured and deployed your architecture to one region, you can duplicate it to deploy the architecture to another region. 

1. From the `Example Corp infrastructure` project, select the **Configurations** tab.
1. Click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for `example-corp-us-south` > **Duplicate**. `example-corp-us-south-copy-01` is added to your project. 
1. Click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for `example-corp-us-south-copy-01` > **Edit**. 
1. From the **Details** section, change the name of the configuration to `example-corp-us-east` and make sure the `Example Corp infrastructure dev` environment is selected. 
1. From the **Security** section, review the information that was pulled in from the environment that you created.
1. From the **Configure architecture** section, enter values for the required input variables for the deployable architecture configuration:
    
    1. Enter the name of an existing key in the `existing_ssh_key_name` input, for example, `existing-ssh-key-dc`.
    1. Select **us-east** as the region to deploy the resources.
    1. Enter `us-east` as the prefix to use for naming conventions.
    
1. Click **Save**.
1. Click **Validate**. The modal that is displayed provides more details about your in-progress validation.

   If the validation fails, you can [troubleshoot the failure](/docs/secure-enterprise?topic=secure-enterprise-ts-na-failures). Or, an administrator on the {{site.data.keyword.cloud_notm}} Projects service can review the results through the {{site.data.keyword.bpshort}} service and [override the failure and approve](/docs/secure-enterprise?topic=secure-enterprise-approve-failed-validation) the configuration to deploy anyway. However, make sure that the pipeline failed due to the Code Risk Analyzer scan and not because of a validation or plan failure. It is not recommended to override a failure that is flagged due to a validation or plan failure as the configuration cannot deploy successfully. For more information about security and compliance in projects, see [Achieving continuous compliance as an enterprise](/docs/secure-enterprise?topic=secure-enterprise-continuous-compliance).

During the configuration and deployment process, monitor your [**Needs attention** items](/docs/secure-enterprise?topic=secure-enterprise-needs-attention-projects). The widget reflects any issue that occurs in your configurations.
{: remember}

## Approve and deploy your second configuration
{: #regions-second-deploy}
{: step}

After the validation completes, you can deploy your second configuration.

You must address any outstanding **Needs attention** items on the **Overview** tab before you can approve and deploy your configurations.
{: tip}

1. From the `Example Corp infrastructure` project, select the **Configurations** tab.
1. Click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for `example-corp-us-east` > **View last validation**. 
1. Add a comment with more details about the approval, and click **Approve**.
1. From the **Configurations** tab in your project, click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for `example-corp-us-east` > **Deploy** and wait for the deployment to finish.

After the deployment successfully completes, you have two slightly different configurations based on a single deployable architecture that is deployed in two separate regions.
