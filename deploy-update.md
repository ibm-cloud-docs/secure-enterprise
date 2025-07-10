---

copyright:

  years: 2023, 2025

lastupdated: "2025-07-10"

keywords: multiple regions, deploy, projects

subcollection: secure-enterprise

content-type: tutorial
account-plan: paid
completion-time: 20m

---

{{site.data.keyword.attribute-definition-list}}

# Deploying a new version of a deployable architecture
{: #update-version-tutorial}
{: toc-content-type="tutorial"}
{: toc-completion-time="20m"}

This tutorial walks you through updating a deployable architecture configuration in a project to a new version with minimal downtime. When a new version of a deployable architecture is made available, you don't need to undeploy anything. You can edit the deployable architecture configuration in your project to use the new version, configure any new inputs, and then deploy the changes. 
{: shortdesc}

Imagine you are a software developer for *Example Corp* enterprise. Your infrastructure architect discovered the Cloud automation for {{site.data.keyword.codeengineshort}} deployable architecture and your cloud automation engineering professional customized it to fully meet your business needs. You deployed that customized architecture to two regions. However, that deployable architecture is now updated to a new version in your private catalog. You want to take advantage of the latest version of the deployable architecture, so you need to update your project and deploy the changes. 

This tutorial uses a fictitious scenario to help you understand how to update a deployable architecture configuration in your project when a new version of the architecture is made available to you. As you complete the tutorial, adapt each step to match your organization's needs. 

## Before you begin
{: #update-prereq}

1. [Create a customized deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-basic-custom) called `Example Corp's infrastructure` and onboard it to a private catalog called `Example Corp catalog`. 
1. [Deploy Example Corp's infrastructure to two regions](/docs/secure-enterprise?topic=secure-enterprise-deploy-regions) by using a project called `Example Corp infrastructure`. 
1. [Update Example Corp's infrastructure with a new version in the private catalog](/docs/secure-enterprise?topic=secure-enterprise-custom-extend) and add two database options that your users can choose from. 
1. Understand that completing this tutorial might result in costs to your account. [Cloud automation for {{site.data.keyword.codeengineshort}}](https://cloud.ibm.com/catalog/7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3/architecture/deploy-arch-ibm-code-engine-413843d9-8962-48a5-8ab5-dfcf4429372c-global){: external} was customized to create `Example Corp's infrastructure`. For more information about associated costs for using {{site.data.keyword.codeengineshort}}, go to [Pricing for {{site.data.keyword.codeengineshort}}](/docs/codeengine?topic=codeengine-pricing). The latest version of `Example Corp's infrastructure` includes two database options that are deployed as a part of this tutorial. The Summary panel in the catalog listing includes an estimated cost for deploying the architectures: 
    1. [Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}](https://cloud.ibm.com/catalog/7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3/architecture/deploy-arch-ibm-icd-elasticsearch-7ee5876d-6e30-49d1-be25-259a442085e8-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2Fc2VhcmNoPUNsb3VkJTI1MjBhdXRvbWF0aW9uJTI1MjBmb3IlMjUyMGRhdGFiYXNlcyUyNTIwZm9yJTI1MjBlbGFzdGljc2VhcmNoI3NlYXJjaF9yZXN1bHRz){: external} catalog listing. 
    1. [Cloud automation for {{site.data.keyword.databases-for-postgresql}}](https://cloud.ibm.com/catalog/7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3/architecture/deploy-arch-ibm-icd-postgresql-0298facd-3e69-43fa-87c0-4d3d0b3c887e-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2Fc2VhcmNoPUNsb3VkJTI1MjBhdXRvbWF0aW9uJTI1MjBmb3IlMjUyMGRhdGFiYXNlcyUyNTIwZm9yJTI1MjBlbGFzdGljc2VhcmNoI3NlYXJjaF9yZXN1bHRz){: external} catalog listing. 

Now, you're ready to update the configurations in your `Example Corp infrastructure` project so you can deploy the latest version of `Example Corp's infrastructure` to the two regions that you previously deployed to. 

## Updating configurations in your project 
{: #update-project}
{: step}

First, update the configurations in your project to use the latest version of `Example Corp's infrastructure` that was recently made available to you. Complete the following steps: 

1. In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **[Projects](/projects/)**. 
1. Click **Example Corp infrastructure** to open the project. 
1. Click **Configurations**. 
1. Click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for `example-corp-us-south` > **Edit**.
1. In the **Details** section, the **Version** menu displays an alert that a new version of `Example Corp's infrastructure` is available. Select **0.0.2** from the menu. 
1. Click **Change notices** to view information about the latest version. Close the panel when you're done. 
1. Click **Save**. 

Repeat the steps but this time, edit `example-corp-us-east` to use version 0.0.2 of the deployable architecture. 

## Selecting a database option to use for each deployment
{: #select-database}
{: step}

Based on the change notice, you know that version 0.0.2 of `Example Corp's infrastructure` includes two optional databases that you can choose between. Now that the deployable architecture configurations are updated to use the latest version from your private catalog, select which database option you want to use with each configuration. Complete the following steps: 

1. From the `Example Corp infrastructure` project, select **Configurations**.
1. Click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for `example-corp-us-south` > **Customize architecture**.
1. Select **Cloud automation for {{site.data.keyword.databases-for-postgresql}}** to add it to your project and click **Save**. 
1. Click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for `example-corp-us-south` > **Validate**.

   If the validation fails, you can [troubleshoot the failure](/docs/secure-enterprise?topic=secure-enterprise-ts-na-failures). Or, an administrator on the {{site.data.keyword.cloud_notm}} Projects service can review the results through the {{site.data.keyword.bpshort}} service and [override the failure and approve](/docs/secure-enterprise?topic=secure-enterprise-approve-failed-validation) the configuration to deploy anyway. However, make sure that the pipeline failed due to the Code Risk Analyzer scan and not because of a validation or plan failure. It is not recommended to override a failure that is flagged due to a validation or plan failure as the configuration cannot deploy successfully. For more information about security and compliance in projects, see [Achieving continuous compliance as an enterprise](/docs/secure-enterprise?topic=secure-enterprise-continuous-compliance).
   
1. Repeat the steps for `example-corp-us-east`, but this time, select **Cloud automation for {{site.data.keyword.databases-for-elasticsearch}}** as the database to add to your project. 

During the configuration and deployment process, monitor your [**Needs attention** items](/docs/secure-enterprise?topic=secure-enterprise-needs-attention-projects). The widget reflects any issue that occurs in your configurations.
{: remember}

## Approving and deploying changes
{: #approve-deploy-changes}
{: step}

It can be beneficial to deploy your first configuration to make sure that your changes work as expected. Then, if the deployment is successful, you can continue to deploy your second configuration.

You must address any outstanding **Needs attention** items on the **Overview** tab before you can approve and deploy your configurations.
{: tip}

1. From the `Example Corp infrastructure` project, select **Configurations**.
1. Click **`example-corp-us-south`** > **View details** to view the last validation and approve the changes.
1. Add a comment with more details about the approval, and click **Approve**.
1. Click **Deploy** and wait for the deployment to finish.
1. Repeat the steps, but this time, approve and deploy `example-corp-us-east`.

## Next steps
{: #deploy-update-next}

Your running deployments are now updated to use the latest version of your deployable architecture. If you need to revert the deployments to the previous version, you can do so by using the CLI. For more information, go to [rolling back an update](/docs/secure-enterprise?topic=secure-enterprise-undeploy-tutorial). 
