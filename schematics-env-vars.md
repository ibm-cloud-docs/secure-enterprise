---

copyright:

  years: 2026

lastupdated: "2026-01-15"

keywords: schematics, environment variables, terraform logs

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Enabling Terraform logs by using the CLI
{: #da-env-vars}

Terraform uses environment variables to customize its behavior, which includes [enabling Terraform logs](https://developer.hashicorp.com/terraform/internals/debugging){: external}. You can set an environment variable to take advantage of Terraform logs to debug unexpected issues with your code. Terraform reads environment variables and applies them at runtime. 

Complete these steps if you use {{site.data.keyword.cloud_notm}} projects to deploy architectures and want to enable Terraform logs. If you use an {{site.data.keyword.bpfull_notm}} workspace to execute Terraform without an {{site.data.keyword.cloud_notm}} project, go to [Using environment variables with workspaces](/docs/schematics?topic=schematics-set-parallelism) for more information. 
{: note}

## Before you begin
{: #prereqs}

- Install the [{{site.data.keyword.cloud_notm}} CLI plug-in](/docs/cli?topic=cli-getting-started).
- Install the [{{site.data.keyword.cloud_notm}} Projects CLI plug-in](/docs/cli?topic=cli-projects-cli).
- Make sure that you have the Editor role on the {{site.data.keyword.cloud_notm}} Projects service. For more information, go to [Assigning access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project&interface=ui). 

## Setting the environment variable
{: #set-variable}

1. Log in to the {{site.data.keyword.cloud_notm}} CLI and select your account:

   ```bash
   ibmcloud login
   ```
   {: pre}

1. Set the environment variable by running the `ibmcloud project config-update` command, specifying the project ID, configuration ID, and a name and value for the environment variable:

   ```bash
   ibmcloud project config-update --project-id PROJECT_ID --id CONFIG_ID --definition-settings "{\"ENV_NAME\":\"ENV_VALUE\"}"
   ```
   {: pre}

   The following example adds the `TF_LOG` environment variable to enable Terraform logs: 

   ```bash
   ibmcloud project config-update --project-id PROJECT_ID --id CONFIG_ID --definition-settings "{\"TF_LOG\":\"debug\"}"
   ```
   {: pre}

   For a full list of command options, go to [`ibmcloud project config-update`](/docs/cli?topic=cli-projects-cli#project-cli-config-update-command). 

Other environment variables are supported in addition to `TF_LOG`. For more information, see the [List of Terraform environment variables](/docs/schematics?topic=schematics-set-parallelism#list-special-env-vars).
{: tip}
