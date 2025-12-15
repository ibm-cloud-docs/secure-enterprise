---

copyright:

  years: 2024, 2025

lastupdated: "2025-12-15"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Managing compliance information for your deployable architecture
{: #format-controls}

By using the `ibm_cloud.json` manifest file, you can claim that your deployable architecture meets specific compliance requirements. After you onboard and publish your deployable architecture to the catalog, users can view which controls or {{site.data.keyword.sysdigsecure_full_notm}} policies your product adheres to. You verify the compliance information before you onboard your deployable architecture.
{: shortdesc}

Here's an example from the VSI on VPC landing zone's catalog page where the Standard variation meets the {{site.data.keyword.framework-fs_full}} v1.6.0 profile:

![Deployable architecture compliance on the catalog details page](images/da-compliance-mapping.png "Deployable architecture compliance on the catalog details page"){: caption="Deployable architecture compliance" caption-side="bottom"}

The process to claim compliance for your deployable architecture includes steps that must be completed before and during the onboarding process to a catalog:

1. [Set up an instance of {{site.data.keyword.sysdigsecure_short}}](#profile)
1. [Add compliance information to your `ibm_cloud.json` manifest file](#compliance-manifest)
1. [Deploy your resources and add your inventory information to your deployable architecture when you onboard](#apply-scc-scan)

## Setting up {{site.data.keyword.sysdigsecure_short}}
{: #profile}

Set up an instance of {{site.data.keyword.sysdigsecure_short}} and implement Cloud Security Posture Management (CSPM) for your {{site.data.keyword.cloud_notm}} account: 

1. [Provision an instance of {{site.data.keyword.sysdigsecure_short}}](/docs/workload-protection?topic=workload-protection-provision&interface=ui) from the catalog if you haven't done so already.
1. Complete the steps to [integrate with either an existing {{site.data.keyword.sysdigsecure_short}} instance or a new instance](/docs/workload-protection?topic=workload-protection-cspm-implement&interface=ui). 
1. {{site.data.keyword.sysdigsecure_short}} provides [default policies to verify compliance](/docs/workload-protection?topic=workload-protection-about&interface=ui#about-available-policies), or you can [create your own custom policies](/docs/workload-protection?topic=workload-protection-cspm-best-practices&interface=ui#cspm-best-practices-manage-policies). 

## Updating the compliance information in the manifest
{: #compliance-manifest}

After you identify the {{site.data.keyword.sysdigsecure_short}} policy you'd like to use, you must add that information to the `ibm_cloud.json` catalog manifest file in your source repo.

1. If one does not exist, create a [catalog manifest file](/docs/secure-enterprise?topic=secure-enterprise-manifest-values) at the root of your repo. For an example catalog manifest file, see the [terraform-ibm-landing-zone repo](https://github.com/terraform-ibm-modules/terraform-ibm-landing-zone/blob/main/ibm_catalog.json){: external}.
1. Open the `ibm_catalog.json` file.
1. Find or add the `flavors.compliance` field for the variation (flavor) that you want to update.
1. Set `authority` to `scc-wp-v1`.
1. Find or add a `profiles[]` array:
   1. Set `profile_name` to the [policy display name in {{site.data.keyword.sysdigsecure_short}}](/docs/workload-protection?topic=workload-protection-posture-policies&interface=ui#access-posture-controls). 
   1. Set `profile_version` to the version of the policy in {{site.data.keyword.sysdigsecure_short}}.

   For example:
   ```json
      "authority": "scc-wp-v1`",
        "profiles": [
          {
            "profile_name": "IBM Cloud for Financial Services",
            "profile_version": "1.3.0"
          }
        ]
   ```
   {: codeblock}

1. Save the file.

## Adding compliance verification during onboarding
{: #apply-scc-scan}

When you [onboard your deployable architecture in the console](/docs/secure-enterprise?topic=secure-enterprise-onboard-da&interface=ui#manage-compliance), you can add the inventory from {{site.data.keyword.sysdigsecure_short}} so that users can see the claimed compliance when they evaluate your product in the catalog.

In {{site.data.keyword.sysdigsecure_short}}, your inventory is updated once every day. You must deploy your resources and wait for the inventory to be updated before you add the inventory to your catalog listing. For more information, go to [Inventory](/docs/workload-protection?topic=workload-protection-inventory)
{: important}

1. On the Manage compliance page, click **Add inventory**.
1. Select the {{site.data.keyword.sysdigsecure_short}} instance that you provisioned in the previous step.
1. Click **Apply**. 

Now that your inventory is added, you can complete onboarding and choose to share the deployable architecture to other accounts or enterprises, or publish to the {{site.data.keyword.cloud_notm}} catalog.

## Cleaning up your resources
{: #clean-up}

To add compliance information to your deployable architecture, you had to create the resources in your account and a {{site.data.keyword.sysdigsecure_short}} instance. To reduce future costs, you can delete all of the resources that you created during this process that you no longer need.
