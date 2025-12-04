---

copyright:

  years: 2022, 2025

lastupdated: "2025-12-04"

keywords: add project, create project, set project

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}


# Creating a project
{: #setup-project}

In a [project](#x2035151){: term}, you can add deployable architectures from the catalog and edit their configuration. Deploying a configuration from a project groups resources based on the configuration that you deployed. 
{: shortdesc}

## Before you begin
{: #prereq-project}

Resources are created in the project account for the user. You must have permission to create a project and permission to create the project tooling resources within the account. Make sure that you have the following access:

* The Editor role on the {{site.data.keyword.cloud_notm}} Projects service.
* The Editor and Manager role on the {{site.data.keyword.bplong}} service
* The Viewer role on the resource group for the project

For more information about access and permissions, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).

## Adding users to a project
{: #add-users-project}

Project access is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). You add users to a project by granting the Reader role or higher on the project instance. For more information on assigning access to projects, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).

## Creating a project by using the console
{: #create-project-ui}
{: ui}

You can create a project by going to the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") and selecting **[Projects](/projects/)** or from a deployable architecture in the [catalog](/catalog/). Projects can also be created by using the [Project API](https://{DomainName}/apidocs/projects).

## Adding a deployable architecture to a project by using the console
{: #add-deployment-project}
{: ui}

Deployable architectures that you add to a project are represented as configurations in the project UI. After you add a deployable architecture to a project, you can edit your configuration before you deploy it. There are a couple of ways to add a deployable architecture to your project.

To add a deployable architecture to your project from the project dashboard, complete the following steps:

1. From the project dashboard, select the Configurations tab.
1. Click **Create**.
1. Select a deployable architecture from the catalog.
1. Click **Configure and deploy**.

You can also add a deployable architecture to a project directly from the catalog:

1. Go to the [{{site.data.keyword.cloud_notm}} catalog](/catalog).
1. Select the deployable architecture.
1. From the catalog listing, you can create a new project or add to an existing one. 
1. Click **Configure and deploy**.

### Customizing a deployable architecture in your project
{: #customize-da-in-project}
{: ui}

Depending on the deployable architecture, you might be able to customize it when you add it to a project from a catalog. You might have the following options: 

* Add optional deployable architectures to expand the solution for a particular use case. 
* Swap out one architecture for another. For example, if multiple database options are provided, you can select the one you'd like to use.  
* Select an existing configuration of an architecture in your project to reuse with the architecture that you're adding. By doing so, you can reduce costs and reuse the same deployment for multiple architectures. 

After you make your selections and add the deployable architecture to your project, you can customize it further by selecting the **Options** icon ![Options icon](../icons/action-menu-icon.svg "Options") > **Customize architecture** from the Configurations tab. From there, you can add other compatible deployable architectures to further customize the solution.

To back up the project information outside of the {{site.data.keyword.cloud_notm}} Projects service, you can export the project. For more information, go to [Project JSON](/docs/secure-enterprise?topic=secure-enterprise-json-project&interface=ui).
{: tip}

Check out the steps on how to [configure and deploy a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-config-project) when you're ready to deploy resources from a deployable architecture from a project.

## Creating a project by using the CLI
{: #create-project-cli}
{: cli}

To create a project by using the CLI, run the following `ibmcloud project create` command:

```sh
ibmcloud project create [--definition DEFINITION | --definition-name DEFINITION-NAME --definition-destroy-on-delete=DEFINITION-DESTROY-ON-DELETE --definition-description DEFINITION-DESCRIPTION --definition-auto-deploy=DEFINITION-AUTO-DEPLOY --definition-monitoring-enabled=DEFINITION-MONITORING-ENABLED] --location LOCATION --resource-group RESOURCE-GROUP [--configs CONFIGS] [--environments ENVIRONMENTS]
```
{: codeblock}

See [**`ibmcloud project create`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-create-command) for an example command and more information about the command parameters.

## Adding deployable architecture to a project by using the CLI
{: #add-deployment-project-cli}
{: cli}

To add a deployable architecture to your project by using the CLI, run the following `ibmcloud project config-create` command:

```sh
ibmcloud project config-create --project-id PROJECT-ID [--definition DEFINITION | --definition-compliance-profile DEFINITION-COMPLIANCE-PROFILE --definition-locator-id DEFINITION-LOCATOR-ID --definition-description DEFINITION-DESCRIPTION --definition-name DEFINITION-NAME --definition-environment-id DEFINITION-ENVIRONMENT-ID --definition-authorizations DEFINITION-AUTHORIZATIONS --definition-inputs DEFINITION-INPUTS --definition-settings DEFINITION-SETTINGS --definition-members DEFINITION-MEMBERS --definition-resource-crns DEFINITION-RESOURCE-CRNS] [--schematics SCHEMATICS | --schematics-workspace-crn SCHEMATICS-WORKSPACE-CRN]
```
{: codeblock}

See [**`ibmcloud project config-create`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-create-command) for an example command and more information about the command parameters.

To back up the project information outside of the {{site.data.keyword.cloud_notm}} Projects service, you can export the project. For more information, go to [Project JSON](/docs/secure-enterprise?topic=secure-enterprise-json-project&interface=ui).
{: tip}

## Creating a project by using the API
{: #create-project-api}
{: api}

You can programmatically create a project by calling the [Projects API](/apidocs/projects#create-project){: external} as shown in the following sample request:

```bash
curl -X POST --location --header "Authorization: Bearer {iam_token}" \
  --header "Accept: application/json" \
  --header "Content-Type: application/json" \
  --data '{ "definition": { "name": "acme-microservice", "description": "A microservice to deploy on top of ACME infrastructure.", "authorizations": { "method": "trusted_profile", "trusted_profile_id": "Profile-9ac10c5c-195c-41ef-b465-68a6b6dg5f12" } }, "configs": [ { "definition": { "name": "account-stage", "description": "The stage account configuration.", "locator_id": "1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.018edf04-e772-4ca2-9785-03e8e03bef72-global" } }, { "definition": { "name": "env-stage", "description": "The stage environment configuration that includes services common to all the environment regions.", "locator_id": "1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.018edf04-e772-4ca2-9785-03e8e03bef72-global", "inputs": { "account_id": "ref:/configs/account-stage/inputs/account_id", "resource_group": "stage", "access_tags": [ "env:stage" ], "logdna_name": "The name of the LogDNA stage service instance.", "sysdig_name": "The name of the SysDig stage service instance." } } }, { "definition": { "name": "region-us-south-stage", "description": "The stage us-south configuration.", "locator_id": "1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.018edf04-e772-4ca2-9785-03e8e03bef72-global" } }, { "definition": { "name": "region-eu-de-stage", "description": "The stage eu-de configuration.", "locator_id": "1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.018edf04-e772-4ca2-9785-03e8e03bef72-global", "inputs": { "account_id": "ref:/configs/account-stage/inputs/account_id", "resource_group": "ref:/configs/env-stage/outputs/resource_group_id", "logdna_id": "ref:/configs/env-stage/outputs/logdna_id", "sysdig_id": "ref:/configs/env-stage/outputs/sysdig_id", "access_tags": [ "region:eu-de" ] } } } ], "location": "us-south", "resource_group": "Default" }' \ 
  "{base_url}/v1/projects"
```
{: curl}
{: codeblock}

## Adding a deployable architecture to a project by using the API
{: #add-deployment-project-api}
{: api}

You can programmatically add a deployable architecture to an existing project by calling the [Projects API](/apidocs/projects#create-config){: external} and customizing the configuration as shown in the following sample request:

```bash
curl -X POST --location --header "Authorization: Bearer {iam_token}" \
   --header "Accept: application/json" \
   --header "Content-Type: application/json" \
   --data '{ "definition": { "name": "env-stage", "description": "The stage environment configuration.", "locator_id": "1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.018edf04-e772-4ca2-9785-03e8e03bef72-global", "inputs": { "account_id": "account_id", "resource_group": "stage", "access_tags": [ "env:stage" ], "logdna_name": "LogDNA_stage_service", "sysdig_name": "SysDig_stage_service" } } }' \
   "{base_url}/v1/projects/{project_id}/configs"
```
{: curl}
{: codeblock}

To back up the project information outside of the {{site.data.keyword.cloud_notm}} Projects service, you can export the project. For more information, go to [Project JSON](/docs/secure-enterprise?topic=secure-enterprise-json-project&interface=ui).
{: tip}
