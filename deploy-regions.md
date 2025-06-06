---

copyright:

  years: 2023, 2025

lastupdated: "2025-05-28"

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

Imagine you are a member of the _Example Corp_ enterprise. You discovered the VSI on VPC landing zone deployable architecture and want to use it for your business. However, you need to deploy the deployable architecture to multiple regions for local data storage and performance or high availability reasons.

This tutorial uses a fictitious scenario to help you learn and understand how to use projects to deploy to multiple regions. As you complete the tutorial, adapt each step to match your organization's needs.

## Before you begin
{: #regions-prereqs}

1. [Set up your {{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-account-getting-started).

1. Make sure you have the following access roles to create a project and permission to create the project tool resources within the account:
    * The Editor role on the {{site.data.keyword.cloud_notm}} Projects service.
    * The Editor and Manager role on the {{site.data.keyword.bplong}} service
    * The Viewer role on the resource group for the project

    For more information about access and permissions, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).

1. Set up an authentication method.
    1.  [Create a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance&interface=ui) in your {{site.data.keyword.cloud_notm}} account. To create a secret, you must have the Writer role or higher on the {{site.data.keyword.secrets-manager_short}} service.

    1. After you create your secret instance, make sure that you select **Other secret type** to add an arbitrary secret. For information about creating an arbitrary secret, see [Creating arbitrary secrets in the UI](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). Your arbitrary secret must contain the API key. The API key must be created in the target account that you want to deploy to.

    For more information, see [Using an API key with Secrets Manager to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).

## Add a deployable architecture to a project
{: #regions-project}
{: step}

Before you can configure VSI on VPC landing zone, you need to find the deployable architecture in the {{site.data.keyword.cloud_notm}} catalog and add it to a project.

1. In the [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/catalog) console, click **Catalog**.
1. Search for **VSI on VPC landing zone** and select the deployable architecture from the search results.
1. Select **Review deployment options** > **Add to project** to be redirected to the **Projects** space.
1. In the **Name** field, enter `VSI on VPC landing zone regions` to name the project.
1. Add the following description to your project: `Project to manage the different configurations and deployments of VSI on VPC landing zone.`
1. Change the configuration name to `landing-zone-us` to indicate that you want to deploy the configuration in the US.
1. Select **Dallas** as the region where the project data is stored.
1. Keep `Default` for the resource group.
1. Click **Create**.

You successfully added the deployable architecture to a project and are ready to define the configuration.

## Configure the deployable architecture
{: #configure-architecture}
{: step}

1. In the **Define details** section, review the information.
1. From the **Security** tab in the **Configure** section, select **API key using {{site.data.keyword.secrets-manager_short}}** as the authentication method.
1. Click **Select from {{site.data.keyword.secrets-manager_short}}**.
1. Choose the service instance, secret group, and secret that you previously created in the [before you begin](#regions-prereqs) steps.
1. Click **Save**.
1. During validation, a Code Risk Analyzer scan is run on your architecture. In the Security and compliance area, select **Architecture default** to use the default controls that the owner of the deployable architecture added when they onboarded it. For more information about using **Architecture default** controls, see [Configuring the architecture](/docs/secure-enterprise?topic=secure-enterprise-config-project#how-to-config).
1. Select the **Required** tab to enter values for the required fields for the deployable architecture configuration.
1. Enter `existing_ssh_key_name` in the **ssh_public_key** field to use an existing key.
1. Select `us-south` as the region to deploy the resources.
1. Enter `us` as the prefix to use for naming conventions.
1. Keep the Optional values as-is.
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

1. From the `VSI on VPC landing zone regions` project dashboard, select the **Configurations** tab of one of the configurations.
1. Click **Edit** > **View last validation**.
1. Add a comment with more details about the approval, and click **Approve**.
1. From the **Configurations** tab in your project, click the name of your deployable architecture configuration > **Edit**.
1. Review the input changes, click **Deploy**, and wait for the deployment to finish.

## Create an environment
{: #regions-environment}
{: step}

If you successfully deployed your first configuration, you can now use it to create an environment to share values across configurations for easier deployments. The properties that you add to an environment are automatically added to configurations that are using that environment. For more information, see the [benefits to using environments](/docs/secure-enterprise?topic=secure-enterprise-best-practices-projects#best-practice-env).

1. In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **[Projects](/projects/)** and select a project.
1. From the **Manage** tab, select **Environments**.
1. Click **Create**.
1. Name your environment `VSI on VPC landing zone regions dev`.
1. Click **Add** > **Add from a configuration…**
1. Select `landing-zone-us` to use the configuration you previously configured.
1. Since we want to keep the configuration the same but deploy to a different region, select all properties except `region` and `prefix`.
1. Click **Add > Save**.

## Add a second deployable architecture to a project
{: #next-regions-project}
{: step}

Now that you created an environment, you can use it to configure the second deployable architecture.

1. From the `VSI on VPC landing zone regions` project dashboard, select the **Configurations** tab.
1. Click **Create**.
1. Select VSI on VPC landing zone from the catalog.
1. Click **Review deployment options**.
1. Click **Add to project**.
1. Make sure **Add to existing** is selected.
1. Choose `VSI on VPC landing zone regions` as the project.
1. Update the configuration name to `landing-zone-eu` to indicate that you want to deploy the configuration in Europe.
1. Select `landing zone dev` as the environment.
1. Click **Add**.

## Configure the second deployable architecture
{: #configure-second-architecture}
{: step}

1. From the **Define details** section, review the information and make sure the `landing zone dev` environment is selected.
1. From the **Security** tab in the **Configure** section, review the information that was pulled in from the environment that you created.
1. Select the **Required** tab to enter values for the `region` and `prefix` variables.
    The value for `ssh_public_key` is pulled in from the environment that you created.
    {: note}

1. Select `eu-de` as the region to deploy resources.
1. Enter `eu` as the prefix to use for naming conventions.
1. Select the **Optional** tab to review the optional values that were pulled in from the environment.
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

1. From the `VSI on VPC landing zone regions` project dashboard, select the **Configurations** tab of one of the configurations.
1. Click **Edit** > **View last validation**.
1. Add a comment with more details about the approval, and click **Approve**.
1. From the **Configurations** tab in your project, click the name of your deployable architecture configuration > **Edit**.
1. Review the input changes and click **Deploy**.

After the deployment successfully completes, you have two slightly different configurations based on a single deployable architecture that is deployed in two separate regions.
