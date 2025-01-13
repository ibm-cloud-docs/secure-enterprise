---

copyright:
  years: 2024, 2025
lastupdated: "2025-01-13"

keywords: customize, deployable architecture, set up, security and compliance center, custom profiles

subcollection: secure-enterprise

content-type: tutorial
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}


# Preparing your account to scan for security and compliance
{: #compliance-evaluation-resources}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

With {{site.data.keyword.compliance_full}}, as a security focal within your enterprise, you can scan your account resources to evaluate whether they meet your enterprise's regulatory compliance requirements.
{: shortdesc}

This tutorial walks you through the process of setting up the automatic evaluation of your [VSI on VPC landing zone deployable architecture](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-vsi-ef663980-4c71-4fac-af4f-4a510a9bcf68-global){: external} resources against the {{site.data.keyword.framework-fs_notm}} profile. This workflow requires you to set up an attachment to target a resource group in your account that is to contain the resources for the deployable architecture.

You can follow the [Using {{site.data.keyword.cloud_notm}} deployable architectures to build a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-basic-custom) tutorial to customize the deployable architecture to meet your needs and offer it through a private catalog to your enterprise users. Or, you can use the unedited deployable architecture from the catalog. Either way, with this tutorial, you can scan your account resources on a recurring schedule to get ready for audits.


## Before you begin
{: #security-prereqs}

Before you get started, be sure that you have the following prerequisites.

* Check whether you are part of the relevant [access group](/docs/enterprise-management?topic=enterprise-management-enterprise-iam-ag-tutorial) in your enterprise's account. The access group must have the required permissions to select a profile, set up attachments, run scans, view detailed results, and more.
* To manage your security and compliance evaluations, you need to make sure that you have the [**Editor** platform role or higher](/docs/security-compliance?topic=security-compliance-assign-roles).
* Make sure that you have an [authorization](/docs/account?topic=account-serviceauth) with **Writer** access permissions in place to enable communication between {{site.data.keyword.compliance_short}} and {{site.data.keyword.cos_full_notm}}.

## Setting up storage for your scan results
{: #storage-setup-tutorial}
{: step}

You must connect the {{site.data.keyword.compliance_short}} service to a {{site.data.keyword.cos_short}} bucket to store your results. Then, you can create an attachment.

1. In the {{site.data.keyword.cloud_notm}} Dashboard, go to **Resource list** and select the **{{site.data.keyword.compliance_short}}** instance that you want to work with.
2. In the {{site.data.keyword.compliance_short}} console, go to the **Settings** page.
3. In the **Storage** section, click **Connect**.
4. Select the correct **{{site.data.keyword.cos_short}} instance** from the dropdown list.
5. Select the **{{site.data.keyword.cos_short}} bucket** where you want to store your scan results.
6. Click **Connect**.

## Creating an attachment
{: #attachment-create-tutorial}
{: step}

In this scenario, your solution architect and development team plan to customize and deploy the [VSI on VPC landing zone architecture](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-vsi-ef663980-4c71-4fac-af4f-4a510a9bcf68-global){: external}. Working as a team, you can determine which specific deployable architecture resources are to be contained in the resource group that you plan to use. Then, you can set up an attachment between the resource group and the profile that best meets your enterprise's regulatory objectives.

The attachment that you're creating validates the group of specific deployable architecture resources in your account against the {{site.data.keyword.framework-fs_notm}} profile. The {{site.data.keyword.framework-fs_notm}} profile evaluates the deployable architecture resources that the resource group contains, whether they're created already or are planned to be in the future.

1. In the {{site.data.keyword.cloud_notm}} Dashboard, go to **Resource list** and select the **{{site.data.keyword.compliance_short}}** instance that you want to work with.
2. In the {{site.data.keyword.compliance_short}} console, navigate to the **Attachments** page and click **Create**.
3. Provide a name and description for your attachment. Click **Next**.
4. Select the {{site.data.keyword.framework-fs_notm}} profile in the **Profile** dropdown list. Then, click **Next**.
5. Define a scope by selecting the resource group that you created with your team and specify the resources that you want to exclude from your scope. Click **Next**.
6. Select **Weekly** as the frequency at which you want to evaluate your resources. With this interval, the scan starts as soon as the attachment is created. Click **Next**.

   If your development team hasn't deployed the deployable architecture resources to the resource group yet, select **Never**. When the team adds the resources to the resource group, you can return and update the frequency of the scans.
   {: note}

7. Review your choices and click **Create**.

When you create your attachment, a scan is scheduled. When the scan completes, your results are available in the {{site.data.keyword.compliance_short}} dashboard. If your results are not updated, review the [troubleshooting guide](/docs/security-compliance?topic=security-compliance-ts-cache).
{: note}

## Next Steps
{: #profile-tutorial-next}

Now that you set up the attachment to scan the resource group that is to contain the VSI on VPC landing zone deployable architecture resources, you can view scan results and prepare for audits. For more information, check out [Viewing results](/docs/security-compliance?topic=security-compliance-results&interface=ui).

When your development team is ready to customize the architecture, they can [use the {{site.data.keyword.compliance_short}} attachment](/docs/secure-enterprise?topic=secure-enterprise-config-project&interface=ui#cra-validate-failure) that you just created to validate the architecture before they deploy it.
