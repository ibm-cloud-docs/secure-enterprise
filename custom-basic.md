---

copyright:

  years: 2023, 2025

lastupdated: "2025-07-09"

keywords: deployable architecture, basic customization, Code Engine, AI, application, infrastructure

subcollection: secure-enterprise

content-type: tutorial
account-plan: paid
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}

# Customizing an {{site.data.keyword.cloud_notm}} deployable architecture to build your own infrastructure
{: #basic-custom}
{: toc-content-type="tutorial"}
{: toc-completion-time="15m"}

This tutorial walks you through creating a customized deployable architecture based on an existing {{site.data.keyword.cloud}} deployable architecture to meet your business needs. By completing this tutorial, you learn how to download the Terraform files, update the variables, and then test the updated architecture. 
{: shortdesc}

By starting with an {{site.data.keyword.cloud_notm}} deployable architecture, you don't need to worry about creating infrastructure architecture from the ground up. You can get a jump-start by using an {{site.data.keyword.cloud_notm}} deployable architecture and configuring it to meet your specific needs.

Imagine you are a cloud automation engineering professional for *Example Corp*, a fictitious company. Your infrastructure architect browsed the {{site.data.keyword.cloud_notm}} catalog and discovered [Cloud automation for {{site.data.keyword.codeengineshort}}](https://cloud.ibm.com/catalog/7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3/architecture/deploy-arch-ibm-code-engine-413843d9-8962-48a5-8ab5-dfcf4429372c-global){: external}, a deployable architecture that meets most of your requirements. However, your infrastructure architect needs you to make the following changes to meet your business needs:

- Remove unwanted variables.
- Constrain {{site.data.keyword.cloud_notm}} regions to US deployment regions.
- Update the deployable architecture to reference Example Corps' AI application. 

This tutorial uses a fictitious scenario to help you learn and understand a few of the configuration options for a deployable architecture. It explains how to customize a deployable architecture to automate the deployment of a containerized application on {{site.data.keyword.codeenginefull_notm}}. The existing container image at `icr.io/codeengine/helloworld` is used as an example application. As you complete the tutorial, adapt each step to match your organization's needs.

## Before you begin
{: #basic-custom-prereqs}

1. To build an architecture, you must have familiarity with [Terraform](https://developer.hashicorp.com/terraform){: external}.
1. Verify that you're using a Pay-As-You-Go or Subscription account by going to **Manage** > **Account** > **Account settings** in the {{site.data.keyword.cloud_notm}} console.
1. Create a repository to store your deployable architecture. For the purposes of this tutorial, GitHub is used. For more information, see [Create a repo](https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories){: external}.
1. Install an editor of your choice, for example, [Visual Studio Code](https://code.visualstudio.com/){: external}.
1. Verify that you're assigned the following {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) roles:
      * Administrator on All IAM Account Management services, All Account Management services, and All Identity and Access enabled services. 
      * Editor on the Catalog Management service.
      * Manager service access role for {{site.data.keyword.bpshort}}.
      * Other roles that are required for specific resources in your deployable architecture. Cloud automation for {{site.data.keyword.codeengineshort}} requires the Writer service access role that is scoped to all resources for the {{site.data.keyword.codeengineshort}} service. 

      For more information, go to [Assigning access to account management services](/docs/account?topic=account-account-services) and [Managing access to resources](/docs/account?topic=account-assign-access-resources).

## Downloading the deployable architecture files
{: #basic-custom-bundle}
{: step}

To begin, you need to download the deployable architecture files. The files include a `main.tf` file that invokes the root Terraform module.

1. In the {{site.data.keyword.cloud_notm}} console, click **Catalog**.
1. Enter `Cloud automation for {{site.data.keyword.codeengineshort}}` into the search bar and select the architecture from the list of search results.
1. Select **v4.2.2** as the product version. 
    
    This tutorial was created based on version 4.2.2 of Cloud automation for {{site.data.keyword.codeengineshort}}. However, you can select another version of the deployable architecture if you want to. 
    {: note}
    
1. Make sure the **New {{site.data.keyword.codeengineshort}} apps** variation is selected. 
1. Click **Review deployment options** from the summary panel.
1. Select **Work with code** > **Download bundle** to download the bundle.
1. Open the downloaded bundle on your local computer.
1. Extract the `.tar.gz` bundle to access and edit the files within a folder. Renamed the extracted folder `Example-corp-infrastructure` for findability and ease-of-use.

    After successfully downloading and extracting the bundle, you'll see the following files and folders:

    - `automation folder`
    - `ibm_catalog.json`
    - `main.tf`
    - `outputs.tf`
    - `provider.tf`
    - `README.md`
    - `variables.tf`
    - `version.tf`

## Editing your list of variables
{: #basic-custom-remove}
{: step}

When your infrastructure architect researched {{site.data.keyword.codeenginefull_notm}}, they decided to modify the region and {{site.data.keyword.cloud_notm}} API key. The other variables that are included in the architecture aren't needed for your purposes, and so you need to remove them by using your favorite editor Visual Studio Code.

1. In Visual Studio Code, open the `variables.tf` file.
1. Move the following variables to the start of the `variables.tf` file.
   - `ibmcloud_api_key`
   - `prefix`
   - `existing_resource_group_name` 
   - `region`

   The order of the variables does not matter.
   {: tip}

1. Delete all the other variables and save the file.

## Updating the main.tf file
{: #basic-custom-main}
{: step}

Now that you updated the `variables.tf` file, make sure that the configuration information in the `main.tf` file is correct. Since you want to use {{site.data.keyword.codeengineshort}} to run an application that *Example Corp* created, you need to provide information about your application.

1. Open the `main.tf` file.
1. Update the `app_name` variable and enter `"example-corp-ai-app"` as the name of the application. 
1. Update the `image_reference` variable and enter `"icr.io/codeengine/helloworld"`, which is the docker image for your application. 
1. Add the `project_name` variable and enter `"example-corp-ce-project"` as the name for your {{site.data.keyword.codeengineshort}} project. 
1. Add the `provider_visibility` variable and set it to `"public"`. 
1. Confirm that the `ibmcloud_api_key`, `prefix`, and `region` variables are included as shown in the following example:

    ```terraform
    module "deploy-arch-ibm-code-engine" {
      source      = "https://cm.globalcatalog.cloud.ibm.com/api/v1-beta/offering/source/archive//solutions/apps?archive=tgz&catalogID=7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3&flavor=apps&installType=fullstack&kind=terraform&name=deploy-arch-ibm-code-engine&version=v4.2.2"
      ibmcloud_api_key      = var.ibmcloud_api_key
      prefix      = var.prefix
      existing_resource_group_name      = var.existing_resource_group_name
      region      = var.region
      app_name      = "example-corp-ai-app"
      image_reference      = "icr.io/codeengine/helloworld"
      project_name      = "example-corp-ce-project"
      provider_visibility      = "public"
    }
    ```
    {: codeblock}
    {: pre}

1. Save the file.

Your architecture is now hardcoded to reference your Example Corp AI app and configured to use the variables for {{site.data.keyword.cloud_notm}} API key, prefix, and region.

## Updating the ibm_catalog.json file
{: #basic-custom-json}
{: step}

The `ibm_catalog.json` file is a manifest JSON file that is used to automatically import version information when you onboard a deployable architecture to a private catalog. Because you changed the deployable architecture, you created a brand-new architecture for your needs. Update the following information in the `ibm_catalog.json` file to reflect your changes.

For more information on the manifest file and what it contains, see [Locally editing the catalog manifest](/docs/secure-enterprise?topic=secure-enterprise-manifest-values&locale=en).

### Updating the product information
{: #basic-custom-info}
{: step}

Now that you updated the configuration and created your own architecture, you must also update the name and the programmatic name of the deployable architecture.

1. Open the `ibm_catalog.json` file.
1. Find the `label` field and update the name of your deployable architecture to `Example Corps' infrastructure`.
1. Find the `name` field and update the programmatic name of your architecture to `deploy-arch-example-corp`.
1. Find the `version` field and enter `0.0.1` to update the version number. If the field is missing, add it on a new line immediately following the `name` field, for example: 

    ```json
			"label": "Example Corps' infrastructure",
			"name": "deploy-arch-example-corp",
			"version": "0.0.1",
    ```
    {: codeblock}
    {: pre}

1. Within the `flavors` section, find the `label` field and update the name for this variation of your deployable architecture to `Deploy Example Corp AI app on {{site.data.keyword.codeenginefull_notm}}`. By doing so, you identify the purpose of this variation of your deployable architecture and what it accomplishes. 
1. Save the file.

### Updating the configuration information
{: #basic-custom-config}
{: step}

Now, you want to make sure that users of your deployable architecture can deploy it in US regions only. To do so, update the variable information in the configuration section of the `ibm_catalog.json` file.

1. In the `configuration` section of the `ibm_catalog.json` file, find the following variables and move them to the beginning of the configuration section:
   - `ibmcloud_api_key`
   -  `prefix`
   - `existing_resource_group_name`
   - `region`


1. To restrict the regions to only US regions, you must add each region as an option for the `region` variable, delete unneeded sections, and set the `region` as a required variable.

    ```json
    {
        "key": "region",
        "type": "string",
        "default_value": "us-south",
        "description": "The region in which to provision all resources created by this solution.",
        "required": true,
        "options": [
            {
                "displayname": "us-east",
                "value": "us-east"
            },
            {
                "displayname": "us-south",
                "value": "us-south"

            }
        ],
        "virtual": false
    }
    ```
    {: codeblock}
    {: pre}

1. Delete the rest of the variables in the configuration section.

    When users deploy your architecture, they can choose between the region options that you listed. For a list of available regions, see [Regions](/docs/overview?topic=overview-locations#regions).
    {: note}

1. Save the `ibm_catalog.json` file.

## Testing your deployable architecture
{: #basic-custom-test}
{: step}

Before you onboard your configured deployable architecture to a private catalog and make it available for use, test your configuration to help ensure that the architecture runs as intended. To test your architecture with the Terraform command line, complete the following steps:

1. Create or update a `.netrc` file that is needed to use Terraform modules from {{site.data.keyword.cloud_notm}}. For more information, see [ibmcloud catalog utility netrc](/docs/cli?topic=cli-manage-catalogs-plugin#generate-netrc).

   ```bash
   ibmcloud catalog utility netrc
   ```
   {: codeblock}

1. Initialize the Terraform CLI. For more information, see [Initializing Working Directories](https://developer.hashicorp.com/terraform/cli/init){: external}.

   ```terraform
   terraform init
   ```
   {: pre}

1. Provision the resources. For more information, see [Provisioning Infrastructure with Terraform](https://developer.hashicorp.com/terraform/cli/run){: external}.

   1. Run `terraform plan` to generate a Terraform execution plan to preview the proposed actions.

      ```terraform
      terraform plan
      ```
      {: pre}

   1. Run `terraform apply` to create the resources that are defined in the plan.

      ```terraform
      terraform apply
      ```
      {: pre}


## Next steps
{: #basic-custom-next}

If your deployable architecture ran as expected, you successfully created your own deployable architecture from Cloud automation for {{site.data.keyword.codeengineshort}}. You are now ready to move your updated files to the GitHub repository that you created and [onboard your product to a private catalog](/docs/secure-enterprise?topic=secure-enterprise-onboard-da). 

After you onboard `Example Corps' infrastructure` to a private catalog, you can [share it with your enterprise](/docs/secure-enterprise?topic=secure-enterprise-share-custom). If you're not ready to share your deployable architecture, or you're not a part of an enterprise, you can skip sharing it and continue on to the next tutorial, [deploying `Example Corps' infrastructure` by using a project](/docs/secure-enterprise?topic=secure-enterprise-deploy-regions). 
