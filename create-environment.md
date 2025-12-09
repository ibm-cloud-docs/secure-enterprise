---

copyright:

  years: 2023, 2025

lastupdated: "2025-12-09"

keywords: environment, add environment, deployment environment, production environment

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}


# Creating an environment
{: #create-env}

Within your project, create an environment to group related configurations together and share values across them for easier deployments. The properties that you add to an environment are automatically added to configurations that are using that environment. Within the configuration, you can override any values that are provided by the environment. For more information, see the [benefits to using environments](/docs/secure-enterprise?topic=secure-enterprise-best-practices-projects#best-practice-env).
{: shortdesc}

When you edit a configuration, you can select an environment for the configuration to use in the **Define details** section.
{: tip}

## Before you begin
{: #prereq-env}

You must have the Editor role or greater on the {{site.data.keyword.cloud}} Projects service to create and edit environments. For more information about access and permissions, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).

## Creating an environment by using the console
{: #create-env-ui}
{: ui}

To create an environment from your project, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **[Projects](/projects/)** and select a project.
1. From the Manage tab, select **Environments**.
1. Click **Create**.
1. Provide a name for your environment, then click **Add** to add properties.

There are three categories of properties that you can add to an environment: authentication, compliance, and input. You can add these properties manually, or add multiple properties at once from a configuration in your project. 

### Adding properties manually
{: #add-prop-manually}
{: ui}

If you add properties manually, consider adding the authentication method first. The authentication method for your target account is required to authorize a deployment to that target account. The authentication method is also used if you want to pull in a {{site.data.keyword.compliance_short}} attachment from your target account. You can add only one authentication property to an environment, and you can only add a compliance property to an environment if you have a {{site.data.keyword.compliance_short}} attachment in your target account that you want to use. You can't add a compliance property that uses the [default controls from the deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#cra-validate-failure), as those default controls vary from architecture to architecture.

You must provide an authentication property in your environment first with valid authentication details if you want to add a compliance property and specify an attachment from {{site.data.keyword.compliance_short}}. You can [use an API key with {{site.data.keyword.secrets-manager_short}}](/docs/secure-enterprise?topic=secure-enterprise-authorize-project&interface=cli) or you can [use a trusted profile](/docs/secure-enterprise?topic=secure-enterprise-tp-project&interface=cli) to authenticate with the target account that contains your {{site.data.keyword.compliance_short}} attachment.
{: important}

Authentication and compliance information is used in all configurations, so if you add an authentication property or a compliance property with a {{site.data.keyword.compliance_short}} attachment to your environment, it is automatically added to any configuration that's using the environment.

Input values can also be added as properties to your environment. When you add an input to an environment, the name of your input in the environment must match the name of the input in the configurations that are using the environment. If the names do not match, the value from the environment is not used in the configuration. For example, if you add an input to your environment and name it `resource_group`, but a configuration that is using that environment has an input that is named `resourcegroup` the value from the environment is not used in that configuration.

Multiple architectures can use the same environment, but not all architectures have the same input names for similar values. You can add multiple inputs to your environment with the same value, but different names. For example, you can add two input properties and name one `resource_group` and the other `resourcegroup`. Keep the values the same for both properties. Any configuration that's using the environment and has a `resource_group` input name or a `resourcegroup` input name will use the same value from the environment.

To add a property manually, complete the following steps:

1. After you create your environment, click the **Edit** icon ![Edit icon](../icons/edit-tagging.svg "Edit") for the environment.
1. Click **Add** > **Add manually...**
1. Select which category of property you want to add.

   Consider adding an authentication property first with valid authentication details for your target account.
   {: remember}

1. Provide the details for the property and click **Add**.
1. Click **Save**. 



### Adding properties from a configuration
{: #add-from-config}
{: ui}

Save time and reduce errors by adding multiple properties to your environment from a configuration. By doing so, you help ensure that the properties are accurate in your environment, as the input names and values are pulled directly from the configuration that you select. After you add the properties from a configuration, you can edit them in the environment without affecting your configuration. Future edits to your configuration do not impact the properties in your environment.

To add multiple properties from a configuration, complete the following steps:

1. After you create your environment, click the **Edit** icon ![Edit icon](../icons/edit-tagging.svg "Edit") for the environment.
1. Click **Add** > **Add from a configuration…**
1. Select the configuration that you want to use to add properties to your environment.
1. Select the items from the configuration that you want to add to your environment, and click **Add**.

## Creating an environment by using the CLI
{: #create-env-cli}
{: cli}

To create an environment by using the CLI, run the following `ibmcloud project environment-create` command:

```sh
ibmcloud project environment-create --project-id PROJECT-ID [--definition DEFINITION]
```
{: codeblock}

Within the `definition`, you can specify the input properties that you want to include in your environment by using the `inputs` option. Similarly, you can specify an authentication property in your environment by using the `authorizations` option within the `definition`. And, you can add a compliance property by including the `compliance-profile` option within the `definition` as well.

For example, the following command creates an environment that is named `development` and has one input property that defines the resource group:

```sh
ibmcloud project environment-create \
     --project-id exampleString \
     --definition '{"name": "development", "inputs": {"resource_group" : "stage"}}'
```
{: codeblock}

Similarly, the following command creates an environment that is named `development` and has one input property that defines the resource group. A [trusted profile](/docs/secure-enterprise?topic=secure-enterprise-tp-project&interface=cli) is also provided as an authentication property, along with an attachment from {{site.data.keyword.compliance_short}} for the compliance property that is located in the `us-south` region:

```sh
ibmcloud project environment-create \
     --project-id exampleString \
     --definition '{
    "name": "development",
    "authorizations": {
      "method": "trusted_profile",
      "trusted_profile_id": "<trusted-profile-id>"
    },
    "inputs": {
      "resource_group": "stage"
    },
    "compliance_profile": {
      "id": "<profile-id>",
      "instance_id": "<instance-id>",
      "instance_location": "us-south",
      "profile_name": "{\"instance_crn\":\"crn:v1:staging:public:compliance:us-south:a/<account>:<instance-id>::\",\"instance_label\":\"compliance\",\"name\":\"Basic Control\",\"version\":\"1.0.0\",\"attachment_label\":\"basic\"}",
      "attachment_id": "<attachment-id>"
    }
  }'
```
{: codeblock}

For more information about the command parameters, see [**`ibmcloud project environment-create`**](/docs/cli?topic=cli-projects-cli#project-cli-environment-create-command).

## Creating an environment by using the API
{: #create-env-api}
{: api}

You can programmatically create an environment by calling the [Projects API](/apidocs/projects#create-project-environment){: external} as shown in the following sample request. The example creates an environment that is named `development` and has two input properties, one to define the resource group and one to define the region:

```bash
curl -X POST --location --header "Authorization: Bearer {iam_token}" \
  --header "Accept: application/json" \
  --header "Content-Type: application/json" \
  --data '{ "definition": { "name": "development", "description": "The environment 'development'", "authorizations": { "method": "api_key", "api_key": "TbcdlprpFODhkpns9e0daOWnAwd2tXwSYtPn8rpEd8d9" }, "inputs": { "resource_group": "stage", "region": "us-south" }, "compliance_profile": { "id": "some-profile-id", "instance_id": "some-instance-id", "instance_location": "us-south", "profile_name": "some-profile-name", "attachment_id": "some-attachment-id" } } }' \
  "{base_url}/v1/projects/{project_id}/environments"
```
{: curl}
{: codeblock}
