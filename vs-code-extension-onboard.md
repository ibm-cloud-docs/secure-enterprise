---

copyright:

  years: 2024, 2025

lastupdated: "2025-12-15"

keywords: vs code extension, extension, onboard, deployable architecture

subcollection: secure-enterprise

---

# Onboarding your deployable architecture by using a Visual Studio Code extension
{: #vs-code-extension}

You can [onboard your deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-onboard-da&interface=ui) by using the {{site.data.keyword.cloud_notm}} Deployable Architecture Builder Visual Studio Code extension. Using the extension autogenerates materials that are required and it can avoid or minimize issues where a separate configuration is made each time the user runs validation through the catalog. The extension helps you to easily onboard your deployable architecture to your private catalog and a project.

## Before you begin
{: #onboard-before-vs}

Before you can onboard your deployable architecture, be sure that you complete the following prerequisites.

* [Download Visual Studio Code](https://code.visualstudio.com/){: external}
* Verify that you're using a Pay-As-You-Go or Subscription account. See [Viewing your account type](/docs/account?topic=account-account_settings#view-acct-type) for more details.
* Verify that you have the required access to work with private catalogs and deployable architectures.
   * Manager role on the {{site.data.keyword.bplong_notm}} service
   * Editor role on the Catalog Management service
   * Viewer role on all resource groups in your account
   * SecretsReader role on the {{site.data.keyword.secrets-manager_short}} service if you plan to store your secure values in an instance of {{site.data.keyword.secrets-manager_short}}
   * Reader role on the {{site.data.keyword.sysdigsecure_short}} service
   * Other roles that are required for specific resources in your customized deployable architecture.
* Ensure that you have the source code for your deployable architecture stored in a GitHub. For help with getting your source code into a repository, see [Setting up your source code repository](/docs/sell?topic=sell-source-repo-setup).
* A Terraform module in a public or private GitHub repository that is cloned to a local folder. For experimentation, you can make a fork of this [sample Terraform module](https://github.com/l2fprod/simple-da). If your repository is private, you need a personal access token with `repo` and `read:user` permissions.

## Getting the VS Code extension
{: #getting-extension}

To get the extension, open VS Code, go to **Extensions**, search for and select **{{site.data.keyword.cloud_notm}} Deployable Architecture Builder** to download. After you download, you can follow the walkthrough to get started or use the following steps. If the walkthrough doesn't automatically open, you can use the command palette. Go to **View > Command Palette**. Search for `Walkthrough`, click **Welcome: Open Walkthrough...**, then select **Get started with {{site.data.keyword.cloud_notm}} Deployable Architecture Builder**.

## Onboarding your deployable architecture
{: #extension-onboard-da}

The first time that you validate your deployable architecture, you go through the steps for onboarding it to {{site.data.keyword.cloud_notm}}. Use the following steps to onboard your deployable architecture:

1. Log in to your {{site.data.keyword.cloud_notm}} account.
   1. To log in to your account, click **View** and select **Command Palette**.
   1. In the search field, search for `IBM Cloud - Log in`.
   1. Select **{{site.data.keyword.cloud_notm}} - Log in** and log in with an API key, federated ID, or username and password.
   1. (Optional) You can change your login environment. Click **View**, select **Command Palette**, search for `IBM Cloud - Log in`, and select **{{site.data.keyword.cloud_notm}} - Log in > Change login environment**. From the menu, select **Production** or **Staging**.
1. Make sure that you're logged in to GitHub from VS Code.
   1. Select the ![Avatar icon](../icons/i-avatar-icon.svg "Avatar") > **Sign in to Sync Settings**.
   1. Select your account.
   1. Click **Authorize Visual-Studio-Code**.
1. Clone your repo and add it to a VS Code workspace.
   1. Clone your deployable architectures repo from GitHub.
   1. To add it to your workspace within VS Code, click **File > Add folder to workspace...**, then select the repo from your files.
1. Add the catalog manifest files to the Terraform module.
   1. To add the catalog manifest, right-click the module folder in your workspace.
   1. Select **{{site.data.keyword.cloud_notm}} > Add deployable architecture manifest files**.
1. Onboard and validate your deployable architecture.
   1. Right-click the `ibm_catalog.json` file, and select **{{site.data.keyword.cloud_notm}} > Validate a deployable architecture on {{site.data.keyword.cloud_notm}}**.
   1. Select the repo branch. Or you can click **+** and enter the name of a new one that will be created.
   1. Select an existing private catalog. Or you can click **+** and enter the name of a new one that will be created.
   1. Enter the offering version.
   1. Select an existing project. Or you can click **+** and enter the name of a new one that will be created for you.
   1. Enter your {{site.data.keyword.cloud_notm}} API key for the account where resources will be deployed.
   1. Enter the relative path for the architectural diagram. If you don't have a diagram, a placeholder is provided.
   1. (Optional) If the GitHub repository is private or in GitHub Enterprise, enter your personal access token.
   1. The YAML inputs file is opened. Edit the values under the `inputs` property and save the file.
   1. Click **Continue**

When validation begins, an Output channel is opened that shows the validation logs. If it does not, in the **Output** view, select the `DA Validation` channel from the menu. Validation can take several minutes to complete.
