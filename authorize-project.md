---

copyright:
  years: 2023, 2025


lastupdated: "2025-01-08"


keywords: authorizing a project, add project to account, project secrets, project API key, authenticate, authentication for a project, target account

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Using an API key with {{site.data.keyword.secrets-manager_short}} to authorize a project to deploy an architecture
{: #authorize-project}

When you configure your deployable architecture, you are required to select an authentication method. You can use an API key that is stored as a secret to authorize a project to deploy in an account.
{: shortdesc}

A secret is a piece of sensitive information, for example an API key, password, or any type of credential that you might use to access a confidential system. You can use a secret in {{site.data.keyword.secrets-manager_full}} as a way to manage API keys dynamically and store them securely in your own dedicated instance.

Though it is possible to add your API key directly into your configuration, it is not recommended, as the API key displays in your `project.json` file and is visible to anyone who exports it.
{: important}

## Before you begin
{: #gs-prereqs}

1. Make sure that you are assigned the required permissions to create the project tooling resources within the account in addition to permission to create projects:

   * The Editor role on the {{site.data.keyword.cloud_notm}} Projects service.
   * The Editor and Manager role on the {{site.data.keyword.bplong}} service.
   * The Viewer role on the resource group for the project.

   For more information about access and permissions, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).

1. Each API key that you create has the same access that you are assigned as a user. Before you create an API key, make sure that you have access to deploy architectures in the account that you want to deploy to, also known as your target account. If you have the following [wide-ranging access](/docs/secure-enterprise?topic=secure-enterprise-tp-project#serviceid-access-wide), then you can deploy architectures in the account:
   * The Administrator role for All Identity and Access enabled services.
   * The Administrator role for All Account Management services.

   Alternatively, you can [create an API key that is associated with a service ID](/docs/account?topic=account-serviceidapikeys&interface=ui#serviceidapikeys). Service ID API keys inherit all access that is assigned to the specific service ID. So, you can scope the access for the service ID to the minimum that is required for the deployable architecture that you're configuring. This approach is similar to using a trusted profile with specific access based on the deployable architecture. For more information on finding the required access roles for any given deployable architecture, see [granting specific access based on the deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-tp-project#serviceid-access-specific).

1. Complete the required steps to [create a project and add a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-setup-project). You authorize the project to deploy with an API key or existing secret when you configure your deployable architecture.

## Creating an API key
{: #create-new-secret}

Complete the following steps to create an API key:

1. In the {{site.data.keyword.cloud_notm}} console, sign in to the account that you want to deploy to.
1. Go to **Manage** > **Access (IAM)** > **API keys**.
1. Click **Create an {{site.data.keyword.Bluemix_notm}} API key**.
1. Enter a name and description for your API key.
1. Click **Create**.
1. Then, click **Show** to display the API key. Or, click **Copy** to copy and save it for later, or click **Download**.

   For security reasons, the API key is only available to be copied or downloaded at the time of creation. If the API key is lost, you must create a new API key.
   {: tip}

## Adding an API key to {{site.data.keyword.secrets-manager_short}}
{: #add-api-key-sm}

After you create an API key, complete the following steps to add it to {{site.data.keyword.secrets-manager_short}} in the account that contains your project:

1. [Create a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance&interface=ui) in your {{site.data.keyword.cloud_notm}} account. To create a secret, you must have the Writer role or higher on the {{site.data.keyword.secrets-manager_short}} service.
1. After you create your secret instance, make sure that you select **Other secret type** to add an arbitrary secret. For information about creating an arbitrary secret, see [Creating arbitrary secrets in the UI](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). Your arbitrary secret must contain the API key. The API key must be created in the target account that you want to deploy to.


## Using a secret reference
{: #deploy-secret-reference}

After you create a secret, you can use that secret as a reference to authorize your project to deploy. For more information about references, see [Referencing values](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#reference-values). To configure your deployable architecture with a secret reference, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, sign in to the account that you want to deploy the project to.
1. Go to **Menu** ![Menu icon](../icons/icon_hamburger.svg "Menu") > **Projects**.
1. Select the project that includes the deployable architecture configuration that you want to authorize.
1. From the **Configurations** tab, select the configuration.
1. Click **Edit**
1. In the Configure section, hover over the API key field, and select the **Key icon**.
1. Choose the service instance and select the secret that you want to use and click **OK**.

You can also use a trusted profile as a reference. In the Configure section, for the method, select **Trusted profile** and enter your trusted profile ID. For more information, see [Using trusted profiles](/docs/secure-enterprise?topic=secure-enterprise-tp-project&interface=ui).
{: note}
