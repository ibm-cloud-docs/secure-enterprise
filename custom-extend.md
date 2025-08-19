---

copyright:

  years: 2025

lastupdated: "2025-08-19"

keywords: deployable architecture, customization, Elasticsearch, Postgresql, Code Engine, extend, stack, deployable architecture stack

subcollection: secure-enterprise

content-type: tutorial
account-plan: paid
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Adding customizable options to your deployable architecture 
{: #custom-extend}
{: toc-content-type="tutorial"}
{: toc-completion-time="30m"}

This tutorial walks you through one way to extend a deployable architecture with customizable options to fit your business needs. Complete this tutorial to learn how to create a new version of an existing deployable architecture in your private catalog and stack other architectures with it that users can choose to include. 
{: shortdesc}

Imagine you are a cloud automation engineering professional for the fictitious company _Example Corp_. You previously [customized a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-basic-custom) that is called `Example Corp's infrastructure` and added it to a private catalog. That customized deployable architecture is the base on which Example Corp's applications will be built. Now, you want to give your software developers some database options to use with `Example Corp's infrastructure`. After you browse the {{site.data.keyword.cloud_notm}} catalog, you decide to give your developers a choice between two deployable architectures that create different databases:

   [Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}](https://cloud.ibm.com/catalog/7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3/architecture/deploy-arch-ibm-icd-elasticsearch-7ee5876d-6e30-49d1-be25-259a442085e8-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2Fc2VhcmNoPUNsb3VkJTI1MjBhdXRvbWF0aW9uJTI1MjBmb3IlMjUyMGRhdGFiYXNlcyUyNTIwZm9yJTI1MjBlbGFzdGljc2VhcmNoI3NlYXJjaF9yZXN1bHRz){: external}
   :   This deployable architecture creates an instance of Elasticsearch, which is a NoSQL database suitable for full-text searching and querying large datasets. This database is a suitable choice for Example Corp's application, as it's flexible enough to process unstructured and semi-structured data like text, images, and video. This tutorial was created based on version 1.32 of the deployable architecture, but you can use a later version if you want to. 

   [Cloud automation for {{site.data.keyword.databases-for-postgresql}}](https://cloud.ibm.com/catalog/7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3/architecture/deploy-arch-ibm-icd-postgresql-0298facd-3e69-43fa-87c0-4d3d0b3c887e-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2Fc2VhcmNoPUNsb3VkJTI1MjBhdXRvbWF0aW9uJTI1MjBmb3IlMjUyMGRhdGFiYXNlcyUyNTIwZm9yJTI1MjBlbGFzdGljc2VhcmNoI3NlYXJjaF9yZXN1bHRz){: external}
   :   This deployable architecture creates an instance of {{site.data.keyword.postgresql}}, which is an SQL database for relational queries and structured data. This database is another suitable choice for Example Corp, as it manages structured transactional data to deliver personalized app experiences to users. This tutorial was created based on version 3.22, but you can use a later version if you want to. 

This tutorial uses a fictitious scenario to help you understand how to create a more complex deployable architecture by stacking it with other architectures. As you complete the tutorial, adapt each step to match your organization's needs.

## Before you begin
{: #extend-prereq}

1. Verify that you're using a Pay-As-You-Go or Subscription account by going to **Manage** > **Account** > **Account settings** in the {{site.data.keyword.cloud_notm}} console.
1. Verify that you're assigned the following {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) roles:
      * Administrator on All Account Management services and all IAM services.
      * Editor on the Catalog Management service.
      * Manager service access role for {{site.data.keyword.bpshort}}.
      * Other roles that are required for specific resources in your deployable architecture. Cloud automation for {{site.data.keyword.codeengineshort}} requires the Writer service access role that is scoped to all resources for the {{site.data.keyword.codeengineshort}} service. 

      For more information, go to [Assigning access to account management services](/docs/account?topic=account-account-services) and [Managing access to resources](/docs/account?topic=account-assign-access-resources).
1. [Create a customized deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-basic-custom) called `Example Corp's infrastructure` and onboard it to a private catalog called `Example Corp catalog`. This architecture is the one you will be extending as you complete this tutorial. 
1. Set up an authentication method. You can use an API key that is stored in {{site.data.keyword.secrets-manager_short}} or a trusted profile to authorize a deployment to your target account: 
    * [Create a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance&interface=ui) in your {{site.data.keyword.cloud_notm}} account. To create a secret, you must have the Writer role or higher on the {{site.data.keyword.secrets-manager_short}} service. After you create your secret instance, make sure that you select **Other secret type** to add an arbitrary secret. For information about creating an arbitrary secret, see [Creating arbitrary secrets in the UI](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). Your arbitrary secret must contain the API key. The API key must be created in the target account that you want to deploy to. For more information, go to [Using an API key with Secrets Manager to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).

    * Create a trusted profile in the account that you want to deploy to. The trusted profile needs the ability to create a service ID, create and delete API keys for the service ID, and deploy the architecture. For more information, go to [Using trusted profiles to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-tp-project).

## Creating a version of a deployable architecture
{: #create-version}
{: step}

Since `Example Corp's infrastructure` is already available in a private catalog, you need to update it with a new version. 

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Catalogs** > **Private catalogs** and open the private catalog `Example Corp catalog`. 
1. Select **Example Corp's infrastructure** from the list of products. 
1. Click **Versions** > **Add version**
1. Select **Terraform** as the delivery method. 
1. Specify whether your repository is public or private and provide the source URL. For the purposes of this tutorial, you can use the same source URL that you used to onboard version 0.0.1 of `Example Corp's infrastructure`. 
1. Select **Deploy Example Corp AI app on {{site.data.keyword.codeenginefull_notm}}** as the variation. 
1. Enter `0.0.2` for the software version. 
1. Click **Add version**

From here, you can configure details for the version that you added. 

## Extending Example Corp's infrastructure by adding database options
{: #extend-example-corps}
{: step}

As you configure the version details, extend `Example Corp's infrastructure` by stacking it with two other deployable architectures that create different databases. You want your users to choose either {{site.data.keyword.databases-for-elasticsearch_full_notm}} or {{site.data.keyword.databases-for-postgresql_full_notm}}. Complete the following steps: 

1. From **Step 3 - Extend your architecture**, click **Add**.
1. In the **Product** menu, search for `Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}` and select it. 
1. User Semantic Versioning Specification (SemVer) to list the versions of the architecture that work with Example Corp's infrastructure. 
    
    Typically, patch versions don't contain breaking changes, so you can use SemVer to specify a range of versions that are compatible with Example Corp's infrastructure, for example `~1.32`. That way, if Cloud automation for {{site.data.keyword.databases-for-elasticsearch}} is updated with a patch version to `1.32.5`, for example, any user who deployed Example Corps with Elasticsearch can update their project to use the new version. For major version changes to Elasticsearch, create a new version of Example Corp's infrastructure to make sure the new version of Elasticsearch functions properly with your architecture. 
    {: tip}

1. Select **Standard** variation. 
1. Specify **Standard** as the default variation, which is the variation that is selected for users by default. 
1. Select **Optional** from the Relationship menu, as the architecture is not required to deploy `Example Corp's infrastructure`.
1. (Optional) provide a display description to help users understand how this architecture works with `Example Corp's infrastructure` and what benefits it provides. For example, enter `NoSQL database for full-text searching and querying large datasets.`
1. Click **Add**. 

Now that you added Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}, repeat the steps again but this time, search for `Cloud automation for {{site.data.keyword.databases-for-postgresql}}`. For the optional display description, enter `SQL database for relational queries and structured data.` 

## Making the databases swappable with one another
{: #make-swappable}
{: step}

Now that you added both databases as optional to `Example Corp's infrastructure`, you can specify that they are swappable with one another. Your users can select which database option they want to use with `Example Corp's infrastructure`. Complete the following steps: 

1. From **Step 3 - Extend your architecture**, select the checkbox for **Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}**. 
1. Select the checkbox for **Cloud automation for {{site.data.keyword.databases-for-postgresql}}**.
1. Click **Group as swappable**. 
1. Enter `Databases` as the group name and click **Add**.
1. Click **Next**.

## Define variables for your users
{: #define-variables-example}
{: step}

Now that you added some database options to your architecture, you need to define variables for your users. Doing so connects the inputs in the architectures together, which makes it easier for your users to configure `Example Corp's infrastructure` for deployment. 

1. From **Step 4 - Configure the deployment details**, click **Define variables**. 
    
    The Define variables window displays with Cloud automation for {{site.data.keyword.databases-for-elasticsearch}} selected for you, so you can access the inputs and outputs from that architecture. From here, you can either add references or move inputs to `Example Corp's infrastructure` so users can configure them. 
    {: note}

1. When `Example Corp's infrastructure` was created, the region was restricted to the US. To make sure that Cloud automation for {{site.data.keyword.databases-for-elasticsearch}} uses the same region as `Example Corp's infrastructure`, add a reference to connect them together by completing the following steps:
    1. In the Required inputs window for Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}, select the **Reference a variable** icon ![Reference a variable icon](../icons/link.svg "Reference a variable") for the `region` input. 
    1. Then, select **region** in the **Variable name** menu.
    1. Click **Add**. When a user configures the `region` input in `Example Corp's infrastructure`, Cloud automation for {{site.data.keyword.databases-for-elasticsearch}} will use that value that the user provides as its input.

1. Since `Example Corp's infrastructure` also contains a `prefix` input, complete the same steps for the `prefix` input in Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}:
    1. Select the **Reference a variable** icon ![Reference a variable icon](../icons/link.svg "Reference a variable") for the `prefix` input.
    1. Then, select **prefix** in the **Variable name** menu. 
    1. Click **Add**.

1. Since `Example Corp's infrastructure` contains an input for the `existing_resource_group_name`, you can use that same resource group for Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}:
    1. Select the **Reference a variable** icon ![Reference a variable icon](../icons/link.svg "Reference a variable") for the `resource_group_name` input.
    1. Then, select **existing_resource_group_name** in the **Variable name** menu. 
    1. Click **Add**. 

