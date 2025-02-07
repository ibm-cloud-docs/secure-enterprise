---

copyright:
  years: 2024
lastupdated: "2025-02-07"

keywords: Git, Git integration, Connect to Git, Github, Gitlab, GitHub enterprise, pipelines, toolchains, workflow

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Integrating a project with a Git repository
{: #connect-to-git}

Connect a project to a Git repository to save configurations there. By doing so, you can use your repository and the CI and CD tools of your choosing to automate pipelines on configurations in a project. 
{: shortdesc}

This is an experimental feature that is available for evaluation and testing purposes and might change without notice.
{: experimental}

Pipelines and toolchains are customizable, so you can automate many actions between your Git repository and your project by using projects API methods or CLI commands. For example, you can create a pipeline to trigger an update in your project when configuration changes are merged to the main branch in your repository. For more information, see [Automating projects actions in your Git repository](/docs/secure-enterprise?topic=secure-enterprise-tutorial-git-integration).



In a project, draft configurations can be saved to any branch in your repository. However, you can validate and deploy draft configurations only after they are merged to the branch in your repository that manages your CD pipelines. You must also sync the updates from your repository into your project by updating configurations before you can validate and deploy those configurations. You can automate this update by using the [`project.config.update`](/apidocs/projects#update-config) API method or by using the [`ibmcloud project config-update`](/docs/secure-enterprise?topic=secure-enterprise-projects-cli#project-cli-config-update-command) CLI command. 
{: important}

## Before you begin
{: #byb-connect-git}

1. Make sure you have the Editor role on the {{site.data.keyword.cloud_notm}} Projects service to manage Git repository integrations.

1. Save an access token as a secret to connect to your Git repository. The access token needs write access to the branches that you want to use and to create commits in your repository. The access token also needs the ability to list branches and read them. 
    1.  [Create a {{site.data.keyword.secrets-manager_short}} service instance](/docs/secrets-manager?topic=secrets-manager-create-instance&interface=ui) in your {{site.data.keyword.cloud_notm}} account. To create a secret, you must have the Writer role or higher on the {{site.data.keyword.secrets-manager_short}} service.

    1. After you create your secret instance, make sure that you select **Other secret type** to add an arbitrary secret. For information about creating an arbitrary secret, see [Creating arbitrary secrets in the UI](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). Your arbitrary secret must contain the access token for your Git repository.

## Connecting a project to a Git repository
{: #git-integration}

Connect your Git repository to your project. By doing so, configuration changes are saved to your repository, as opposed to the project JSON file. Because your project needs to save configurations to your repository, you must provide an access token to authenticate with the repository from your project. 

1. In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **[Projects](/projects/)** and select a project.
1. From the Manage tab, select **Integrations**.
1. In the Git repository integration section, click **Connect**. 
1. From the **Repository type** menu, select the type of repository that you want to use to manage your configurations. Typically, this repository is the one you use to manage your pipelines and toolchains. You can select **GitHub**, **GitLab**, or **GitHub Enterprise**. 
1. Enter the URL to the repository. 
1. Optionally, specify a folder within the repository. Consider specifying a folder if you want to integrate your repository with multiple projects. Each project can have its own folder. 
1. Hover or click the access token field, then click the **Secrets** icon ![Key icon](../icons/secret-key.svg "Secrets") to select the secret that contains your access token. 
1. If your project already contains configurations, select **Copy existing configuration files to this repository** to save existing configurations to your repository. 
1. Click **Save**. 

## Next steps
{: #connect-git-next-steps}

Now that your Git repository is connected with your project, you're ready to customize pipelines and toolchains. To learn how to create a pipeline that automatically updates configurations in your project when changes are merged to your main branch, see [Automating projects actions in your Git repository](/docs/secure-enterprise?topic=secure-enterprise-tutorial-git-integration). 
