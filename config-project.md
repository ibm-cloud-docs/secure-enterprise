---

copyright:

  years: 2023, 2025

lastupdated: "2025-05-27"

keywords: manage project, rename project, move project, deploy project, merge request, merge changes, deploy configuration

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}


# Configuring a deployable architecture
{: #config-project}

After you add a deployable architecture to your project, you can edit the input values to configure the architecture for deployment.
{: shortdesc}

Configurations can be generic, but many projects use a configuration, or a group of configurations, to deploy resources to different environments. For example, a group of configurations can be used to deploy resources to development, test, and production environments and set up common services outside of the environments. When you deploy your configuration, {{site.data.keyword.bplong}} uses Terraform to apply the underlying plan. 

Before you can deploy your architecture, the inputs, plan, compliance, and estimated cost for the deployable architecture must be validated. Any changes that are made to the configuration are validated to help ensure that there aren't any issues or failures.
{: note}



## Setting input values
{: #project-input-values}

Input values are used to configure a deployable architecture to match your specific needs. The required inputs vary based on the deployable architecture that you choose. Depending on how the architecture was designed, some inputs might include a set of options that you can select, or you can enter values in fields as text strings.

### Referencing values
{: #reference-values}

Configurations can be linked together by using the outputs of one configuration as the inputs in another. For example, a configuration for an application could use an output from an infrastructure configuration, such as a cluster ID, to deploy onto that infrastructure. To achieve this, you can add a reference to an input or an output from another configuration. You can also reference parameters from an environment. When you add a reference, the value is pulled from the input, output, or environment and used as the input value in the architecture that you're configuring.

In the console, you can add a reference in an architecture that you're configuring by hovering over an input and selecting the **Reference** icon ![Reference icon](../icons/link.svg "Reference").
{: tip}

If you are using the API or CLI to configure a deployable architecture, or if you are [editing a deployable architecture stack definition](/docs/secure-enterprise?topic=secure-enterprise-stack-definition) and you want to include a reference, you can write one as a text string. References comply with the URL specification, but use a different `ref` protocol instead of `http`. Just like URLs on websites, you can write a reference that's relative to your current context. For example, if you're adding a reference to an input within the configuration that you're currently editing, then your current path is `/configs/<configname>` and you can write a reference relative to that path. For example, `ref:./inputs/region` adds a reference to the input that is named `region` within the same configuration. In this case, the configuration that you're editing does not need to be deployed to reference another value within it.

You can find the name of an output to reference by opening a deployed configuration in your project and going to the **Outputs** tab.
{: tip}

#### Referencing values from a configuration
{: #reference-values-config}

The general format to reference a value in a configuration is as follows:

`ref:/configs/<config_name>/inputs_or_outputs/<input_or_output_name>`.

You can reference an input or an output from a configuration that was deployed from your project. For example, the following reference points to an output that is named `cluster_id` within the `ProdCluster` configuration: `ref:/configs/ProdCluster/outputs/cluster_id`.

You can add a relative reference to another input within the configuration that you're currently editing. The configuration does not need to be deployed to do so.
{: remember}

[Experimental]{: tag-purple}

If you stacked configurations together in your project, you can reference outputs between the configurations that were stacked together. The general format to reference a value is as follows:{: #reference-values-stack}

`ref:/configs/<stack_name>/members/<member_name>/inputs_or_outputs/<input_or_output_name>`

If you want to make a relative reference, you can do so. A relative reference between configurations that are stacked together would be formatted as `ref:../<member_name>/inputs_or_outputs/<input_or_output_name>`. But, if you are referencing a value in the stack definition, it would be formatted as `ref:../../inputs/<input_name>` within the member configuration. Currently, members can't reference outputs from the stack definition.


#### Referencing inputs from an environment
{: #reference-parameters-env}

Since environments are created within a project, and not within a configuration, you don't need to include `/configs/<configname>` if you want to reference a parameter in an environment. But you must include the name of the environment after the `environments` reference type. Then, specify `inputs` and provide the name of the input that you want to reference: `ref:./environments/<environment_name>/inputs/<name>`. You can't add a reference to an authentication parameter or a compliance profile from an environment.

For example, the following reference points to an input parameter that is named `cluster_id` within the `Production` environment: `ref:./environments/Production/inputs/cluster_id`.

## Configuring an architecture by using the console
{: #how-to-config}
{: ui}

To create a customized configuration, complete the following steps:

1. From the **Security** panel, select the authentication method that you want to use to deploy your architecture.

   Add an API key by using {{site.data.keyword.secrets-manager_full}}. This authorizes the project to deploy to a target account and is required to deploy your architecture. For more information, see [Using an API key with Secrets Manager to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).
   {: note}
   {: #cra-validate-failure}

1. During validation, a Code Risk Analyzer scan is run on your architecture. Select the controls that you want to use during validation. You can use the **Architecture default** controls, or the **Select from {{site.data.keyword.compliance_short}}** option if you have an attachment set up in your target account.

    If you select **Architecture default**:
    * The scan uses the default controls that the owner of the deployable architecture added when they onboarded it.
    * Controls that the architecture owner added that are also included in the [supported set of {{site.data.keyword.compliance_short}} rules](/docs/ContinuousDelivery?topic=ContinuousDelivery-cra-cli-plugin#terraform-scc-rules) are checked.
    * Any extra controls that the architecture owner added that are not included in the list of supported rules are not checked when you validate your configuration.
    * If the owner of the deployable architecture didn't add compliance controls to their product, the full set of {{site.data.keyword.compliance_short}} rules is used.

    To view the list of added controls, go to the [{{site.data.keyword.cloud}} catalog](/catalog){: external} and select the deployable architecture that you're configuring. The Security & compliance tab lists all of the controls that were added to the deployable architecture.
    {: tip}

    If you select **Select from {{site.data.keyword.compliance_short}}**, you must have an instance of the service and an attachment through {{site.data.keyword.compliance_short}} in the target account that you want to deploy to. For help with creating an attachment, see [Evaluating resource configuration with {{site.data.keyword.compliance_long}}](/docs/secure-enterprise?topic=secure-enterprise-security-compliance-scanning).

1. From the **Configure architecture** panel, enter values for the required inputs for the deployable architecture configuration. 
    
    Depending on the deployable architecture, some inputs might be configured for you with references to other architectures or fixed values. These values were provided by the creator of the deployable architecture. Be careful if you edit these values, especially references, as the architecture might not successfully deploy with different values. Hover on the input and click **Reset** to revert the input to the fixed value the creator specified. You can't reset input references. Only inputs with fixed values can be reset to their original fixed value. 
    {: important} 

1. Optional: Toggle **Advanced** to the on position to enter values for optional inputs.
1. Click **Save**.
1. Click **Validate**. The modal that is displayed provides more details about your in-progress validation.

   If the validation fails, you can [troubleshoot the failure](/docs/secure-enterprise?topic=secure-enterprise-ts-na-failures). Or, an administrator on the {{site.data.keyword.cloud_notm}} Projects service can review the results through the {{site.data.keyword.bpshort}} service and [override the failure and approve](/docs/secure-enterprise?topic=secure-enterprise-approve-failed-validation) the configuration to deploy anyway. However, ensure that the pipeline failed due to the Code Risk Analyzer scan and not because of a validation or plan failure. It is not recommended to override a failure that is flagged due to a validation or plan failure as the configuration might not deploy successfully. For more information about security and compliance in projects, see [Achieving continuous compliance as an enterprise](/docs/secure-enterprise?topic=secure-enterprise-continuous-compliance).

If you're configuring deployable architectures that are stacked together, make sure to validate each architecture in order according to their dependencies. Alternatively, you can edit your project's settings to automatically deploy configuration changes that are validated successfully. If you do so, each architecture is validated, approved, and deployed automatically according to their dependencies. For more information, go to [Deploying an architecture](/docs/secure-enterprise?topic=secure-enterprise-deploy-project&interface=ui). 
{: important}

## Approving configuration changes by using the console
{: #approve-changes}
{: ui}

After you validate your configuration, the changes must be approved by an editor or administrator on the {{site.data.keyword.cloud_notm}} Projects service. Complete the following steps to approve changes:

1. From the projects list, select a project.
1. Check that there aren't any outstanding needs attention items on the **Overview** tab in your project. Needs attention items can block your ability to deploy.
1. Go to the **Configurations** tab, and select a deployable architecture configuration.
1. Click **Edit**.
1. Click **View last validation**.
1. Add a comment providing more details about the approval, and click **Approve**.

If your validation failed due to the Code Risk Analyzer scan, an administrator on the {{site.data.keyword.cloud_notm}} Projects service can [override the failure and approve](/docs/secure-enterprise?topic=secure-enterprise-approve-failed-validation) the configuration to deploy anyway.
{: remember}

## Configuring an architecture by using the CLI
{: #how-to-config-cli}
{: cli}

To add a configuration to a project by using the CLI, run the following `ibmcloud project config-create` command:

```sh
ibmcloud project config-create --project-id PROJECT-ID [--definition DEFINITION] [--schematics SCHEMATICS]
```
{: codeblock}

For more information about the command parameters, see [**`ibmcloud project config-create`**](/docs/cli?topic=cli-projects-cli#project-cli-config-create-command).

## Approving configuration changes by using the CLI
{: #approve-changes-cli}
{: cli}

1. Run the following `ibmcloud project config-validate` command to get a validation check on your configuration:

   ```sh
   ibmcloud project config-validate --project-id PROJECT-ID --id ID
   ```
   {: codeblock}

   For more information about the command parameters, see [**`ibmcloud project config-validate`**](/docs/cli?topic=cli-projects-cli#project-cli-config-validate-command).

1. After validating your configuration, approve your configuration edits and merge them to the main configuration by running the following `ibmcloud project config-approve` command:

   ```sh
   ibmcloud project config-approve --project-id PROJECT-ID --id ID [--comment COMMENT]
   ```
   {: codeblock}

   For more information about the command parameters, see [**`ibmcloud project config-approve`**](/docs/cli?topic=cli-projects-cli#project-cli-config-approve-command).

## Configuring an architecture by using the API
{: #how-to-config-api}
{: api}

You can programmatically add a configuration to a project by calling the [Projects API](/apidocs/projects#create-config){: external} as shown in the following sample request. The example adds a configuration with the name `My new configuration` to a project:

```bash
curl -X POST --location --header "Authorization: Bearer {iam_token}" \
  --header "Accept: application/json" \
  --header "Content-Type: application/json" \
  --data '{ "definition": { "name": "env-stage", "description": "Stage environment configuration.", "locator_id": "1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.018edf04-e772-4ca2-9785-03e8e03bef72-global", "inputs": { "account_id": "account_id", "resource_group": "stage", "access_tags": [ "env:stage" ], "logdna_name": "LogDNA_stage_service", "sysdig_name": "SysDig_stage_service" }, "settings": { "IBMCLOUD_TOOLCHAIN_ENDPOINT": "https://api.us-south.devops.dev.cloud.ibm.com" } } }' \
  "{base_url}/v1/projects/{project_id}/configs"
```
{: curl}
{: codeblock}

## Approving configuration changes by using the API
{: #approve-changes-api}
{: api}

You can programmatically approve configuration edits and merge them to the main configuration by calling the [Projects API](/apidocs/projects#approve){: external} as shown in the following sample request. The example approves configuration edits and merges them to the configuration:

```bash
curl -X POST --location --header "Authorization: Bearer {iam_token}" \
  --header "Accept: application/json" \
  --header "Content-Type: application/json" \
  --data '{ "comment": "Approving the changes" }' \
  "{base_url}/v1/projects/{project_id}/configs/{id}/approve"
```
{: curl}
{: codeblock}