1. Set the default value for `use_existing_resource_group` to **true** to indicate that your users need an existing resource group for your architecture. 
1. To simplify setup and reduce costs for a development environment, remove the key encryption requirement by completing the following steps: 
    1. Set the default value for `existing_kms_instance_crn` to `__NULL__`. 
    1. Click **Optional inputs**. 
    1. Set the default value for `use_ibm_owned_encryption_key` to **true**. 
    1. Set the default value for `plan` to `enterprise`.
    1. Set the default value for `member_cpu_count` to `2`. 
    1. Set the default value for `member_host_flavor` to `multitenant`. 

Now that you defined variables for Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}, you need to define variables for the other database option you included: Cloud automation for {{site.data.keyword.databases-for-postgresql}}. Complete the following steps: 

1. In the Define variables window, use the architecture menu to switch from **Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}** to **Cloud automation for {{site.data.keyword.databases-for-postgresql}}**.
1. Click **Required inputs** and remove the key encryption requirement by setting the default value for `existing_kms_instance_crn` to `__NULL__`. 
1. The three other required inputs in this architecture can all reference inputs that are already available in `Example Corp's infrastructure`. Complete the following steps to add references to these inputs: 
    1. Select the **Reference a variable** icon ![Reference a variable icon](../icons/link.svg "Reference a variable") for `resource_group_name`, then select **existing_resource_group_name** from the **Variable name** menu. Click **Add**, and the `resource_group_name` input within Cloud automation for {{site.data.keyword.databases-for-postgresql}} will reference the `existing_resource_group_name` value from Example Corp's infrastructure that your user provides.
    1. Select the **Reference a variable** icon ![Reference a variable icon](../icons/link.svg "Reference a variable") for `prefix` and select **prefix** from the **Variable name** menu and click **Add**. 
    1. Select the **Reference a variable** icon ![Reference a variable icon](../icons/link.svg "Reference a variable") for `region` and select **region** from the **Variable name** menu and click **Add**. 
