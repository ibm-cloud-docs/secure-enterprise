---

copyright:
  years: 2023
lastupdated: "2025-12-09"

keywords: catalog, catalogs, private catalogs, account catalogs, catalog visibility, module visibility, import module, module registry, terraform module

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Onboarding modules to a private catalog
{: #onboard-modules}

A module is a stand-alone unity of automation code that can be reused by developers and shared as part of a larger system. You can choose to create your own module or select one from the [{{site.data.keyword.cloud_notm}} Terraform module](https://github.com/terraform-ibm-modules) repository. To share a module with others in your organization or to validate the deployment, you can add the module to a private catalog.
{: shortdesc}

## Before you begin
{: #onboard-modules-prereq}

Before you can onboard your module, be sure that you complete the following prerequisites.

* Verify that you're using a Pay-As-You-Go or Subscription account. See [Viewing your account type](/docs/account?topic=account-account_settings#view-acct-type) for more details.
* Verify that you have the required access to work with private catalogs and modules.
   * Manager role on the {{site.data.keyword.bplong_notm}} service
   * Editor role on the Catalog Management service
   * Viewer role on all resource groups in your account
   * SecretsReader role on the {{site.data.keyword.secrets-manager_short}} service if you plan to store your secure values in an instance of {{site.data.keyword.secrets-manager_short}}
   * Reader role on the  {{site.data.keyword.compliance_short}}   service
   * Other roles that are required for specific resources in your customized module.
* Create a private catalog.
* Ensure that you have the source code for your module stored in a GitHub or GitLab repository. For help with getting your source code into a repository, see [Setting up your source code repository](/docs/sell?topic=sell-source-repo-setup).


## Packaging your source code
{: #onboard-modules-source-code}

To create the `.tgz` file that you need to onboard your module to a private catalog, you must create a release version of your source code. For help with creating a release, see [Managing releases in a repository](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository){: external}.

If you're using a private source code repository, be sure that you have a [Git personal access token](https://github.com/settings/tokens/new){: external} or a secret that is stored in [{{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-getting-started#getting-started).



## Adding a module to your private catalog
{: #onboard-modules-add-public-repo-ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to the **Manage** > **Catalogs** > **Private catalogs** page of the console.
2. Select the private catalog that you want to add a module to. The catalog details page opens.
3. Click **Add product**. A side panel opens.
4. Select **Module** for **Product type**.
5. Select **Terraform** as your **Delivery method**.
6. Select the type of repository where your source code is located.

	If your source code is located in a private repository, you need to authenticate by using a [Git personal access token](https://github.com/settings/tokens/new){: external} or a secret from [{{site.data.keyword.secrets-manager_short}}](/docs/secrets-manager?topic=secrets-manager-getting-started#getting-started).

7. Add a link to your source code in the **Source URL** field. It should look similar to `https://github.com/terraform-ibm-modules/terraform-ibm-cos/archive/refs/tags/v7.0.5.tar.gz`.
8. Select the **Example** that you want to use.
9. Enter the software version in the format of major version, minor version, and revision. For example, `1.0.0`. Typically, this version matches the version number of your release snapshot.
10. Select the category that you want your module to be grouped with in the catalog.
11. Click **Add product**. An overview page is displayed.


## Adding examples to your module in a private catalog
{: #add-examples}

To add additional examples to your module, you can add a new version.

1. Go to the **Manage** > **Catalogs** > **Private catalogs** page of the console.
2. Select the private catalog that you added your module to. The catalog details page opens.
3. Select the module that you want to provide more details for.
4. On the **Versions** tab, click **Add version**.
5. Follow the process outlined in the previous section - [Adding a module to your private catalog](#onboard-modules-add-public-repo-ui).



## Editing your catalog entry
{: #onboard-modules-tile-ui}

After you successfully add your module to a private catalog, you can specify the information that users sees when they attempt to use your module. The information includes descriptions of the module, links to documentation, and keywords that ensure that your module is easily findable.

1. Go to the **Manage** > **Catalogs** > **Private catalogs** page of the console.
2. Select the private catalog that you added your module to. The catalog details page opens.
3. Select the module that you want to provide more details for.
4. Edit the catalog entry details for your module.
   1. In the **catalog entry details** section, click **Edit**.
   2. Review the information that was imported with your deployable architecture and make edits as needed.
   3. Verify that your entry is showing as expected by checking the **Catalog entry preview**.
   4. When you're finished making your selections, click **Save**.
5. Edit the **About** page for your module. When users select your module from the catalog, an **About** section is shown that allows them to learn more about the features that are available.
   1. In the **Actions** drop-down, select **Edit product page**.
   2. Enter a description of your module that explains the module's value and benefits to your users.
   3. To add specific feature information, click **Features** > **Add feature**.
   4. Add module-level features that explain the processes, abilities, and results of the module.
   5. Click **Update**.


## Specifying version details through the console
{: #onboard-modules-catalog-version-details-ui}

Your users see the example-level information that you define as part of the catalog entry for your module. The information provided as part of this flow can help others to understand the functionality of the individual components that are associated with it.

After you specify details through the console, you can generate a manifest file and add it to your source code. By doing so, you can ensure that your specifications are carried over into future releases and you don't need to reconfigure them with each release.
{: important}

### Getting to the details
{: #set-up-details}

After you add your module to a private catalog, you can use a step-by-step walkthrough in the console to update general information about your module. To get to the console page, you can use the following steps.

1. Go to the **Manage** > **Catalogs** > **Private catalogs** page of the console.
2. Select the private catalog that you added your module to. The catalog details page opens.
3. Select the module that you want to provide more details for.
4. On the **Versions** tab, select the specific version that you want to provide information for. You can use the console to update any of the sections that you need to.


### Configuring version details
{: #onboard-modules-version-details}

On the **Configure version** tab, you can review and update information about the specific version of your module. You also configure deployment details, define the required IAM access, and detail the change notices that you want your users to be aware of.

If your module requires a specific Terraform runtime version, you can override the default version. If you included `TF_VERSION` as an input variable within your source code repository, it was automatically updated when you created your catalog entry.

Input variables are the parameters that users specify when they use your module. You can review and modify the input and output variables that were imported with your source code, or you can add variables to your module as part of this step. When variables are added, you can update whether they're required, visible, or the format in which they need to be provided.

When you release a new version of your module, there might be changes that you want to notify your users of before they get started with the new version. You can separate the information into three categories - breaking changes, new features, and general updates.

* **Breaking changes**: Detail any changes to the code of the new version that can cause a disruptive experience to your users who are working with a previous version.
* **New features**: Highlight any new functionality provided in the new version that users might want to take advantage of.
* **Updates**: Describe any general updates that were made to the new version. For example, bug fixes or improvements to existing features.


### Adding module details
{: #onboard-modules-add-details-ui}

When you make a module is made available to other users in the cloud, you must provide the following information:

* An architecture diagram that details how the components in your module work together.

   Each example of a module must have at least one architecture diagram that outlines the relationships, constraints, and boundaries between the components included in the module example. Architecture diagrams must be provided in `.svg` format.

* Any highlights that can help users to differentiate between which version or example of module might be best suited to their needs. For example, you might want to list the number of VPCs, network connectivity type, high availability capability, or other important factors that might be used to differentiate each example.


### Adding license agreements
{: #onboard-modules-add-license-ui}
{: ui}

If users are required to accept any license agreements beyond the {{site.data.keyword.cloud_notm}} Services Agreement, provide the URL to each agreement.


### Editing your readme file
{: #onboard-modules-readme-edit-ui}
{: ui}

Document the instructions for running your module in the readme file.


### Validating the version
{: #onboard-modules-validate-ui}
{: ui}

The validation process tests your Terraform template by running it from the {{site.data.keyword.bpshort}} service that you configure. A successful validation ensures that users can use your module with your default input variables. You must validate your module before you can share it. To monitor the progress of the validation process, click **View logs** from the **Actions** menu. The Schematics workspace is opened.

### Reviewing cost
{: #onboard-modules-cost-ui}
{: ui}

Ensure that you fully understand the costs that are associated with onboarding your module. The version must be validated before you can generate an estimated cost.



## Managing compliance
{: #onboard-modules-compliance}

When you onboard a module to a private catalog, you can specify compliance controls that your module meets when it is run. Compliance with regulatory controls is evaluated by {{site.data.keyword.compliance_long}}. For more information, see [Targeting resources to evaluate](/docs/security-compliance?topic=security-compliance-scopes&interface=ui).

1. Click **Add claims**.
2. Select a profile. The profile is pulled from the {{site.data.keyword.compliance_short}} service. You can choose to select a predefined profile or go to {{site.data.keyword.compliance_short}} and create one of your own.
3. Specify whether your module meets all of the controls in the profile or whether it can satisfy the control requirements for a subset of the controls.
4. If your module can meet only a subset of the controls, then you must select the controls that can be satisfied and add them as claims.
5. Use {{site.data.keyword.compliance_short}} to confirm the claims that you've identified.

	1. In the {{site.data.keyword.cloud_notm}} console, click the **menu** icon ![Menu icon](../icons/icon_hamburger.svg) > **Security and Compliance** to access {{site.data.keyword.compliance_short}}.
	2. Create an attachment between the profile that you selected and a scope that contains the resources that are created during your validation deployment.
	3. Run a scan and wait for the results to be available.
6. In the **Manage compliance** tab of the catalog UI, click **Add scan**.
7. Select an **Instance**, **Profile**, and the specific scan that you want to add.
8. Click **Add** 



### Reviewing requirements
{: #onboard-modules-review-reqs-ui}

When you've completed the walkthrough, you must review your selections and confirm that you are ready to share your module to your catalog. When you're ready, click **Ready to share**.


## Downloading the manifest
{: #module-download-manifest}

Whenever changes are made to your module configuration through the console, it is a best practice to generate and download a new manifest file to ensure that your changes are picked up in future releases of your modue.

To download a manifest through the UI, you can use the following steps.

1. Go to the **Manage** > **Catalogs** > **Private catalogs** page of the console.
2. Select the module that was previously onboarded. A details page opens.
3. On the **Versions** tab, select the version that you want to generate a manifest for.
4. From the **Actions** drop-down menu, select **Generate manifest**.
5. Add the file into the root folder of your source code repository as `ibm_catalog.json`.



## Next steps
{: #onboard-modules-share-ui}

Now that your module is added to a private catalog and the details are set, you're ready to share it with other members of your organization. For help with sharing, see [Sharing your product](/docs/secure-enterprise?topic=secure-enterprise-catalog-share&interface=ui).
