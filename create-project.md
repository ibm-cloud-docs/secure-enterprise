---

copyright:

  years: 2022, 2024

lastupdated: "2024-11-15"

keywords: add project, create project, set project

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}


# Creating a project
{: #setup-project}

In a [project](#x2035151){: term}, you can add deployable architectures from the catalog and customize their configuration. Deploying a configuration from a project groups resources based on the configuration that you deployed. 
{: shortdesc}



You can create a project by going to the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") and selecting **[Projects](/projects/)** or from a deployable architecture in the [catalog](/catalog/). Projects can also be created by using the [Project API](https://{DomainName}/apidocs/projects).



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



## Adding a deployable architecture to a project
{: #add-deployment-project}






Deployable architectures that you add to a project are represented as configurations in the project UI. After you add a deployable architecture to a project, you can edit your configuration before you deploy it. There are a couple of ways to add a deployable architecture to your project.

To add a deployable architecture to your project from the project dashboard, complete the following steps:

1. From the project dashboard, select the Configurations tab.
1. Click **Create**.
1. Select a deployable architecture from the catalog.
1. Click **Review deployment options**.
1. Click **Add to project**.

You can also add a deployable architecture to a project directly from the catalog:

1. Go to the [{{site.data.keyword.cloud_notm}} catalog](/catalog).
1. Select the deployable architecture.
1. Click **Review deployment options**.
1. Click **Add to project**.
1. You can create a new project or add to an existing project.
1. Enter the required details for the deployable architecture.
1. Click **Add**.

Check out the steps on how to [configure and deploy a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-config-project) when you're ready to deploy resources from a deployable architecture from a project.

## Exporting the project JSON
{: #json-export}

As a user that’s working with projects, you can export the project JSON to manually manage the project in your own public or private repository. For example, you might want to push updates to the JSON by calling the project API using CLI commands or by using your own CICD tools. You might also export the JSON as a way to backup the project information outside of the {{site.data.keyword.cloud_notm}} Projects service.

To export the project JSON, complete the following steps:
1. In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **Projects**.
1. From the project dashboard, click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Export JSON**
1. From the modal, click **Export**.

For more information about the project JSON, see [Project JSON](/docs/secure-enterprise?topic=secure-enterprise-json-project).
