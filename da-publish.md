---
copyright:
  years: 2024
lastupdated: "2024-10-09"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# How do I decide where to share my solution?
{: #publish-da-options}

The options for sharing and publishing differ based on whether you have a module or deployable architecture, who you want to share the solution with, and the level of support that is available for the solution.
{: shortdesc}

## Sharing and publishing options
{: #publishing-options}

First, you need to know whether you're creating a module or deployable architecture. It's important to note that deployable architectures can be deployed from private catalogs or the {{site.data.keyword.cloud}} catalog by using projects, but modules can't be deployed from private catalogs. Let's look at the available options for each.

| Component        | Sharing and publishing options |
|------------------|--------------------|
| Module           | Private catalog |
| Deployable architecture | Private catalog  \n {{site.data.keyword.cloud_notm}} catalog |
{: caption="Publishing options" caption-side="top"}



Let's learn a little more about each of these options.

Private catalog
:   Private catalogs provide a way to centrally manage access to products in the {{site.data.keyword.cloud_notm}} catalog and your own catalogs. You can customize the public catalog and your private catalogs to make specific solutions available to users in your account. By doing so, you can ensure that your catalogs are relevant to your business. You can share products in your private catalog with users in your account, account groups within your enterprise, the entire enterprise, and even other enterprises that you have access to. By sharing your offering, any user within the account, enterprise, or account groups can configure and deploy an instance.







{{site.data.keyword.cloud_notm}} catalog
:   The [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} is the official collection of generally available supported products and offerings from {{site.data.keyword.cloud_notm}} and approved third-party partners. All products onboarded and published to the catalog must complete the requirements in Partner Center.

## Choosing where to share or publish deployable architectures
{: #da-publish}

If you plan to build a deployable architecture, you might choose to share it with just people in your enterprise or the wider cloud community in the {{site.data.keyword.cloud_notm}} catalog.

| Scenario                                   |  Publishing location |
|----------------------------------------------|--------------------|
|I'm a development team in an enterprise that's building a proprietary solution for my company | Private catalog |
|I'm creating an experimental type of pattern | Private catalog |
|The pattern applies broadly to a larger customer base | {{site.data.keyword.cloud_notm}} catalog |
|My team has development resources to provide support and ongoing maintenance | {{site.data.keyword.cloud_notm}} catalog |
{: caption="Scenarios for publishing location" caption-side="top"}

If you plan to offer your deployable architecture in the {{site.data.keyword.cloud_notm}} catalog, there are required tasks and approvals that must be completed in Partner Center. For more information, see the [Checklist for selling deployable architectures](/docs/sell?topic=sell-checklist-da).

## Next steps: Creating the solution automation
{: #next-automation}

Now that you have a plan and architecture design and you know where you want to share or publish your solution, you can get started creating the automation. Use the following resources to help you get started creating your solution.

* [Creating a module](/docs/secure-enterprise?topic=secure-enterprise-create-module)
* [Creating a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-create-da)
* [Stacking deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui)
