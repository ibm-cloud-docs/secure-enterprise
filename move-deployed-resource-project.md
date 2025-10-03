---

copyright:

  years: 2023, 2025

lastupdated: "2025-10-03"

keywords: move schematics workspace, migrate schematics workspace, deployable architecture

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Moving resources from a {{site.data.keyword.bpshort}} workspace into a project
{: #move-deployed-resource-project}

You can manage the lifecycle of an enterprise architecture by moving resources from an [{{site.data.keyword.bplong}} workspace](/docs/schematics?topic=schematics-sch-create-wks&interface=ui) into an {{site.data.keyword.cloud}} project. While you can use a {{site.data.keyword.bpshort}} workspace for using Terraform and deployable architectures, you can use a project to manage the full lifecycle at scale. After you complete this task, you have a deployable architecture tile in your private catalog, and you can manage your deployable architecture's lifecycle from a project.
{: shortdesc}

During this process, you can use the CLI commands [ibmcloud catalog utility create-product-from-workspace](/docs/cli?topic=cli-manage-catalogs-plugin#publish-utility-create) and [ibmcloud project config-create](/docs/cli?topic=cli-projects-cli#project-cli-config-create-command) to create a catalog and a project that are linked together, and then move the {{site.data.keyword.bpshort}} workspace resources into the project.

Watch and learn the process of moving deployed resources to a project.

## Watch and learn
{: #move-wks-da}

![Moving a {{site.data.keyword.bpshort}} workspace into a project](https://cdnapisec.kaltura.com/html5/html5lib/v2.101/mwEmbedFrame.php/p/1773841/uiconf_id/27941801/entry_id/1_ytu3tvzb?wid=_1773841&iframeembed=true&entry_id=1_ytu3tvzb){: video output="iframe" data-script="#move-wks-da-script" id="mediacenter-player" width="560" height="315" scrolling="no" allowfullscreen webkitallowfullscreen mozAllowFullScreen frameborder="0" style="border: 0 none transparent;"}

### Video transcript
{: #move-wks-da-script}
{: notoc}

I am Gili Mendel, an STSM with the {{site.data.keyword.cloud_notm}} development team. I am going to show you how to move a {{site.data.keyword.bpshort}} workspace in the {{site.data.keyword.cloud_notm}} deployable architecture or a project.

A {{site.data.keyword.bpshort}} workspace is the way that you run a Terraform automation on the {{site.data.keyword.cloud_notm}}. The workspace holds a link to a Git repository from which it fetches the Terraform template. It also holds the template variable, and the resources state as it creates it.

It is a great way to run Terraform IaaS automation on the cloud without maintaining a highly available {{site.data.keyword.cos_full}} bucket for the state with the intimate integrations to cloud resource providers.

However, this approach does not scale if you are driving multiple automations. Managing the resources and rolling up a new version of the template can be a daunting task.

Although an enterprise leverages Terraform automation, is all about deployable architectures. You publish a reference architecture, with a compliance footprint designation, cost estimates, and a Terraform template that automates the deployment of the architecture.

The Git repository is used to iterate and build the architecture and its automations. It might or might not be visible to users that deploy the architecture. The architecture itself has a lifecycle that includes version upgrades.

Step 1: Use the Catalogs management CLI to create a deployable architecture from the existing workspace, and the lifecycle that is needed to publish the new versions for it.

Step 2: Use the Project CLI to modify the workspace so that it is managed as a deployable architecture by a project. As such, these resources can be treated as a single deployable architecture version configuration and can be managed from an admin account across many environments. Version migration, cost, and the compliance footprint are simpler and is all involving the architecture that is deployed.

## Before you begin
{: #move-prereqs}

* Make sure that the [{{site.data.keyword.bplong}} workspace](/docs/schematics?topic=schematics-sch-create-wks&interface=ui) is created and the resources are provisioned in your account. The project and the workspace must be in the same account.
* Set a [GIT_TOKEN](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens){: external} environment variable to authenticate with your Git source repository. For example, run `export GIT_TOKEN="enter your GIT TOKEN"`. The token needs permission to complete [these Git-related tasks](#git-tasks) as you move deployed resources to your project. 
* Set up an authentication method to connect the project together with the catalog. You can use one of the following methods: 
   * Set the `IC_API_KEY` environment variable to access your {{site.data.keyword.cloud_notm}} account. For example, run `IC_API_KEY="Your API key"`.
   * Use your `trusted-profile-id` details to access your {{site.data.keyword.cloud_notm}} account. For more information, see [Finding the trusted profile ID](/docs/secure-enterprise?topic=secure-enterprise-tp-project#find-tp-id). For example, a trusted profile ID might look similar to `Profile-1bd5eala-000-4a6666-00000`.
* Make sure that the [Project](/docs/cli?topic=cli-projects-cli) and [Catalogs management](/docs/secure-enterprise?topic=secure-enterprise-manage-catalogs-plugin) CLI plug-ins are up to date. Run the `ibmcloud plugin list` command to view the current version of the CLI plug-ins.
* Create a project for production deployment and record the name and ID of the project where you want to move the resources.
* Make sure that the Projects service is authorized in your account to communicate with other {{site.data.keyword.cloud_notm}} services. For more information, see [Granting access between the Projects service and other {{site.data.keyword.cloud_notm}} services](/docs/secure-enterprise?topic=secure-enterprise-access-project&interface=ui#user-create-role).

## Creating a deployable architecture from a {{site.data.keyword.bpshort}} workspace
{: #move-create-da}

Skip this section if you already have a deployable architecture in your {{site.data.keyword.bpshort}} workspace.
{: important}

If you deployed software by using a {{site.data.keyword.bpshort}} workspace or through a private catalog, you can move the deployed resources to a project by using the [ibmcloud catalog utility create-product-from-workspace](/docs/cli?topic=cli-manage-catalogs-plugin#publish-utility-create) command.

The command does the following Git-related tasks:
- Clones the deployable architecture repo. {: #git-tasks}
- Pushes the `ibm_catalog.json` manifest file to a new branch.
- Creates a placeholder architecture diagram (`architecture-diagram-placeholder.drawio.svg`).
- Creates a Git release in the repo.
- Provides the release asset URL.

The command also completes the following {{site.data.keyword.cloud_notm}}-related tasks:
- Creates a private catalog in {{site.data.keyword.cloud_notm}}.
- Creates a project where you do development and testing.
- Links the project and catalog together by using a trusted profile ID or an API key.
- Creates the product tile in the private catalog.
- Onboards the product version to the new private catalog.
- Marks the version as ready to share.
- Completes the onboarding of the workspace and the deployable architecture by moving the workspace to the catalog.
- Shows you the project ID, catalog ID, offering ID, and version locator.

Complete the following steps to create a deployable architecture and move all the deployed resources into a project configuration:

1. Run the [ibmcloud catalog utility create-product-from-workspace](/docs/cli?topic=cli-manage-catalogs-plugin#publish-utility-create) command.

   ```bash
   ibmcloud catalog utility create-product-from-workspace [--workspace-id ID] [--api-key KEY] [--trusted-profile-id ID] [--catalog-label LABEL] [--offering-label LABEL] [--project-name NAME] [--project-resource-group GROUP] [--target-version VERSION] [--variation-label LABEL]
   ```
   {: pre}

   `--api-key` and `--trusted-profile-id` are mutually exclusive. You need only one or the other. The `--project-name` parameter creates the project where you do development and testing. You might want to name it the same as your workspace ID for simplicity.
   {: tip}

   See the following example of the `utility create-product-from-workspace` command options.

   ```bash
   ibmcloud catalog utility create-product-from-workspace --workspace-id us-south.workspace.move.xxxx --api-key $IC_API_KEY --catalog-label myCatalog --offering-label myOffering --project-name myProject --project-resource-group Default --target-version 1.0.0 --variation-label test_standard
   ```
   {: pre}

   When the command is finished running, the command prompt returns `OK`.

1. Verify the results in Git by viewing the commit, repo, and the new release. You can see the `.tgz` file with the versions.
1. Verify the results in the {{site.data.keyword.cloud_notm}} console:
   1. Go to **Manage** > **Catalogs** > **Private catalogs**.
   1. Select the new private catalog.
   1. Select the new product in your private catalog, and then select the **Versions** tab. See that you have version `1.0.0` (or similar) of your deployable architecture. If you go to your private catalog, you see the same deployable architecture in a tile.
   1. Click the **Navigation menu** icon ![Navigation menu icon](../icons/icon_hamburger.svg "Menu") > **Projects**, and look at the new project.
   1. Select the **Configurations** tab to see the deployable architecture. See that it is in `draft` status.

## Creating the project configuration
{: #move-create-project-config}

The [ibmcloud project config-create](/docs/cli?topic=cli-projects-cli#project-cli-config-create-command) command creates a project configuration that makes the {{site.data.keyword.bpshort}} workspace run under a project where you can then create another version from the project and modify the deployable architecture as needed.

Before you begin this section, make sure that you have a deployable architecture in a private catalog and a running {{site.data.keyword.bpshort}} workspace and know its CRN. You can locate the workspace CRN either from the resource list in the {{site.data.keyword.cloud_notm}} console or by using the CLI.

   - {{site.data.keyword.cloud_notm}} console: In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation menu icon](../icons/icon_hamburger.svg "Menu") > **Resource list** > **Developer tools** > find the workspace, and copy the CRN.
   - {{site.data.keyword.cloud_notm}} CLI: Run the [ibmcloud schematics workspace get](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-get) command to fetch the workspace CRN details. For example, run `ibmcloud schematics workspace get --id workspace-id --output destination folder --json output.json` to fetch the {{site.data.keyword.bpshort}} workspace CRN details.

Complete the following steps to add a project configuration and move the {{site.data.keyword.bpshort}} workspace resources to the project:

1. Run the [ibmcloud project config-create](/docs/cli?topic=cli-projects-cli#project-cli-config-create-command) command.

   ```bash
   ibmcloud project config-create --project-id PROJECT-ID [--schematics-workspace-crn SCHEMATICS-WORKSPACE-CRN] [--definition-name DEFINITION-NAME] [--definition-locator-id DEFINITION-LOCATOR-ID]
   ```
   {: pre}

   The value for `--project-id` is the ID of the project that you created for your production deployment in the [before you begin](#move-prereqs) step. The value for `--definition-locator-id` is the same as the version locator from the previous section.
   {: tip}

   See the following example of `project config-create` command options.

   ```bash
   ibmcloud project config-create --project-id myProdProject --schematics-workspace-crn MyWorkspaceCRN --definition-name production-deployment --definition-locator-id myVersionLocator
   ```
   {: pre}

   When the command is finished running, the command prompt returns the name, state, and type.

1. Verify that your Terraform templates are deployed in your project:
   1. In the {{site.data.keyword.cloud_notm}} console, click the **Navigation menu** icon ![Navigation menu icon](../icons/icon_hamburger.svg "Menu") > **Projects**.
   1. Select your project that you first created for deployment production, and go to the **Configurations** tab.
   1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **View last deployment**.
   1. In the Deployment successful section, click **Changes deployed successfully** > **Logs** to view all the deployed resources in the project.

The {{site.data.keyword.bpshort}} workspace deployment is now included in the logs that are inside the new project.

If you want to verify the results by using the CLI, run the [ibmcloud project get](/docs/cli?topic=cli-projects-cli#project-cli-get-command) or [ibmcloud project list](/docs/cli?topic=cli-projects-cli#project-cli-list-command) commands to verify that your workspace was successfully moved to your project.

## Next steps
{: #move-next-steps}

To complete the remainder of the process for the deployable architecture, complete the following steps:
1. Configure deployment details for the deployable architecture.
1. Define IAM access.
1. Add an architecture diagram, prereqs, and highlights.
1. Add compliance information (controls).
1. Revalidate to receive a cost estimate.

For more information about how to complete the steps, see [Onboarding a customized deployable architecture to your private catalog](/docs/secure-enterprise?topic=secure-enterprise-onboard-da).

For more information, see [Adopting the Enterprise Architecture](/docs/adopt-enterprise-architecture?topic=adopt-enterprise-architecture-intro).
