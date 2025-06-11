---

copyright:

  years: 2024, 2025

lastupdated: "2025-06-11"

keywords: stack, configure stack, deployable architecture stack, stacked deployable architecture

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}


# Stacking deployable architectures in a project
{: #config-stack}

You can stack deployable architectures together in a project to create a more complex end-to-end solution architecture. You don't need to code Terraform to connect the deployable architectures together. As you configure input values in a deployable architecture, you can reference inputs or outputs from another architecture to link them together. After you deploy the stacked architectures, you can add them to a private catalog as a deployable architecture to easily share your end-to-end solution with others in your organization.
{: shortdesc}

This is an experimental feature that is available for evaluation and testing purposes and might change without notice.
{: experimental}

In a project, you can stack deployable architectures that are already available to you in a catalog. If you're creating a deployable architecture that isn't onboarded to a catalog already, you can [extend the deployable architecture as you onboard it](/docs/secure-enterprise?topic=secure-enterprise-extend-da) by stacking it with other architectures. That approach provides more customization options that aren't available when you stack deployable architectures in a project, like including optional architectures for different use cases. That approach also works if the deployable architecture isn't available yet in a private catalog. Regardless of the approach you take to stack the architectures together, the result is a more robust deployable architecture that users can configure and deploy by using a project. 

## Before you begin
{: #stack-prereq}

Make sure that you have the following access. For more information about access and permissions, see [Assigning access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).

* The Editor role on the {{site.data.keyword.cloud_notm}} Projects service.
* The Editor and Manager role on the {{site.data.keyword.bplong}} service.
* The Viewer role on the resource group for the project.

Add the deployable architectures that you'd like to stack together to your project. For more information, see [Adding deployable architectures to a project](/docs/secure-enterprise?topic=secure-enterprise-setup-project&interface=ui#add-deployment-project).

When you add deployable architectures to your project, provide meaningful names to help identify them. For example, if you add an infrastructure deployable architecture that creates the base for an application, that infrastructure needs to be deployed first. Otherwise, the application can't deploy onto that infrastructure. Name your infrastructure deployable architecture `1 - infrastructure` when you add it to your project. Name the application `2 - application` to indicate it needs to be deployed second.
{: tip}

## Stacking architectures together by using the CLI
{: #stack-architectures-cli}
{: cli}

After you add the deployable architectures to your project, stack them together by running the following `ibmcloud project config-create` command. In the `Definition` option, specify the `members` by providing a name and the configuration ID for the existing deployable architectures that you want to stack together:

```sh
ibmcloud project config-create --project-id PROJECT-ID [--definition DEFINITION]
```
{: codeblock}

For example, the following command creates a deployable architecture that is named `StackDev` in your project. It contains two deployable architectures, `custom-apache` and `test-slz` that were already added as configurations to the project:

```sh
ibmcloud project config-create \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--definition '{"name": "StackDev", "members": [{"name": "custom-apache", "config_id": "caff3a49-0bf4-40c4-b348-47e5da6e2274"}, {"name": "test-slz", "config_id": "fc7fa3d1-33db-4c40-9570-7604348ab3c4"} ]}' --output json
```

For more information about the command parameters, see [**`ibmcloud project config-create`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-create-command).

## Creating the stack definition by using the CLI
{: #create-stack-definition-cli}
{: cli}

To onboard your deployable architecture to a private catalog, you must create a stack definition. It defines how each deployable architecture relates to each other. Provide this information so users can deploy the entire solution successfully when they add it to a project from the private catalog.

The stack definition contains inputs and outputs that can be referenced in member deployable architectures. You can also include references between deployable architectures, which links them together for users. Inputs that require a specific value or reference to deploy successfully need to be included in the stack definition.

![A diagram of a deployable architecture that was made by stacking two architectures together. Three input values are defined in the stack definition, a prefix, an ssh_key, and an ssh_private_key. The test-slz architecture references the prefix and ssh_key as two of its input values. While the custom-apache architecture references an output from test-slz as one of its inputs, along with the ssh_private_key from the stack definition.](images/apache-stack.svg "References between deployable architectures"){: caption="References between deployable architectures" caption-side="bottom"}

Currently, members can't reference outputs from the stack definition. 
{: note}

Run the following `ibmcloud project stack-definition-create` command to create the stack definition and provide the inputs:

```sh
ibmcloud project stack-definition-create --project-id PROJECT-ID --id
```

Where `id` is the configuration ID of the `StackDev` deployable architecture that you just created in your project.

For example, the following command adds the following three inputs to the stack definition. These inputs are required strings that are not hidden from users, so users must configure these input values to deploy the end-to-end deployable architecture:
* `prefix` input with `stackDemo` as the default value.
* `ssh_key` input with no default value.
* `ssh_private_key` with a default value provided to assist users as they configure the input.

The command also includes input names for the two deployable architectures that are stacked together. These inputs will be populated with values as references and saved for users who add the solution to a project from the private catalog:
* The `test-slz` deployable architecture contains a `prefix` input and an `ssh_key` input.
* The `custom-apache` deployable architecture contains an `ssh_private_key` input and a `prerequisite_workspace_id` input.

For more information about writing references, see [referencing values](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#reference-values).

```sh
ibmcloud project stack-definition-create \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--id 4d69cee6-0fb2-4621-96c6-16d987f3d9d7 \
--stack-definition '{"inputs": [{"name": "prefix", "type": "string", "hidden": false, "required": true, "default": "stackDemo"}, {"name": "ssh_key", "type": "string", "hidden": false, "required": true}, {"name": "ssh_private_key", "type": "string", "hidden": false, "required": true, "default": "<<-EOF\nINSERT YOUR KEY HERE\nEOF"}], "members": [{"name": "test-slz", "inputs": [{"name": "prefix"}, {"name": "ssh_key"}]}, {"name": "custom-apache", "inputs": [{"name": "ssh_private_key"}, {"name": "prerequisite_workspace_id"}]} ]}' --output json
```

For more information about the command parameters, see [**`ibmcloud project stack-definition-create`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli&interface=cli#project-cli-stack-definition-create-command).

## Referencing inputs from the stack definition within member deployable architectures by using the CLI
{: #update-members-cli}
{: cli}

Now that the inputs are added to the stack definition, update the member deployable architectures to reference those inputs by running the `ibmcloud project config-update` command for each architecture that you stacked together:

```sh
ibmcloud project config-update --project-id PROJECT-ID --id
```

For example, the following command updates the `test-slz` deployable architecture to reference the inputs that were added to the stack definition:

```sh
ibmcloud project config-update \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--id fc7fa3d1-33db-4c40-9570-7604348ab3c4 \
--definition '{"inputs": {"prefix": "ref:../../inputs/prefix", "ssh_key": "ref:../../inputs/ssh_key"}}' --output json
```

Since the `custom-apache` architecture uses the `ssh_private_key` value from the stack definition, update the `custom-apache` deployable architecture to reference that value. The `custom-apache` architecture also uses the `schematics_workspace_id` input value as one of its inputs, so include a reference to that value:

```sh
ibmcloud project config-update \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--id caff3a49-0bf4-40c4-b348-47e5da6e2274 \
--definition '{"inputs": {"ssh_private_key": "ref:../../inputs/ssh_private_key", "prerequisite_workspace_id": "ref:../test-slz/outputs/schematics_workspace_id"}}' --output json
```

For more information about the command parameters, see [**`ibmcloud project config-update`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli&interface=cli#project-cli-config-update-command).

## Updating input values in the stack definition by using the CLI
{: #update-stack-values-cli}
{: cli}

Now that the member deployable architectures are configured to reference the values you want, update the input values in the stack definition by running `ibmcloud project config-update` for the `StackDev` deployable architecture. For example, the following command updates the `prefix` input value that is referenced by the `test-slz` deployable architecture. Values are also provided for the `ssh_key` and `ssh_private_key` inputs:

```sh
ibmcloud project config-update  \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--id 4d69cee6-0fb2-4621-96c6-16d987f3d9d7 \
--definition '{"inputs": {"prefix": "kb-stack-0327", "ssh_key": "<publicKey>", "ssh_private_key": "<privateKey>"}}' --output json
```

For more information about the command parameters, see [**`ibmcloud project config-update`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli&interface=cli#project-cli-config-update-command).

Now that the input value is configured, [validate](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#how-to-config) and [deploy](/docs/secure-enterprise?topic=secure-enterprise-deploy-project&interface=ui) each member deployable architecture.

For example, the following command validates the `test-slz` deployable architecture:

```sh
ibmcloud project config-validate \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--id fc7fa3d1-33db-4c40-9570-7604348ab3c4
```

While the following command approves the `test-slz` deployable architecture for deployment:

```sh
ibmcloud project config-approve \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--id fc7fa3d1-33db-4c40-9570-7604348ab3c4 \
--comment 'I approve'
```

And the following command deploys the `test-slz` deployable architecture:

```sh
ibmcloud project config-deploy \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--id fc7fa3d1-33db-4c40-9570-7604348ab3c4
```

## Onboarding to a private catalog by using the CLI
{: #onboard-da-stack-to-catalog}
{: cli}

After each member deployable architecture is validated and deployed, you can onboard your deployable architecture to a private catalog for others to access. When a user adds your deployable architecture to a project from the private catalog, each architecture that you stacked together is included in the project. Run the following `ibmcloud project stack-definition-export` command:

```sh
ibmcloud project stack-definition-export --project-id PROJECT ID
```

You can create a new product or add a version to an existing product. For example, the following command creates a new product in your private catalog named `My Apache Stack`:

```sh
ibmcloud project stack-definition-export --project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 --id 4d69cee6-0fb2-4621-96c6-16d987f3d9d7 --settings '{"catalog_id": "702ff97a-e35a-45a4-a0c0-a04e2e052bc8", "label": "My Apache Stack"}' --output json
```

While the following command creates a new version of an existing product:

```sh
ibmcloud project stack-definition-export \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--id 4d69cee6-0fb2-4621-96c6-16d987f3d9d7 \
--settings '{"catalog_id": "702ff97a-e35a-45a4-a0c0-a04e2e052bc8", "product_id": "1bf57631-27a2-42cc-ac87-733cca67e8a5", "target_version": "1.0.1"}' --output json
```

For more information about the command parameters, see [**`ibmcloud project stack-definition-export`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli&interface=cli#project-cli-stack-definition-export-command).

Your architecture is now a draft in the private catalog that is not yet published, but available to anyone who has Editor access to the private catalog.

To finish onboarding to your private catalog, [edit the catalog details](/docs/secure-enterprise?topic=secure-enterprise-onboard-da&interface=ui#edit-catalog-entry) and provide information like an architecture diagram and a category.

## Stacking architectures together by using the console
{: #stack-architectures-ui}
{: ui}

After you add the deployable architectures to your project, [configure them](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#how-to-config). If the architectures that you're stacking together depend on each other, [link the architectures together by referencing inputs or outputs](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#reference-values) as you configure them. Then, stack the architectures together by completing the following steps:

1. Select the checkbox for the deployable architectures that you want to stack together.
1. Select **Stack**.
1. Provide a name for the deployable architectures or select an existing one. 
   
    The deployable architectures should work together to provide a solution. Consider a meaningful name for the end-to-end solution that accurately represents each architecture that you're stacking together. For example, if you're stacking an Apache application along with an infrastructure base, name it something like `Apache application with infrastructure base` to clearly identify what the architecture deploys. 
    {: remember}
    
1. Click **Continue**.

## Defining variables by using the console
{: #stack-define-variables}
{: ui}

After you stack the deployable architectures together, you need to define variables for your users. Your goal is to link the deployable architectures together so users can configure inputs in a single interface, instead of configuring architectures individually. 

The input variables that you define are configured by users after the deployable architecture is added to a project from a catalog. Similarly, the output variables that you select display for users at the parent level of the architecture. Don't select variables that users shouldn't configure. For example, if your architecture requires a specific value for an input variable, such as a storage plan, don't select the storage plan input. Don't select [references that link the deployable architectures together](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#reference-values). If you do so, the connection between those architectures might break and the entire solution might not successfully deploy.

To make it easier for users to configure, minimize the number of required input values. Review the required inputs for each architecture and make sure that those inputs are configured by adding references to input values in the stack definition, or referencing output values of other architectures.
{: tip}

Complete the following steps:

1. On the **Configurations** tab in your project, click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for the stacked deployable architecture and select **Define variables**.
1. On the **Security** tab, select any variables that users need to configure.
1. Go to the **Required inputs** tab and select any required inputs that users need to configure.
1. Go to the **Optional inputs** tab and select any optional inputs that users need to configure.
1. Go to the **Outputs** tab and select any output variables that you want displayed at the parent level.

    Make it easier for users to find important output values after deploying the architecture, such as application URLs or credential names. Select the important output values from member deployable architectures to display them for users at the parent level.
{: tip}

1. Click **Next** and continue selecting variables for the remaining architectures.
1. When you're done, click **Finish** and configure the architecture for deployment. Any inputs you selected as you defined the variables might need to be configured.

## Onboarding to a private catalog by using the console
{: #onboard-stack-ui}
{: ui}

After you validate and deploy each of the deployable architectures that you stacked together, you can add them as a deployable architecture to a private catalog to easily share the solution with others in your organization. For more information, see [sharing a private catalog](/docs/secure-enterprise?topic=secure-enterprise-catalog-share&interface=ui).

Complete the following steps:

1. On the **Configurations** tab in your project, click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for the deployable architecture and select **Add to private catalog**.
1. Select or create the private catalog that you want to add the deployable architecture to.
1. Select whether it's a new product, or a new version of an existing product.
1. Provide the details like a product name, if applicable, the category, variation, and version.
1. Click **Next**.
1. Review the variables that users can configure after they add deployable architecture to a project from the private catalog. If you need to make any changes, you can [define the variables](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui#stack-define-variables).
1. Click **Add**.

Your deployable architecture is now a draft in the private catalog that is not yet published, but available to anyone who has Editor access to your private catalog. When a user adds your deployable architecture to a project from the private catalog, each architecture that you stacked together is included in the project.

To finish onboarding your deployable architecture to your private catalog, [edit the catalog details](/docs/secure-enterprise?topic=secure-enterprise-onboard-da&interface=ui#edit-catalog-entry) and provide information like an architecture diagram and a category.

Is there a new version available for a deployable architecture that you stacked with others? [Update the configuration in your project to use the latest version](/docs/secure-enterprise?topic=secure-enterprise-needs-attention-projects&interface=ui#na-version-update), validate and deploy the changes, then complete the steps to onboard the updated solution to a private catalog. Select the existing product that you already onboarded and provide a new version number. By doing so, you help ensure that the updated deployable architecture still works properly with the other architectures you stacked along with it. 
{: important}
