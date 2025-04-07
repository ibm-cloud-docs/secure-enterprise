---

copyright:

  years: 2024, 2025

lastupdated: "2025-04-07"

keywords: stack, configure stack, deployable architecture stack, stacked deployable architecture

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}




# Stacking deployable architectures
{: #config-stack}

You can stack deployable architectures together in a project to create a robust end-to-end solution architecture. You don't need to code Terraform to connect the member deployable architectures within the stack. As you configure input values in a member deployable architecture, you can reference inputs or outputs from another member to link the deployable architectures together. After you deploy the deployable architectures in your stack, you can add the stack to a private catalog to easily share it with others in your organization.
{: shortdesc}

This is an experimental feature that is available for evaluation and testing purposes and might change without notice.
{: experimental}

## Before you begin
{: #stack-prereq}

Make sure that you have the following access. For more information about access and permissions, see [Assigning access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).

* The Editor role on the {{site.data.keyword.cloud_notm}} Projects service.
* The Editor and Manager role on the {{site.data.keyword.bplong}} service.
* The Viewer role on the resource group for the project.

Add the deployable architectures that you'd like to stack together to your project. For more information, see [Adding deployable architectures to a project](/docs/secure-enterprise?topic=secure-enterprise-setup-project&interface=ui#add-deployment-project).

When you add deployable architectures to your project, provide meaningful names to help identify them. For example, if your stack contains an infrastructure deployable architecture that creates the base for an application, that infrastructure needs to be deployed first. Otherwise, the application can't deploy onto that infrastructure. Name your infrastructure deployable architecture `1 - infrastructure` when you add it to your project. Name the application `2 - application` to indicate it needs to be deployed second.
{: tip}

## Stacking architectures together by using the CLI
{: #stack-architectures-cli}
{: cli}

After you add the deployable architectures to your project, stack them together by running the following `ibmcloud project config-create` command. In the `Definition` option, specify the `members` by providing a name and the configuration ID for the existing deployable architectures that you want to include in your stack:

```sh
ibmcloud project config-create --project-id PROJECT-ID [--definition DEFINITION]
```
{: codeblock}

For example, the following command creates a deployable architecture stack that is named `StackDev` in your project. The stack contains two deployable architectures, `custom-apache` and `test-slz` that were already added as configurations to the project:

```sh
ibmcloud project config-create \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--definition '{"name": "StackDev", "members": [{"name": "custom-apache", "config_id": "caff3a49-0bf4-40c4-b348-47e5da6e2274"}, {"name": "test-slz", "config_id": "fc7fa3d1-33db-4c40-9570-7604348ab3c4"} ]}' --output json
```

For more information about the command parameters, see [**`ibmcloud project config-create`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-create-command).

## Creating the stack definition by using the CLI
{: #create-stack-definition-cli}
{: cli}

To onboard your deployable architecture stack to a private catalog, you must create a stack definition. It defines how the member deployable architectures within the stack relate to each other. Provide this information so users can deploy the entire stack successfully when they add it to a project from the private catalog.

The stack definition contains inputs and outputs at the stack level that can be referenced in the member deployable architectures within the stack. You can also include references between members of the stack, which links the member deployable architectures together for users. Inputs that require a specific value or reference to deploy the stack successfully need to be included in the stack definition.

![A diagram of a deployable architecture stack. Three input values are defined at the stack level, a prefix, an ssh_key, and an ssh_private_key. The test-slz architecture within the stack references the prefix and ssh_key as two of its input values. While the custom-apache architecture within the stack references an output from test-slz as one of its inputs, along with the ssh_private_key from the stack level.](images/apache-stack.svg "A deployable architecture stack with references"){: caption="A deployable architecture stack with references" caption-side="bottom"}

Currently, members can't reference outputs from the stack level.
{: note}

Run the following `ibmcloud project stack-definition-create` command to create the stack definition and provide the inputs:

```sh
ibmcloud project stack-definition-create --project-id PROJECT-ID --id
```

Where `id` is the configuration ID of the `StackDev` deployable architecture stack that you just added to your project.

For example, the following command adds the following three inputs at the stack level. These inputs are required strings that are not hidden from users, so users must configure these input values to deploy the stack:
* `prefix` input with `stackDemo` as the default value.
* `ssh_key` input with no default value.
* `ssh_private_key` with a default value provided to assist users as they configure the input.

The command also includes input names for the two members of the stack. These inputs will be populated with values as references and saved for users who add the stack to a project from the private catalog:
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

## Referencing inputs from the stack level within member deployable architectures by using the CLI
{: #update-members-cli}
{: cli}

Now that the inputs are added at the stack level, update the member deployable architectures in the stack to reference those inputs by running the `ibmcloud project config-update` command for each member of the stack:

```sh
ibmcloud project config-update --project-id PROJECT-ID --id
```

For example, the following command updates the `test-slz` deployable architecture to reference the inputs that were added at the stack level:

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

## Updating input values at the stack level by using the CLI
{: #update-stack-values-cli}
{: cli}

Now that the member deployable architectures are configured to reference the values you want, update the input values at the stack level by running `ibmcloud project config-update` for the `StackDev` deployable architecture stack. For example, the following command updates the `prefix` input value that is referenced by the `test-slz` deployable architecture within the stack. Values are also provided for the `ssh_key` and `ssh_private_key` inputs:

```sh
ibmcloud project config-update  \
--project-id 0e13c360-45c4-4b68-a53f-bb8f6ac04161 \
--id 4d69cee6-0fb2-4621-96c6-16d987f3d9d7 \
--definition '{"inputs": {"prefix": "kb-stack-0327", "ssh_key": "<publicKey>", "ssh_private_key": "<privateKey>"}}' --output json
```

For more information about the command parameters, see [**`ibmcloud project config-update`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli&interface=cli#project-cli-config-update-command).

Now that the input value is configured at the stack level, [validate](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#how-to-config) and [deploy](/docs/secure-enterprise?topic=secure-enterprise-deploy-project&interface=ui) each member deployable architecture within the stack.

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

## Onboarding a deployable architecture stack to a private catalog by using the CLI
{: #onboard-da-stack-to-catalog}
{: cli}

After each member deployable architecture in your stack is validated and deployed, you can onboard your stack to a private catalog for others to access. Run the following `ibmcloud project stack-definition-export` command:

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

Your stack is now a draft in the private catalog that is not yet published, but available to anyone who has Editor access to the private catalog.

To finish onboarding your deployable architecture stack to your private catalog, [edit the catalog details](/docs/secure-enterprise?topic=secure-enterprise-onboard-da&interface=ui#edit-catalog-entry) and provide information like an architecture diagram and a category.

## Stacking architectures together by using the console
{: #stack-architectures-ui}
{: ui}

After you add and [configure the deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#how-to-config) to your project, stack them together by completing the following steps:

1. Select the checkbox for the deployable architectures that you want to stack.
1. Select **Stack**.
1. Provide a name for the new stack, or select an existing stack.
1. Click **Continue**.

## Defining stack variables by using the console
{: #stack-define-variables}
{: ui}

The input variables that you define for the stack are configured by users after the stack is added to a project from a catalog. Similarly, the output variables that you select display for users at the stack level. Don't select variables that users shouldn't configure. For example, if your stack requires a specific value for an input variable, such as a storage plan, don't select the storage plan input. Don't select [references that link the deployable architectures together](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#reference-values) within your stack. If you do so, the connection between those architectures might break and the stack might not successfully deploy.

To make it easier for users to configure a deployable architecture stack, minimize the number of required input values that users must configure to deploy the architectures. Review the required inputs for each architecture within the stack and make sure that those inputs are configured by adding references to stack level input values, or referencing output values of other architectures.
{: tip}

Complete the following steps:

1. On the **Configurations** tab in your project, click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for the stack and select **Define variables**.
1. On the **Security** tab, select any variables that users need to configure.
1. Go to the **Required inputs** tab and select any required inputs that users need to configure.
1. Go to the **Optional inputs** tab and select any optional inputs that users need to configure.
1. Go to the **Outputs** tab and select any output variables that you want displayed at the stack level.

    Make it easier for users of the stack to find important output values after deploying the stack, such as application URLs or credential names. Select the important output values from member deployable architectures to display them for users at the stack level.
{: tip}

1. Click **Next** and continue selecting variables for the remaining architectures in your stack.
1. When you're done, click **Finish** and configure the stack for deployment.

## Onboarding a deployable architecture stack to a private catalog by using the console
{: #onboard-stack-ui}
{: ui}

After you validate and deploy each of the deployable architectures in your stack, you can add the stack to a private catalog to easily share it with others in your organization. For more information, see [deploying an architecture](/docs/secure-enterprise?topic=secure-enterprise-deploy-project&interface=ui).

Complete the following steps:

1. On the **Configurations** tab in your project, click the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") for the stack and select **Add to private catalog**.
1. Select or create the private catalog that you want to add the deployable architecture stack to.
1. Select whether this stack is a new product, or a new version of an existing product.
1. Provide the details like a product name, if applicable, the category, variation, and version.
1. Click **Next**.
1. Review the variables that users can configure after they add the stack to a project from the private catalog. If you need to make any changes, you can [define the stack variables](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui#stack-define-variables).
1. Click **Add**.

Your stack is now a draft in the private catalog that is not yet published, but available to anyone who has Editor access to your private catalog.

To finish onboarding your deployable architecture stack to your private catalog, [edit the catalog details](/docs/secure-enterprise?topic=secure-enterprise-onboard-da&interface=ui#edit-catalog-entry) and provide information like an architecture diagram and a category. 
