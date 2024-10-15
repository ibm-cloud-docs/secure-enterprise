---

copyright:

  years: 2022, 2024

lastupdated: "2024-10-09"

keywords: project access, iam projects, assigning project access, assign access, access project

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Assigning access to projects
{: #access-project}

Projects are controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM). As an administrator on a project, you can grant users access to view and edit projects, approve changes, and deploy or destroy configuration resources. Projects also requires authorization with other {{site.data.keyword.cloud_notm}} services in order for users to validate and deploy configurations.
{: shortdesc}

## Actions and roles for the {{site.data.keyword.cloud_notm}} Projects service
{: #service-access-projects}

The following table includes the actions that users can take when they are assigned a specific role on the {{site.data.keyword.cloud_notm}} Projects service. Review the following information to make sure that you are assigning the correct level of access to your users.

| Role | Definition |Project Permissions |
|-------------|---------------------|---------------------|
| Viewer | Viewers can perform read-only actions within a project. | View a project (including the project.json) \n \n Find a project by using Global Search |
| Operator | Operators can perform the same actions as viewers, with more permissions beyond the viewer role, including planning project deployments | All viewer project permissions \n \n Validate a configuration \n \n Edit a configuration |
| Editor | Editors can perform the same actions as operators, with more permissions beyond the operator role, including creating projects and deploying resources. | All viewer and operator project permissions \n \n Create a project \n \n Edit a project \n \n Edit project settings \n \n Delete a project \n \n Create a configuration \n \n  Discard a draft configuration \n \n Approve configuration changes \n \n Deploy configuration changes \n \n Destroy resources \n \n Create an environment \n \n Edit an environment \n \n Delete an environment |
| Administrator | Administrators can perform the same actions as editors, with more permissions beyond the editor role, including updating project statuses and planning new or changed project deployments. | All viewer, operator, and editor project permissions \n \n Force approve changes that failed validation |
{: caption="Access roles for projects" caption-side="top"}

In addition to access on the {{site.data.keyword.cloud_notm}} Projects service, you must be assigned the following IAM privileges on the project tooling resources within the account:

* The Viewer role on the resource group for the project, which allows a resource group for the project to be selected to deploy the tooling services.

## Assigning access in the console
{: #console-project-service}

To assign access to the {{site.data.keyword.cloud_notm}} Projects service, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**, and then select **Access groups**.
1. Select the access group that you want to assign access to, then go to **Access** > **Assign access**.
1. For the service, select **{{site.data.keyword.cloud_notm}} Projects**. Then, click **Next**.
1. Scope the access to **All resources** or **Specific resources**. Then, click **Next**.
1. Select any combination of roles or permissions, and click **Next**.
1. Optionally, add conditions to the policy. Then, click **Review**.
1. Click **Add** to add your policy configuration to your access summary.
1. Click **Assign**.

It's a best practice to [assign access to an access group](/docs/secure-enterprise?topic=secure-enterprise-access-enterprises) and then add users to the access group, instead of assigning access to users one by one. However, you can assign access to a single user by going to **Manage** > **Access (IAM)** > **Users** and selecting the user you want to assign access to.
{: tip}

## Granting access between the Projects service and other {{site.data.keyword.cloud_notm}} services
{: #user-create-role}

Before a project can validate or deploy configurations, the Projects service must be authorized in your account to communicate with other {{site.data.keyword.cloud_notm}} services. The following table lists the required authorizations. This authorization is only required once.

An IAM administrator or a user with the required roles on those services can automatically grant authorizations by creating a project in your account, or they can [create the service to service authorizations manually](/docs/secure-enterprise?topic=secure-enterprise-serviceauth&interface=ui). 
{: tip}

| Role | Source | Target | Source account |
| ------------- | --------------------- | --------------------- | --------------------- |
| Manager and Administrator | {{site.data.keyword.cloud_notm}} Projects service | {{site.data.keyword.bpshort}} service | This account |
| Viewer | {{site.data.keyword.cloud_notm}} Projects service | Resource group only \n \n All resource groups in the account | This account |
| Viewer | {{site.data.keyword.cloud_notm}} Projects service | Catalog Management service | This account |
| Viewer and SecretsReader| {{site.data.keyword.cloud_notm}} Projects service | {{site.data.keyword.secrets-manager_short}} service | This account |
{: caption="Projects service to service authorizations " caption-side="top"}
