---

copyright:

  years: 2024, 2025

lastupdated: "2025-04-08"

keywords: manage project, deploy project, merge request, merge changes, deploy configuration, deploy architecture

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}


# Deploying an architecture
{: #deploy-project}

After your deployment updates are validated and approved, you can deploy your architecture to your target account. You can deploy to any account that has authorized your project for deployments. For more information, see [Using an API key with Secrets Manager to authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project).

You can edit your project settings to automatically approve and deploy changes that are successfully validated. To do so, open your project and go to **Manage** > **Settings** and toggle **Auto-deploy** on. With this setting enabled, if you have any architectures that are stacked together in your project, they are validated, approved, and deployed according to their dependencies. 
{: tip}

## Deploying your architecture by using the console
{: #deploy-config-copy}
{: ui}

To deploy your architecture, complete the following steps:

1. From the **Deployments** tab in your project, click the name of your deployable architecture > **Edit**.
1. Review the input values and make any necessary changes.
1. Click **Deploy**. This action includes preparing your resources to deploy, which can take a few minutes. You are notified when the deployment is successful.
1. Review the resources and outputs from the deployable architecture.

If any additional changes are needed, or if a new version of the deployable architecture is available, edit the architecture deployment, validate it, and deploy it again. Deploy your architecture again [if there is any drift detected](/docs/schematics?topic=schematics-drift-note&interface=ui#drift-ui) in your {{site.data.keyword.bpshort}} workspace.

## Viewing resources by using the console
{: #resources-project-copy}
{: ui}

View your resources that are tied to the deployable architecture by completing the following steps:

1. From the projects list, select a project.
1. Go to the **Deployments** tab, and select a deployable architecture.
1. From the **Resources** tab, you can view the full list of deployed resources. For additional information, click **View in {{site.data.keyword.bpshort}}**.

## Reviewing output values by using the console
{: #project-output-values}
{: ui}

Output values populate after the architecture is deployed, and they provide important information about your created resources. After the deployment is successful, you can go to the **Output** tab to view the outputs for your deployment.

## Deploying your architecture by using the CLI
{: #deploy-config-cli-copy}
{: cli}

1. Run the following `ibmcloud project config-validate` command to get a validation check on your configured edits:

   ```sh
   ibmcloud project config-validate --project-id PROJECT-ID --id ID [--x-auth-refresh-token X-AUTH-REFRESH-TOKEN] [--version VERSION]
   ```
   {: codeblock}

   For more information about the command parameters, see [**`ibmcloud project config-validate`**](/docs/cli?topic=cli-projects-cli#project-cli-config-validate-command).

1. Run the following `ibmcloud project config-deploy` command to deploy your configured edits after they are validated and approved:

   ```sh
   ibmcloud project config-deploy --project-id PROJECT-ID --id ID
   ```
   {: codeblock}

   For more information about the command parameters, see [**`ibmcloud project config-deploy`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-deploy-command).

## Getting the list of resources that are tied to a deployment by using the CLI
{: #resources-project-cli-copy}
{: cli}

To retrieve the resources that are tied to a deployment, run the following `ibmcloud project get` command:

```sh
ibmcloud project get --id ID [--exclude-configs EXCLUDE-CONFIGS] [--complete COMPLETE]
```
{: codeblock}

For more information about the command parameters, see [**`ibmcloud project get`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-get-command).

You can also run the following `ibmcloud resource search` command to retrieve all the resources in an account created by deployments in a project:

```sh
ibmcloud resource search "service_tags:\"schematics::project_id:PROJECT_ID\""
```
{: codeblock}