1. Click **Optional inputs**. 
1. Set the default value for `use_existing_resource_group` to **true** to indicate that your users need an existing resource group for your architecture. 
1. Set the default value for `use_ibm_owned_encryption_key` to **true**. 
1. Click **Save**. 

## Communicate changes to your users
{: #comm-changes}
{: step}

The latest version of `Example Corp's infrastructure` doesn't contain breaking changes, but it does include new features that your users need to know about. Let your users know about the new database options they can use with `Example Corp's infrastructure` by adding a change notice. Complete the following steps: 

1. From **Step 6 - Add change notices**, click **Add new feature**. 
1. In the **Title** field, enter `Database options added to Example Corp's infrastructure`. 
1. For the **Description**, enter `Version 0.0.2 includes two optional databases that you can choose to add. You can either use Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}, or Cloud automation for {{site.data.keyword.databases-for-postgresql}}.` 
1. Click **Save**. 

From the **Actions** menu, select **Generate manifest** to download the latest changes. It's a best practice to download the files and add them to your source code repository so that your changes carry over into your next release. 
{: tip}

## Validate the architectures by deploying them from a project
{: #validate-deploy-project}
{: step}

Now, version 0.0.2 of `Example Corp's infrastructure` is available as a draft in your private catalog. Any user with access to your private catalog can deploy it. To make sure that the database options work with `Example Corp's infrastructure`, deploy it twice from your project. For one deployment, include Elasticsearch. And, for the other deployment, include {{site.data.keyword.postgresql}}. By deploying the architecture twice, you verify that both database options work with `Example Corp's infrastructure` as intended. 

Before you can [share `Example Corp's infrastructure` with your enterprise](/docs/secure-enterprise?topic=secure-enterprise-share-custom&interface=ui), you must validate the version as you onboard it to your private catalog. Currently, [validating the version in the catalog](/docs/secure-enterprise?topic=secure-enterprise-onboard-da#validate-version) does not validate any architectures that you stacked with your own. Deploying the architectures from a project while `Example Corp's infrastructure` is in a draft state in the private catalog helps ensure that the architectures you included function properly with `Example Corp's infrastructure`. 
{: important}

### Deploy Example Corp's infrastructure with Elasticsearch 
{: #deploy-elasticsearch}

1. Go to the [{{site.data.keyword.cloud_notm}} catalog](/catalog) and open the `Example Corp catalog` private catalog. 
1. Select **Example Corp's infrastructure** to open its catalog listing. 
1. Make sure **0.0.2** is selected as the product version. 
1. Click **Add to project**. 
1. Name the new project `Testing Example Corp infrastructure`. 
1. Change the name of the configuration to `Example Corp infrastructure with Elasticsearch` and click **Next**. 
1. Make sure that **Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}** is selected and click **Add to project**. 
1. [Configure the architecture](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#how-to-config) by completing the following steps: 
    1. In the **Details** section, review the information and click **Next**. 
    1. In the **Security** section, provide an authentication method to deploy to your target account and click **Next**. Use a trusted profile or store an API key in {{site.data.keyword.secrets-manager_short}}. 
    1. In the **Inputs** section, enter `test-south` as the value for the `prefix` input variable. 
    1. Select **Default** for the `existing_resource_group_name` input variable. 
    1. Select **us-south** for the `region` input variable. 
    1. Click **Save** to save the configuration of `Example Corp's infrastructure`. 
1. Click **Validate**. The modal that is displayed provides more details about your in-progress validation.

   If the validation fails, you can [troubleshoot the failure](/docs/secure-enterprise?topic=secure-enterprise-ts-na-failures). Or, an administrator on the {{site.data.keyword.cloud_notm}} Projects service can review the results through the {{site.data.keyword.bpshort}} service and [override the failure and approve](/docs/secure-enterprise?topic=secure-enterprise-approve-failed-validation) the configuration to deploy anyway. However, make sure that the pipeline failed due to the Code Risk Analyzer scan and not because of a validation or plan failure. It is not recommended to override a failure that is flagged due to a validation or plan failure as the configuration cannot deploy successfully. For more information about security and compliance in projects, see [Achieving continuous compliance as an enterprise](/docs/secure-enterprise?topic=secure-enterprise-continuous-compliance).

1. After the validation completes, approve and deploy the changes: 
    1. From the `Testing Example Corp infrastructure` project, select the **Configurations** tab.
    1. Click **`Example Corp infrastructure with Elasticsearch`** > **View details** to view the last validation and approve the changes.
    1. Add a comment with more details about the approval, and click **Approve**.
1. Click **Deploy** and wait for the deployment to finish.
    
    If the deployment was successful, then you know that `Example Corp's infrastructure` works with Elasticsearch as intended. 
    {: note}
    
1. Finally, undeploy the resources that the architectures created. On the Configurations tab in the project, click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for `Example Corp infrastructure with Elasticsearch` > **Undeploy**.

### Deploy Example Corp's infrastructure with {{site.data.keyword.postgresql}} 
{: #deploy-postgresql}

Complete the same steps to verify that `Example Corp's infrastructure` works with {{site.data.keyword.postgresql}}: 

1. On the Configurations tab in the `Testing Example Corp infrastructure` project, click **Create** and open the `Example Corp catalog` private catalog.
1. Select **Example Corp's infrastructure** to open its catalog listing. 
1. Make sure **0.0.2** is selected as the product version. 
1. Click **Add to project**. 
1. Make sure that **Testing Example Corp infrastructure** is selected as the project. 
1. Change the name of the configuration to `Example Corp infrastructure with {{site.data.keyword.postgresql}}` and click **Next**. 
1. Select **Cloud automation for {{site.data.keyword.databases-for-postgresql}}** to include it and click **Add to project**. 
1. Configure the architecture. Then, validate, approve, and deploy it to verify that `Example Corp's infrastructure` works with {{site.data.keyword.postgresql}} as intended. 
    
    As you configure the architecture, select **us-east** for the `region` input variable to confirm that the architecture can be deployed to both the US south and US east regions. Enter `test-east` as the prefix to indicate which region the resources are deployed to. 
    {: tip}

1. Undeploy the configurations from your project to undeploy resources. 

## Next steps
{: #custom-extend-next}

Continue [onboarding the latest version of `Example Corp's infrastructure`](/docs/secure-enterprise?topic=secure-enterprise-onboard-da). When you're done, a new version of the deployable architecture is available for users. When they add the new version to a project, they can customize it by including Cloud automation for {{site.data.keyword.databases-for-elasticsearch}} or Cloud automation for {{site.data.keyword.databases-for-postgresql}}. 

If a user already deployed the earlier version of `Example Corp's infrastructure`, they are prompted in their project with the needs attention widget to [update the architecture to the latest version](/docs/secure-enterprise?topic=secure-enterprise-update-version-tutorial). 
