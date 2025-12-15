---

copyright:
  years: 2022, 2025
lastupdated: "2025-12-15"

keywords: onboard, catalog management, private catalog, catalog manifest, software, automation, metadata

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Onboarding a deployable architecture to a private catalog
{: #onboard-da}

When you're ready to share your deployable architecture with other members of your organization, you can add it to a private catalog. Additionally, you can use the onboarding flow to validate your architecture.
{: shortdesc}

Onboarding your deployable architecture to a private catalog involves adding your source code to the catalog, then editing the details of your deployable architecture, followed by validating it. Then, you review the cost estimate and compliance information for your deployable architecture. When you're done, you can share the deployable architecture or publish it publicly in the {{site.data.keyword.cloud_notm}}.

![Onboarding a deployable architecture to a private catalog](images/deployable-architecture-flow-01.svg "Onboarding a deployable architecture to a private catalog"){: caption="High-level process for onboarding a deployable architecture to a private catalog" caption-side="bottom"}

## Before you begin
{: #onboard-before}

Before you can onboard your deployable architecture, be sure that you complete the following prerequisites.

* Verify that you're using a Pay-As-You-Go or Subscription account. See [Viewing your account type](/docs/account?topic=account-account_settings#view-acct-type) for more details.
* Verify that you have the required access to work with private catalogs and deployable architectures.
   * Manager role on the {{site.data.keyword.bplong_notm}} service
   * Editor role on the Catalog Management service
   * Viewer role on all resource groups in your account
   * SecretsReader role on the {{site.data.keyword.secrets-manager_short}} service if you plan to store your secure values in an instance of {{site.data.keyword.secrets-manager_short}}
   * Reader role on the {{site.data.keyword.sysdigsecure_short}} service
   * Other roles that are required for specific resources in your customized deployable architecture
* Create a private catalog.
* Ensure that you have the source code for your deployable architecture stored in a GitHub or GitLab repository. For help with getting your source code into a repository, see [Setting up your source code repository](/docs/sell?topic=sell-source-repo-setup).

Want to see how it works but don't have a deployable architecture ready to use? [Use our sample deployable architecture](https://github.com/IBM/catalog-create-sample-da){: external}.
{: tip}


## Packaging your source code
{: #package-source}

To create the `.tgz` file that you need to onboard your deployable architecture to a private catalog, you must create a release version of your source code. For help with creating a release, see [Managing releases in a repository](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository){: external}.

If you're using a private source code repository, be sure that you have a Git personal access token or a secret that is stored in [{{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-getting-started).


## Adding a deployable architecture to a private catalog
{: #add-catalog}

To add your deployable architecture to a private catalog, you can use the following steps.

1. In the {{site.data.keyword.cloud_notm}} console, go to the **Manage** > **Catalogs** > **Private catalogs** page of the console.
2. Select the private catalog that you want to add a product to. The catalog details page opens.
3. Click **Add product**. A side panel opens.
4. Select **Deployable architecture** for **Product type**.
5. Select **Terraform** or **Stack** as your **Delivery method**.
6. Select the type of repository where your source code is located.

	If your source code is located in a private repository, you need to authenticate by using a Git personal access token or a secret from [{{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-getting-started#getting-started).

7. Add a link to your source code in the **Source URL** field. It should look similar to `https://github.com/IBM-Cloud/terraform-sample/archive/refs/tags/v1.1.0.tar.gz`.

   If you are onboarding your deployable architecture for testing purposes, you do not need to have a `.tgz` file. You can provide the link to the root level of your architecture.
   {: note}

8. Select a **Variation**.

	A variation is a type of deployable architecture that applies differing capabilities or complexity to an existing deployable architecture. For example, there might be a *Quick start* variation to your deployable architecture that has basic capabilities for simple, low-cost deployment to test internally. And, you might have a *Standard* variation that is a bit more complex that is ready for use in production.

9. Enter the software version in the format of major version, minor version, and revision. For example, `1.0.0`. Typically, this version matches the version number of your release snapshot.
10. Select the **Category** that you want your deployable architecture to be grouped with in the catalog.
11. Click **Add product**. The product overview page is displayed.


## Editing your catalog entry
{: #edit-catalog-entry}

After you successfully onboard your deployable architecture to your private catalog, you must specify the information that a user sees when they attempt to use the architecture. The information includes descriptions of the product, links to documentation, and keywords that ensure that your product is easily findable.

1. Go to the **Manage** > **Catalogs** > **Private catalogs** page of the console.
2. Select the private catalog that you added your product to. The catalog details page opens.
3. Select the product that you previously onboarded.
4. Edit the way that your entry is shown in the catalog.
   1. In the **catalog entry details** section, click **Edit**.
   2. Review the information that was imported with your deployable architecture and make edits as needed.
   3. Verify that your entry is showing as expected by checking the **Catalog entry preview**.
   4. When you're finished making your selections, click **Save**.

5. Edit the **About** page for your product. When a user selects your product from the catalog, an **About** section is shown that allows them to learn more about your product and the features that are available.
   1. In the **Actions** drop-down, select **Edit product page**.
   2. Enter a description of your product that explains the product's value and benefits to your users.
   3. To add specific feature information, click **Features** > **Add feature**.
   4. Add product-level features that explain the processes, abilities, and results of the product. Users can see the high-level product features at the beginning of the product page that apply to the product as a whole, regardless of version or architecture variation differences. For example, if your product creates Virtual Private Clouds, you can add `Creates Virtual Private Clouds` as the feature title and `Virtual Private Clouds are created for you with the necessary underlying network components.` as the feature description. To add features for specific variations or versions, you can do so by [Adding highlights](#add-da-details).
   5. Click **Update**.



## Specifying details through the console
{: #specify-details}

Your users see the version-level information that you define as part of the catalog entry for your product. The information provided as part of this flow can help your users to understand the functionality of the individual components that are associated with it.

To ensure that your selections are carried over into your next release, you can generate a manifest file. The manifest file, `ibm_catalog.json`, is the source of truth for your catalog entry. It contains all of the information about your product and the selections that you've made. After you generate the file, you must add it to the root level of your source code repository. If you prefer to work in the code, the following sections can be configured directly through the mainfest file. For more information about how to structure the file, see [Locally editing your manifest file](/docs/secure-enterprise?topic=secure-enterprise-manifest-values).
{: note}


### Getting to the details
{: #set-up}

After you add your deployable architecture to a private catalog, you're able to use a step-by-step walkthrough in the console to update the general information about your product. To get to the console page, you can use the following steps.

1. Go to the **Manage > Catalogs > Private catalogs** page of the console.
2. Select the private catalog where you added your product. The catalog details page opens.
3. Select the product that you previously onboarded.
4. On the **Versions** tab, select the version of your product that you want to provide information for.
5. Use the following information as a guide to configure your deployable architecture details.


### Configuring your version details
{: #configure-version-details}

On the **Configure version** tab, you can review and update information about the specific version of your architecture. You configure deployment details, define the required IAM access, and detail change notices that you want your users to be aware of.

If you want to extend your deployable architecture, you can do so by stacking it with other architectures. You can include dependencies that your architecture requires, along with optional architectures that help expand your solution for a particular use case. For more information, go to [Extending a deployable architecture during onboarding](/docs/secure-enterprise?topic=secure-enterprise-extend-da). 
{: tip}

If your deployable architecture requires a specific Terraform runtime version, you can override the default version. If you included `TF_VERSION` as an input variable within your source code repository, it should have automatically been updated when you created your catalog entry.

Input variables are the parameters that users specify when they use your product. You can review and modify the input and output variables that were imported with your source code, or you can add variables to your deployable architecture as part of this step. When variables are added, you can update whether they're required, visible, or the format in which they need to be provided.

When you release a new version of your product, there might be changes that you want to notify your users of before they get started with the new version. You can separate the information into three categories - breaking changes, new features, and general updates.

* **Breaking changes**: Detail any changes to the code of the new version that can cause a disruptive experience to your users who are working with a previous version.
* **New features**: Highlight any new functionality provided in the new version that a user might want to take advantage of.
* **Updates**: Describe any general updates that were made to the new version. For example, bug fixes or improvements to existing features.


#### Including pre- and post-scripts
{: #include-scripts}

You can have a pre-script or post-script run for your deployable architectures before or after validating, deploying, and undeploying. Scripts are configured to a specific version of your deployable architecture as specified in the catalog manifest file, and must be run and validated through projects.

Scripts are optional for an offering but if they are used they are required to be in the repository in a directory named `scripts`. The script files themselves must conform to the following naming convention `<action>-<stage>-ansible-playbook.yaml`. Options for `action` include `deploy`, `validate`, and `undeploy`. Options for `stage` include `pre` and `post`. Only ansible scripts in the playbook format are supported at this time.

All scripts must be able to run more than once without failing. For example, a pre-deployment or post-deployment script must operate correctly, even if it is run several times. Post-deployment scripts might add resources to a catalog management database and must be sure not to add duplicate resources if run more than once.

For more information, including examples, see [Creating scripts for a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-understand-scripts).


### Adding deployable architecture details
{: #add-da-details}

When you make a deployable architecture available to other users in the cloud, you must provide the following information:

* An architecture diagram that details how the components in your deployable architecture work together.
* Any highlights that can help users to differentiate between which version or variation of your architecture might be best suited to their needs.


### Adding license agreements
{: #add-license-agreements}

If users are required to accept any license agreements beyond the {{site.data.keyword.cloud_notm}} Services Agreement, provide the URL to each agreement.

### Editing your readme file
{: #edit-readme}

Document the instructions for installing your deployable architecture in the readme file.

### Validating the version
{: #validate-version}

Select the target for validation. When a product is validated, the resources are deployed. For a stand-alone deployable architecture, the target can be either a Schematics workspace in your current account or a specific project. For a deployable architecture stack, you must use a project. Depending on the option that you select, more configuration information might be required. After your target is configured, you must provide the values for the input and output variables that are required for your architecture to successfully deploy to the target. After your variables are configured, you can validate the version.

Do not clean up the resources in your account until after you run the compliance evaluation in the managing security and compliance section.
{: note}

If the version fails validation because of a CRA scan, an administrator for the account can choose to override the failure and deploy anyway. If the validation fails for any other reason, it is highly recommended that you fix any issues that are found before publishing your offering.


### Reviewing cost
{: #review-cost}

Ensure that you fully understand the costs that are associated with deploying your architecture. The version must be validated before you can generate an estimated cost.

### Managing compliance
{: #manage-compliance}

When you make a deployable architecture available to others in your organization, you can specify the specific compliance controls that your architecture meets by using the default installation. Compliance with regulatory controls is evaluated by {{site.data.keyword.sysdigsecure_full}}.

1. Click **Add controls**.
2. Select your {{site.data.keyword.sysdigsecure_short}} instance and policy. You can choose to select a predefined policy or go to {{site.data.keyword.sysdigsecure_short}} and create one of your own.
3. Specify whether your deployable architecture meets all of the controls in the policy or whether it can satisfy the requirements for a subset of the controls.
4. If your architecture can meet only a subset of the controls, then you must select the controls that can be satisfied and add them.
5. Use {{site.data.keyword.sysdigsecure_short}} to confirm compliance with the controls that you identified. {{site.data.keyword.sysdigsecure_short}} uses an inventory to track compliance. The inventory results are updated daily. Deploy the resources that your architecture creates, and wait for the inventory results to be updated. For more information, go to [inventory](/docs/workload-protection?topic=workload-protection-inventory)
6. In the **Manage compliance** tab of the catalog UI, click **Add results**.
7. Select an **Instance** and click **Apply** to apply the latest inventory results.

### Reviewing requirements
{: #review-requirements}

When you've completed the walkthrough, you must review your selections and confirm that you are ready to share your product to your catalog. When you're ready, click **Ready to share**.

## Downloading the manifest
{: #download-manifest}

Whenever changes are made to your product configuration through the console, it is a best practice to generate and download your manifest file to ensure that your changes are picked up in future releases of your product.

To download a manifest, you can use the following steps.

1. Go to the **Manage** > **Catalogs** > **Private catalogs** page of the console.
2. Select the product that was previously onboarded. A details page opens.
3. On the **Versions** tab, select the version that you want to generate a manifest for.
4. From the **Actions** drop-down menu, select **Generate manifest**.
5. Add the file into the root folder of your source code repository as `ibm_catalog.json`.

## Downloading your catalog configuration
{: #export-catalog-config}

If you are onboarding [stacked deployable architectures from a project](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui), there are additional files that are generated in addition to your manifest file. If you have made updates to your catalog configuration by using the console, it is a best practices to download the files and add them to your source code repository so that your changes carry over into your next release.

1. Go to the **Manage** > **Catalogs** > **Private catalogs** page of the console.
2. Select the product that was previously onboarded. A details page opens.
3. On the **Versions** tab, select the version that you want to generate a manifest for.
4. From the **Actions** drop-down menu, select **Generate manifest**.
5. Add the files into the root folder of your source code repository.

## Adding a variation
{: #add-variation}

You can add more variations that are a new version of your architecture that is designed to build upon the funtions of the base deployable architecture. If you [created multiple variations](/docs/secure-enterprise?topic=secure-enterprise-create-variation-da) in separate working directories in your source repo and specified them in the `flavors` array in your `ibm_catalog.json` manifest file, you must onboard each variation separately.

At this point, you've already onboarded your first variation. Now, you can start back at [Adding a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-onboard-da#add-catalog) to onboard your next variation. Here are a few tips for onboarding your next variation:

* The source URL of the repo release will be the same for all of the variations within that release and they should be imported with the same version number. The product name and version number are how the variations are linked together and then result as options on the same catalog tile.
* On the Add deployable architecture details page, step 3 includes adding highlights. These are known as features in the `ibm_catalog.json` manifest file. You might have already added these in the manifest, so you can review them here. If not, go ahead and add some highlights. These should be short ability, process, capacity, or other features of this specific architecture. You will use the same highlight "Name" across all variations. The description is where there should be differences. This enables users to evaluate the differences in the architectures by using the text highlights on the catalog details page.

## Next steps: Sharing and publishing
{: #onboard-next}

Now that your deployable architecture is added to a private catalog and the details are set, you're ready to share the product with other members of your organization. For help with sharing, see [Sharing your product](/docs/secure-enterprise?topic=secure-enterprise-catalog-share&interface=ui).

If you want to publish your deployable architecture to the {{site.data.keyword.cloud_notm}} catalog, you can use Partner Center to get approval and publish for all users to take advantage of the solution that you built. For more information, see [Publishing your deployable architecture](/docs/sell?topic=sell-da-publish).
