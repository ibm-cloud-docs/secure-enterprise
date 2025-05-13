---

copyright:

  years: 2023, 2025

lastupdated: "2025-05-13"

keywords: deployable architecture, basic customization, VSI on VPC landing zone

subcollection: secure-enterprise

content-type: tutorial
account-plan: paid
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}





# Using {{site.data.keyword.cloud_notm}} deployable architectures to build a deployable architecture
{: #basic-custom}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

This tutorial walks you through how to create a new deployable architecture from an existing {{site.data.keyword.cloud}} deployable architecture to fit your business needs. By completing this tutorial, you learn how to download the Terraform files of a deployable architecture, update the variables, and then test the new deployable architecture.
{: shortdesc}

By starting with an {{site.data.keyword.cloud_notm}} deployable architecture, you don't need to worry about creating compliant infrastructure architecture from the ground up. You can get a jump-start by using an {{site.data.keyword.cloud_notm}} deployable architecture and configuring it to meet your specific needs.

Imagine you are the infrastructure architect for the fictitious company _Example Corp_ and you have been given certain infrastructure requirements. You browse the {{site.data.keyword.cloud_notm}} catalog and discover that the [VSI on VPC landing zone](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-vsi-ef663980-4c71-4fac-af4f-4a510a9bcf68-global){: external} deployable architecture meets most of your requirements. However, you want to make the following changes to meet your business needs:

- Remove unwanted variables
- Constrain {{site.data.keyword.cloud_notm}} regions to US deployment regions
- Use an existing SSH key instead of a personal SSH key
- Use a specific Virtual Server Instance profile called `example-profile`

This tutorial uses a fictitious scenario to help you learn and understand a few of the configuration options for a deployable architecture. As you complete the tutorial, adapt each step to match your organization's needs.

## Before you begin
{: #basic-custom-prereqs}

1. To build an architecture, you must have familiarity with Terraform.
1. Verify that you're using a Pay-As-You-Go or Subscription account by going to **Manage** > **Account** > **Account settings** in the {{site.data.keyword.cloud_notm}} console.
1. Create a repository to store your new configuration. For the purposes of this tutorial, GitHub is used. For more information, see [Create a repo](https://docs.github.com/en/repositories/creating-and-managing-repositories/quickstart-for-repositories){: external}.
1. Make sure that you have an editor of your choice set up, for example, [Visual Studio Code](https://code.visualstudio.com/){: external}.
1. Verify that you're assigned the following {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) roles:
      * Administrator on all account management services and all IAM services
      * Editor on the Catalog management service
      * Manager service access role for Schematics
      * Other roles that are required for specific resources in your configured deployable architecture

## Downloading the deployable architecture files
{: #basic-custom-bundle}
{: step}

To begin, you need to download the deployable architecture files. The files include a `main.tf` file that invokes the root Terraform module in VSI on VPC landing zone.

1. In the {{site.data.keyword.cloud_notm}} console, click **Catalog**.
1. Select **Deployable architectures** from the list of **Type** filters > **VSI on VPC landing zone**.
1. Click **Review deployment options** from the summary panel.
1. Select **Work with code** > **Download bundle** to download the bundle.
1. Open the downloaded bundle on your local computer.
1. If needed, extract the `.tar.gz` bundle to access and edit the files.

    After successfully downloading and extracting the bundle, you'll see the following files and folders:

       - automation folder
       - ibm_catalog.json
       - main.tf
       - outputs.tf
       - provider.tf
       - README.md
       - variables.tf
       - version.tf

1. Rename the bundle `Example-corp-architecture` for findability and ease-of-use.

## Editing your list of variables
{: #basic-custom-remove}
{: step}

When you researched VSI on VPC landing zone, you determined that you wanted to modify the region, SSH key, {{site.data.keyword.cloud_notm}} API key, and VSI profile variables. The other variables that are included in the architecture aren't needed for your purposes, and you would like to remove them by using your favorite editor Visual Studio Code.

1. In Visual Studio Code, open the `variables.tf` file.
1. Move the following variables to the start of the `variables.tf` file.
   - `existing_ssh_key_name`
   - `ibmcloud_api_key`
   - `region`
   - `prefix`

   The order of the variables does not matter.
   {: tip}

1. Delete all the other variables and save the file.

## Updating the main.tf file
{: #basic-custom-main}
{: step}

Now that you updated the `variables.tf` file, make sure that the configuration information in the `main.tf` file is correct. Since you want to use an existing SSH key and a specific VSI profile, replace the `ssh_public_key` variable and add the `vsi_instance_profile` variable.

1. Open the `main.tf` file.
1. Replace `ssh_public_key      = var.ssh_public_key` with the following text to indicate that you want users to use an existing SSH key and not a public or personal SSH key.

    ```terraform
   existing_ssh_key_name      = var.existing_ssh_key_name
   ```
{: codeblock}

1. To hardcode the architecture to use the `example-profile` VSI profile, add `vsi_instance_profile     = "example-profile"` as the last variable.
1. Confirm that the `ibmcloud_api_key`, `prefix`, and `region` variables are as shown in the following example.

    ```terraform
    module "deploy-arch-ibm-slz-vsi" {
    source      = "https://cm.globalcatalog.cloud.ibm.com/api/v1-beta/offering/source//patterns/vsi?archive=tgz&flavor=standard&installType=fullstack&kind=terraform&name=deploy-arch-ibm-slz-vsi&version=v4.0.0"
    ibmcloud_api_key      = var.ibmcloud_api_key
    prefix      = var.prefix
    existing_ssh_key_name      = var.existing_ssh_key_name
    region      = var.region
    vsi_instance_profile      = "example-profile"
    }
    ```
    {: codeblock}

1. Save the file.

Your architecture is now hardcoded to use the specific `example-profile` VSI profile and configured to use the variables for {{site.data.keyword.cloud_notm}} API key, prefix, and existing SSH key.


## Updating the ibm_catalog.json file
{: #basic-custom-json}
{: step}

The `ibm_catalog.json` file is a manifest JSON file that is used to automatically import version information when you onboard to a private catalog. Because you changed the deployable architecture, you created a brand new architecture for your needs. Update the following information in the `ibm_catalog.json` file to reflect your changes.

For more information on the manifest file and what it contains, see [Locally editing the catalog manifest](/docs/secure-enterprise?topic=secure-enterprise-manifest-values&locale=en).

### Updating the product information
{: #basic-custom-info}
{: step}

Now that you updated the configuration and created your own architecture, you must also update the name and the programmatic name. For the purposes of this tutorial, update the name to `Example Corps' architecture` and the programmatic name to `deploy-arch-example-corp`.

1. Open the `ibm_catalog.json` file.
1. Find the `label` field and update the name of your deployable architecture to `Example Corps' architecture`.
1. Find the `name` field and update the programmatic name of your architecture to `deploy-arch-example-corp`.
1. Find the `version` field and enter `0.0.1` to update the version number.
1. Save the file.

### Updating the configuration information
{: #basic-custom-config}
{: step}

Now, you want to make sure that users of your new configuration must specify an existing SSH key and can deploy it from only US regions. To do so, update the variable information in the configuration section of the `ibm_catalog.json` file.

1. In the `configuration` section of the `ibm_catalog.json` file, find the following variables and move them to the beginning of the configuration section.
   - `existing_ssh_key_name`
   - `ibmcloud_api_key`
   - `region`
   - `prefix`
1. To require users to specify an `existing_SSH_key`, change the required field to `true`.

1. To restrict the regions to only US regions, you must add each region as an option for the region variable, for example:

    ```terraform
    {
        "key": "region",
        "type": "string",
        "default_value": "__NOT_SET__",
        "description": "Region where VPC is created. To find your VPC region, use `ibmcloud is regions` command to find available regions.",
        "required": true,
        "custom_config": {},
        "options": [
            {
                "displayname": "us-east",
                "value": "us-east"
            },
            {
                "displayname": "us-south",
                "value": "us-south"

            }
        ]
    }
    ```
    {: codeblock}

1. Delete the rest of the variables in the configuration section.

    When users deploy your architecture, they can choose between the region options that you listed. For a list of available regions, see [Regions](/docs/overview?topic=overview-locations#regions).
    {: note}

1. Save the `ibm_catalog.json` file.

## Testing your deployable architecture
{: #basic-custom-test}
{: step}

Before you onboard your configured deployable architecture to a private catalog and make it available for use, test your configuration to ensure that the architecture runs as intended. To test your architecture with the Terraform command line, complete the following steps:

1. Create or update a `.netrc` file that is needed to use Terraform modules from the {{site.data.keyword.cloud_notm}}. For more information, see [ibmcloud catalog utility netrc](/docs/cli?topic=cli-manage-catalogs-plugin#generate-netrc).

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

If your deployable architecture ran as expected, you successfully created your own deployable architecture from VSI on VPC landing zone. You are now ready to move your updated files to the GitHub repository that you created and [onboard your product to a private catalog](/docs/secure-enterprise?topic=secure-enterprise-onboard-da).
