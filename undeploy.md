---

copyright:

  years: 2025

lastupdated: "2025-07-10"

keywords: undeploy, destroy resources, deploy, bug, deployable architecture

subcollection: secure-enterprise

content-type: tutorial
account-plan: paid
completion-time: 20m

---

{{site.data.keyword.attribute-definition-list}}

# Rolling back an update by using the CLI
{: #undeploy-tutorial}
{: toc-content-type="tutorial"}
{: toc-completion-time="20m"}

This tutorial walks you through rolling back a deployment by using the CLI. By completing this tutorial, you learn how to revert deployed changes to a deployable architecture configuration in a project. By the end of this tutorial, your deployable architecture configuration will match the last successfully deployed version. 
{: shortdesc}

Imagine you are a software developer for *Example Corp* enterprise. Your infrastructure architect discovered the Cloud automation for {{site.data.keyword.codeengineshort}} deployable architecture, and your cloud automation engineering professional customized it to fully meet your business needs. You deployed that customized architecture that is called `Example Corp's infrastructure` to two regions. Then, your cloud automation engineering professional released a new version of `Example Corp's infrastructure`, so you updated your running deployments to use the latest version. Recently, your team discovered a bug in the `us-south` region. The bug is new, so you want to roll back the deployment for the `us-south` region to what it was previously, as you know that deployment didn't include the bug. 

This tutorial uses a fictitious scenario to help you understand how to roll back deployed changes to a deployable architecture configuration in your project. As you complete the tutorial, adapt each step to match your organization's needs. 

## Before you begin
{: #undeploy-prereq}

1. [Create a customized deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-basic-custom) called `Example Corp's infrastructure` and onboard it to a private catalog called `Example Corp catalog`. 
1. [Deploy Example Corp's infrastructure to two regions](/docs/secure-enterprise?topic=secure-enterprise-deploy-regions) by using a project called `Example Corp infrastructure`. 
1. [Update Example Corp's infrastructure with a new version in the private catalog](/docs/secure-enterprise?topic=secure-enterprise-custom-extend) and add two database options that your users can choose from. [Validate the version](/docs/secure-enterprise?topic=secure-enterprise-onboard-da#validate-version) in the private catalog so the new version is in the `Validated draft` state. 
1. [Update your project](/docs/secure-enterprise?topic=secure-enterprise-update-version-tutorial) to use the latest version of `Example Corp's infrastructure` and deploy the changes. 
    
    The latest version of `Example Corp's infrastructure` includes database options that might be deployed from your `Example Corp infrastructure` project. Completing this tutorial doesn't impact anything in your project except for the `example-corp-us-south` configuration. If you need to, you can undeploy the other configurations in your project separately, including any databases. 
    {: important}

1. To complete this tutorial, [install the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started). 

## Finding the project ID, the configuration ID, and the version you want to revert to
{: #ids}
{: step}

Before you can run the CLI commands to revert your changes to a previously deployed version, you need the ID of the `example-corp-us-south` configuration and the ID of the `Example Corp infrastructure` project that contains the configuration. 

To find these IDs and the version by using the CLI, run the following commands: 

1. Log in to {{site.data.keyword.cloud_notm}} by running the following [`ibmcloud login`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login) command. If you have multiple accounts, you are prompted to select which account to use:
    
    ```sh
    ibmcloud login \
        --sso
    ```
    {: pre}

    Use the `--sso` option to log in through the console. If you do so, the console opens in a web browser for you to log in. Then, a code is generated for you to paste into the CLI. 
    {: tip}

1. Find the ID for the `Example Corp infrastructure` project in your account by running the following [`ibmcloud project list`](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-list-command) command: 
    
    ```sh
    ibmcloud project list \
        --all-pages
    ```
    {: pre}

    Use the `--all-pages` option to retrieve all of the projects in your account. 
    {: tip}

1. Use the ID of the `Example Corp infrastructure` project and the name of the configuration to find the configuration ID. Run the following [`ibmcloud project configs`](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-configs-command) command to do so:
    
    ```sh
    ibmcloud project configs \
        --project-id <Example-Corp-infrastructure-project-id> \
        --all-pages
    ```
    {: pre}

1. Use the ID of the `Example Corp infrastructure` project and the ID of the `example-corp-us-south` configuration to find the versions of that configuration in the project. Run the following [`ibmcloud project config-versions`](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-versions-command) command: 
    
    ```sh
    ibmcloud project config-versions \
        --project-id <Example-Corp-infrastructure-project-id> \
        --id <example-corp-us-south-id>
    ```
    {: pre}

    Two versions of the `example-corp-us-south` configuration should be listed. Version two is in the `deployed` state, while version one is in the `superseded` state. The earliest version is the first one that was deployed. That version is the one you want to roll back to. 



## Retrieve the version that you want to roll back to
{: #retrieve-version}
{: step}

Now that you have the IDs and the version that you want to roll back to, run the following [`ibmcloud project config-version`](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-version-command) command to retrieve the specific version of `example-corp-us-south`. Make sure that you include the parameter to return the output in `JSON` format: 

```sh
   ibmcloud project config-version \
        --project-id <Example-Corp-infrastructure-project-id> \
        --id <example-corp-us-south-id> \
        --version 1 \
        --output json
```
{: pre}

Copy the contents of the `definition` block in the output `JSON` and save it to a `definition.json` file. The contents of the `definition` include the input values that you provided, the authorization you used to grant the project access to deploy to your target account, the name of your configuration, and the locator ID for `Example Corp's infrastructure` that identifies the deployable architecture in the private catalog. Consider it a snapshot of version 1 of `example-corp-us-south` with all of the inputs and information that you provided in your project, for example: 

```json
{
    "authorizations": {},
    "compliance_profile": {},
    "description": "",
    "inputs": {
      "prefix": "us-south",
      "region": "us-south"
    },
    "locator_id": "<Example-Corp-catalog-locator-id>",
    "name": "example-corp-us-south",
    "environment_id": "<Example-Corp-infrastructure-dev-id>"
}
```
{: pre}

## Update the configuration to use the previous version's definition 
{: #update-south-def}
{: step}

Now that you have the definition of version 1 of `example-corp-us-south`, update that configuration to use that definition. This updates `example-corp-us-south` so it is exactly as you configured it when you deployed version 1. Run the following [`ibmcloud project config-update`](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-update-command) command for `example-corp-us-south`: 

```sh
ibmcloud project config-update \
     --project-id <Example-Corp-infrastructure-project-id> \
     --id <example-corp-us-south-id> \
     --definition "$(cat definition.json)"
```
{: pre}

## Validate, approve, and deploy the changes
{: #val-app-dep}
{: step}

Now that `example-corp-us-south` is updated to use the definition for the first version of the configuration, it's time to validate, approve, and deploy the changes. You can [use the console to do so](/docs/secure-enterprise?topic=secure-enterprise-deploy-regions&interface=ui#configure-architecture), or you can run the following CLI commands: 

1. Validate `example-corp-us-south` by running the following [`ibmcloud project config-validate`](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-validate-command) command: 
    
    ```sh
    ibmcloud project config-validate \
        --project-id <Example-Corp-infrastructure-project-id> \
        --id <example-corp-us-south-id>
    ```
    {: pre}

1. Approve `example-corp-us-south` by running the following [`ibmcloud project config-approve`](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-approve-command) command: 
    
    ```sh
    ibmcloud project config-approve \
        --project-id <Example-Corp-infrastructure-project-id> \
        --id <example-corp-us-south-id> \
        --comment <Rolling back to the previously deployed version>
    ```
    {: pre}

1. Deploy `example-corp-us-south` by running the following [`ibmcloud project config-deploy`](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-deploy-command) command: 
    
    ```sh
    ibmcloud project config-deploy \
        --project-id <Example-Corp-infrastructure-project-id> \
        --id <example-corp-us-south-id>
    ```
    {: pre}

After the deployment successfully completes, `example-corp-us-south` is now reverted to the first version of that configuration that you deployed from the project. Your team can now investigate the bug and release an update to `Example Corp's infrastructure` if needed. 

