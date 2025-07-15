---

copyright:

  years: 2023, 2025

lastupdated: "2025-07-15"

keywords: manage project, destroy resources, remove resources, resource, resources, delete resources, Terraform resources, Terraform workspace, undeploy configurations

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Undeploying resources
{: #remove-resources}



Deploying an architecture creates resources that can incur costs in the target account that you deployed to. Undeploying the resources that were created destroys the resources and removes them from that target account, and can help reduce costs for your business.
{: shortdesc}

Resources that were deployed by a configuration in your project can be undeployed following the steps here. Resources that were added to a project for organization and tracking purposes can be removed from the project, but they can't be destroyed in their target accounts by using a project. For more information, see [Organizing existing resources by using a project](/docs/secure-enterprise?topic=secure-enterprise-organize-resources).
{: important}

## Undeploying resources from a project
{: #delete-resource-without-project}

You can undeploy resources without deleting the project or configuration that deployed those resources. Doing so can be useful if you no longer need the resources in the target account that you deployed to, but want to keep the project or configuration to use again for future deployments. After you undeploy the resources, the configuration returns to a draft status. Complete the following steps:

1. In the {{site.data.keyword.cloud}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **Projects**.
1. Select the project that contains the configuration that deployed the resources that you want to remove.
1. On the **Configurations** tab, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Undeploy** for the configuration.
1. Enter the name of the configuration into the required field.

    Make sure that you want to undeploy the resources, as this action can't be undone. While you can re-create the resources based on the current configuration, any cached data or persisted data is destroyed.
    {: important}

1. Click **Undeploy**.

## Deleting a project and all of its deployed resources
{: #delete-and-remove-resources}

By default, when you delete a project or a configuration, any resources that were deployed are undeployed automatically. Confirm that this setting is enabled by opening your project and going to **Manage** > **Settings** before you delete your project or configuration. If this setting is disabled, when you delete your project or configuration, the resources remain deployed, but you lose the ability to manage them easily with your project. Deployed resources continue accruing costs in your target account.
{: important}

If you no longer need a project and want to remove any resources that were deployed by configurations in your project, delete the project and undeploy any Terraform resources that were created. Complete the following steps:

1. In the {{site.data.keyword.cloud}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **Projects**.
1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Delete project** for the project that you want to delete.
1. Enter the project name into the required field.

    Make sure that you want to delete the project, as this action can't be undone.
    {: important}

1. Click **Delete**.

## Deleting a configuration and all of its deployed resources
{: #delete-and-remove-config}

By default, when you delete a project or a configuration, any resources that were deployed are undeployed automatically. Confirm that this setting is enabled by opening your project and going to **Manage** > **Settings** before you delete your project or configuration. If this setting is disabled, when you delete your project or configuration, the resources remain deployed, but you lose the ability to manage them easily with your project. Deployed resources continue accruing costs in your target account.
{: important}

If you no longer need a configuration and want to remove any resources that were deployed by it, delete the configuration and undeploy any resources that were created. Complete the following steps:

1. In the {{site.data.keyword.cloud}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **Projects**.
1. Select the project that contains the configuration that you want to delete.
1. On the **Configurations** tab, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Delete** for the configuration that you want to delete.
1. Enter the name of the configuration into the required field.

    Make sure that you want to delete the configuration, as this action can't be undone.
    {: important}

1. Click **Delete**.
