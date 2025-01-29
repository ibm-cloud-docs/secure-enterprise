---

copyright:
  years: 2022, 2025


lastupdated: "2025-01-29"

keywords: project access, project, project cost, project estimate, project users

subcollection: secure-enterprise

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ about projects
{: #project-faqs}

FAQs for {{site.data.keyword.cloud}} projects might include questions about support options, configurations, access to projects, and cost estimation.
{: shortdesc}

## What are {{site.data.keyword.cloud_notm}} projects?
{: #project-log-issue}
{: faq}
{: support}

As an enterprise, you use projects to ensure that the configuration of your deployable architecture is always compliant, cost effective, and secure. Projects are a named collection of configurations that are used to manage related resources and Infrastructure as Code (IaC) deployments across accounts.

## How do I add users to my project?
{: #project-add-users}
{: faq}

To add a user to a project, they must be a member of your account with correct IAM access roles assigned.

To assign access to the {{site.data.keyword.cloud_notm}} Projects service, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**, and then select **Users**.
1. Click the user or access group that you want to assign access, then go to **Access** > **Assign access**.
1. For the service, select **Project**. Then, click **Next**.
1. Scope the access to **All resources** or **Specific resources**. Then, click **Next**.
1. Select any combination of roles or permissions, and click **Review**.
1. Click **Add** to add your policy configuration to your policy summary.
1. Click **Assign**.

In addition, users must be assigned the Editor and Manager role on the {{site.data.keyword.bpshort}} service and the Viewer role on the resource group for the project.

## How do I estimate project costs?
{: #project-cost}
{: faq}
{: support}

During the validation process, the starting costs for the project are estimated. You can view the cost details associated with the project from the validation modal after your changes to the configuration are saved and validated.

## Who has access to my project?
{: #access-to-project}
{: faq}
{: support}

Any user that is a member of your account that is assigned access to the {{site.data.keyword.cloud_notm}} Projects service, {{site.data.keyword.bpshort}}, and the resource group for your project can access your project.
