---

copyright:
  years: 2023, 2024
lastupdated: "2024-11-22"

subcollection: secure-enterprise

keywords: project json, project metadata, JSON config, project config, export JSON, json

---

{{site.data.keyword.attribute-definition-list}}


# Project JSON
{: #json-project}

Each configuration in a project is stored as JSON in a file called `project.json`. Projects require governance over configuration changes that are stored in `project.json`, for example, requiring approvals and ensuring that automated checks pass before changes are saved.

## What's in my Project.json file?
{: #project-json}

The `project.json` file has several parts:

* The project ID and description, which are user-defined values.
* The project metadata, which includes project CRN, location, resource group, and state.
* An array of configuratons that contains a reference to the deployable architecture and all of the input values.

It's recommended to create your initial `project.json` from the [Projects page](/projects) in the console. This provides an initial `project.json` file that can be edited. When you create a project in the console, the `project.json` is automatically created for you.

### Project metadata
{: #project-metadata}

Project metadata might contain `cumulative_needs_attention_view`, if there are events that have happened related to the project that the user must now take action on. `event_notifications_crn` is also an optional value, if the project is configured as a source for {{site.data.keyword.en_short}}. For more information, see [Enabling event notifications for projects](/docs/secure-enterprise?topic=secure-enterprise-event-notifications-events&interface=ui).

```json
  ...
  "id": "cfbf9050-ab8e-ac97-b01b-ab5af830be8a",
  "name": "CRA Test",
  "description": "",
  "metadata": {
    "crn": "crn:v1:staging:public:project:us-south:a/<account_id>:cfbf9050-ab8e-ac97-b01b-ab5af830be8a::",
    "location": "us-south",
    "resource_group": "Default",
    "state": "READY",
    "cumulative_needs_attention_view": [
      {
        "event": "config.defn.update"
      },
      {
        "event_id": "489f0090-6d7c-4af5-8f20-9106543e4974"
      },
      {
        "config_id": "069ab83e-5016-4bf2-bd50-cc95cf678293"
      },
      {
        "config_version": 1
      }
    ],
    "event_notifications_crn": "crn:v1:staging:public:event-notifications:us-south:a/<account_id>:instance-id::"
  }
  ...
```

A project's ID and CRN can't be editied and are stored by {{site.data.keyword.cloud_notm}}. Also, tags on the project instance itself are stored in global search and tagging.

### Configurations
{: #project-config-json}

Each validated and approved configuration in a project has an object in the configs array. Each configuration object has a name, an array of inputs, and a type. If the type is an IaC template, then a catalog `locator_id` is included.

```json
...
"configs": [
    {
      "id": "cfbf9050-ab8e-ac97-b01b-ab5af830be8a",
      "name": "my-deployment",
      "description": "A microservice to deploy on top of ACME infrastructure.",
      "locator_id": "1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.018edf04-e772-4ca2-9785-03e8e03bef72-global",
      "type": "terraform_template",
      "input": [
        {
          "name": "cos_bucket_name",
          "type": "string",
          "required": true,
          "default": "sample-cos-bucket",
          "value": ""
        },
        {
          "name": "ibmcloud_api_key",
          "type": "password",
          "required": true,
          "default": "__NOT_SET__",
          "value": ""
        },
      ],
      "output": [
        {
          "name": "resource_group_id"
        },
        {
          "name": "logdna_id"
        }
      ]
   ]
...
```

## Exporting the project JSON by using the console
{: #json-export}
{: ui}

As a user that's working with projects, you can export the project JSON to back up the project information outside of the {{site.data.keyword.cloud_notm}} Projects service. 

Interested in managing your project in your own Git repository? While you can export the project JSON to manually manage your project, you can also integrate your project with a Git repository directly. For more information, go to [Integrating a project with a Git repository](/docs/secure-enterprise?topic=secure-enterprise-connect-to-git).
{: tip}

To export the project JSON, complete the following steps:
1. In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **Projects**.
1. From the project dashboard, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Export JSON**
1. From the modal, click **Export**.

## Setting the JSON output format by using the CLI
{: #json-format-cli}
{: cli}

By using the following `--output json` option on the command line, you can set the output of a command to JSON:

```sh
--output json
```
{: codeblock}

## Exporting the project JSON by using the CLI 
{: #json-export-cli}
{: cli}

As a user that's working with projects, you can export the project JSON to back up the project information outside of the {{site.data.keyword.cloud_notm}} Projects service. 

Interested in managing your project in your own Git repository? While you can export the project JSON to manually manage your project, you can also integrate your project with a Git repository directly. For more information, go to [Integrating a project with a Git repository](/docs/secure-enterprise?topic=secure-enterprise-connect-to-git).
{: tip}

To export the project JSON by using the CLI, run the following `ibmcloud project get` command: 

```sh
ibmcloud project get --id ID
```
{: codeblock}

See [**`ibmcloud project get`**](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-get-command) for an example command and more information about the command parameters.

## Exporting the project JSON by using the API
{: #json-export-api}
{: api}

As a user that's working with projects, you can export the project JSON to back up the project information outside of the {{site.data.keyword.cloud_notm}} Projects service. 

Interested in managing your project in your own Git repository? While you can export the project JSON to manually manage your project, you can also integrate your project with a Git repository directly. For more information, go to [Integrating a project with a Git repository](/docs/secure-enterprise?topic=secure-enterprise-connect-to-git).
{: tip}

You can programmatically export the project JSON by calling the [Projects API](/apidocs/projects#get-project){: external} as shown in the following sample request: 

```bash
curl -X GET --location --header "Authorization: Bearer {iam_token}" \
  --header "Accept: application/json" \  
  "{base_url}/v1/projects/{id}"
```
{: curl}
{: codeblock}
