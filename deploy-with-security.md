---

copyright:

  years: 2025

lastupdated: "2025-07-11"

keywords: deploy, customized application, security services, observability services, project, Code Engine, AI, application

subcollection: secure-enterprise

content-type: tutorial
account-plan: paid
completion-time: 40m

---

{{site.data.keyword.attribute-definition-list}}

# Deploying a customized app with security and observability services
{: #deploy-app-security-tutorial}
{: toc-content-type="tutorial"}
{: toc-completion-time="40m"}

This tutorial walks you through deploying a customized application to {{site.data.keyword.cloud}} by using deployable architectures that include security and observability services. You’ll learn how to integrate the {{site.data.keyword.cloud_notm}} Essential Security and Observability Services deployable architecture into a project that also includes a {{site.data.keyword.codeengineshort}}-based application that you previously created. By completing this tutorial, you’ll have a fully deployed and secured application with observability features that help you monitor and manage it effectively.
{: shortdesc}

Imagine you’re an account owner at an enterprise, responsible for deploying secure and observable cloud applications. You already customized the {{site.data.keyword.codeengineshort}} deployable architecture to meet your business needs. Now, you’re ready to extend your deployment by adding {{site.data.keyword.cloud_notm}} Essential Security and Observability Services.

This tutorial uses a fictitious scenario to help you understand how to integrate and deploy security and observability services alongside your application. As you complete the tutorial, adapt each step to match your own organization’s needs.

## Before you begin
{: #deploy-app-security-prereq}

Before you can start adding security and observability services to your customized app, complete the following prerequisites:

1. [Create a customized deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-basic-custom) called `Example Corp's infrastructure`.
1. [Onboard the deployable architecture to a private catalog](/docs/secure-enterprise?topic=secure-enterprise-onboard-da) called `Example Corp catalog`.
1. After you onboard `Example Corp's infrastructure` to the `Example Corp catalog` private catalog, [share it with your enterprise](/docs/secure-enterprise?topic=secure-enterprise-share-custom).

    If you're not ready to share your deployable architecture, you can skip the sharing step.
    {: note}

1. [Add your deployable architecture to a project called `Example Corp infrastructure` and deploy it](/docs/secure-enterprise?topic=secure-enterprise-deploy-regions). For the purposes of this tutorial, you might want to deploy the architecture only in one region. There’s no need to use multiple regions.

## Add observability and security services to your application
{: #add-observability-security-services}
{: step}

To secure your application and enable monitoring capabilities, add the {{site.data.keyword.cloud_notm}} Essential Security and Observability Services deployable architecture to your existing project.

1. In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **[Projects](/projects/)**. 
1. Select the `Example Corp infrastructure` project that contains your previously created customized deployable architecture and click **Configurations**.
1. Click **Create**. This step takes you to the {{site.data.keyword.cloud_notm}} catalog.
1. Find the **{{site.data.keyword.cloud_notm}} Essential Security and Observability Services** deployable architecture in the catalog and select it.
1. Click **Add to project**.
1. Name your configuration `security-services`.
1. Click **Add** to add the deployable architecture to your `Example Corp infrastructure` project.

## Configure the observability and security services deployable architecture
{: #configure-observability-security}
{: step}

After adding the {{site.data.keyword.cloud_notm}} Essential Security and Observability Services to your project, configure it to match your deployment requirements.

1. In the **Details** section, review the configuration details.
1. From the **Security** section, select **API key using {{site.data.keyword.secrets-manager_short}}** as the authentication method. Confirm that this is the correct authentication method selected based on what you [added to the environment](/docs/secure-enterprise?topic=secure-enterprise-deploy-regions#env-create).

    * [Create a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance&interface=ui) in your {{site.data.keyword.cloud_notm}} account. To create a secret, you must have the Writer role or higher on the {{site.data.keyword.secrets-manager_short}} service. After you create your secret instance, make sure that you select **Other secret type** to add an arbitrary secret. For information about creating an arbitrary secret, see [Creating arbitrary secrets in the UI](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). Your arbitrary secret must contain the API key. The API key must be created in the target account that you want to deploy to. For more information, go to [Using an API key with Secrets Manager to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).

1. Hover over the **api_key** field and click the **Secrets** icon ![Key icon](images/secret-key.svg "Secrets") to select a secret from {{site.data.keyword.secrets-manager_short}}.
1. Click **Next** to edit the basic configurations.
1. Enter a prefix to use for naming conventions.
1. You don't need to set the region value as that is set in the [Connect the customized application to the security and observability services](#connect-customized-da-services) step.
1. Turn on the **Advanced** configuration option if you need to fine-tune your configuration.
1. Click **Done** and then click **Save**.
1. Click **View stack configurations**. This step takes you to your configurations where you can find the `security-services` stack, which contains the following services that are ready to be validated:

    * Key management
    * {{site.data.keyword.cos_short}}
    * {{site.data.keyword.appconfig_short}}
    * Observability
    * {{site.data.keyword.en_short}}
    * {{site.data.keyword.sysdigsecure_full_notm}}
    * {{site.data.keyword.secrets-manager_short}}

## Connect the customized application to the security and observability services
{: #connect-customized-da-services}
{: step}

Before you can validate and deploy the security and observability services, connect your customized deployable architecture to the {{site.data.keyword.cloud_notm}} Essential Security and Observability Services. This connection helps ensure that both architectures are deployed together in the same region and can share configuration values, such as the region setting.

1. Go to the **Configurations** tab in the `Example Corp infrastructure` project.
1. Click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for the `security-services` configuration > **Edit**.
1. In the **Configure architecture** section, update the region value by clicking **Add a reference**. This step connects architectures together, so your customized architecture can be connected with {{site.data.keyword.cloud_notm}} Essential Security and Observability Services.
1. Select **Configurations** as the source and select the `example-corp-us-south` configuration.
1. Select **Inputs** as the category and select **region** as the property value.
1. Click **OK**.
1. Click **Done > Save**.

## Validate the configurations
{: #validate-configs-services}
{: step}

After you save the configurations and connect the customized architecture to the security and observability services, validate the configurations to ensure that they are correct and ready for deployment.

1. Go to the **Configurations** tab in the `Example Corp infrastructure` project.
1. Click **Validate** for each service that is ready for validation. The modal that is displayed provides more details about your in-progress validation.
1. If validation is successful, approve the configuration by entering a comment with more details about the approval, and clicking **Approve**.

## Deploy the configurations
{: #deploy-security-config}
{: step}

After successful validation, deploy the configurations of your architectures. Since the architectures are connected, they are deployed together in the same region.

1. Go to the **Configurations** tab in the `Example Corp infrastructure` project.
1. Click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for the `example-corp-us-south` customized deployable architecture > **Deploy**.
1. Click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for the services included in the `security-services` architecture > **Deploy**.

After deployment is complete, the customized application is connected with the {{site.data.keyword.cloud_notm}} Essential Security and Observability Services, all deployed in the same region.
