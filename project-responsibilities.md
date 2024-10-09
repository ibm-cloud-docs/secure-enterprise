---

copyright:
  years: 2023
lastupdated: "2024-10-09"

subcollection: secure-enterprise

keywords: project responsibilities, incidents, operations, access, change management, security

---

{{site.data.keyword.attribute-definition-list}}

# Understanding your responsibilities when using projects
{: #responsibilities-projects}

Learn about the management responsibilities and terms and conditions that you have when you use projects. For a high-level view of the service types in {{site.data.keyword.cloud}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use projects. For the overall terms of use, see [{{site.data.keyword.cloud}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms).

## Incident and operations management
{: #incident-and-ops-projects}

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Monitor the status of a project| {{site.data.keyword.IBM_notm}} provides the ability for customers to monitor their project's lifecycle.  | Use the [needs attention widget](/docs/secure-enterprise?topic=secure-enterprise-needs-attention-projects) to monitor events that specifically impact the lifecycle of your project. You can also use the {{site.data.keyword.at_full_notm}} service to [audit events for a project](/docs/secure-enterprise?topic=secure-enterprise-at_events). |
{: row-headers}
{: caption="Responsibilites for incident and operations" caption-side="bottom"}
{: summary="The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Change management
{: #change-management-projects}

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

|Task  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Updates, fixes, and new features| {{site.data.keyword.IBM_notm}} provides regular updates and bug fixes, as well as new features following a continuous delivery model in a manner transparent to the customer.  | N/A |
|Creating and deploying configurations| {{site.data.keyword.IBM_notm}} provides the ability for customers to create and deploy configurations by using projects.  | Use projects to [configure and deploy a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-config-project).  |
|Deleting a project| {{site.data.keyword.IBM_notm}} provides the ability for customers to delete a project.  | Customers can [delete a project](/docs/secure-enterprise?topic=secure-enterprise-delete-project) whenever they need to. |
|Pulling deployable architecture changes into a project| {{site.data.keyword.IBM_notm}} provides the ability for customers to update the version of a deployable architecture in a project, should a new version become available. | Customers are [notified when a new deployable architecture version is available](/docs/secure-enterprise?topic=secure-enterprise-needs-attention-projects#na-version-update) so they can update their project accordingly. \n \n Customers can save their existing project data with API, CLI, or by [exporting the project.json](/docs/secure-enterprise?topic=secure-enterprise-setup-project#json-export) from the UI, which can be used as backup or as rollover plan if there is an issue.  \n \n Customers can then test the deployable architecture changes by deploying in a development or test environment before going to production.  This can all be done within the same project.  |
{: row-headers}
{: caption="Responsibilites for change management" caption-side="bottom"}
{: summary="The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Identity and access management
{: #iam-responsibilities-projects}

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Accessing projects| {{site.data.keyword.IBM_notm}} provides the ability to control user access to projects.  | Use Identity and Access Management (IAM) to [assign users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project). |
|Authorize a project to deploy a configuration| {{site.data.keyword.IBM_notm}} provides the ability to authorize a project to deploy a configuration.  | Choose an authentication method to authorize a project to deploy in an account. Use an API key or an existing secret to [authorize a project to deploy an architecture](/docs/secure-enterprise?topic=secure-enterprise-authorize-project) in an account.  |
{: row-headers}
{: caption="Responsibilites for identity and access management" caption-side="bottom"}
{: summary="The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Security and regulation compliance
{: #security-compliance-projects}

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

|  Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Meet security and compliance objectives| Provide a secure {{site.data.keyword.cloud_notm}} Projects service that complies with key standards. For more information about data security, see [How do I know that my data is safe?](/docs/overview/terms-of-use?topic=overview-security).  | Secure your workloads and data. Integrate tools into your toolchains that satisfy your security and compliance requirements. To learn more about securing your cloud apps, see [Security to safeguard and monitor your cloud apps](https://www.ibm.com/topics/cloud-native). To learn more about securing your data while you are using the {{site.data.keyword.cloud_notm}} Projects service, see [Securing your data](/docs/ContinuousDelivery?topic=ContinuousDelivery-cd_data_security&interface=ui). To learn more about regulatory compliance with the {{site.data.keyword.cloud_notm}} Projects service, see [Understanding tool integrations with {{site.data.keyword.IBM_notm}} for Financial Services](/docs/ContinuousDelivery?topic=ContinuousDelivery-integrations&interface=ui). |
{: row-headers}
{: caption="Responsibilites for security and regulation compliance" caption-side="bottom"}
{: summary="The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Disaster recovery
{: #disaster-recovery-projects}

Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Meet disaster recovery objectives| {{site.data.keyword.IBM_notm}} follows best practices for disaster recovery. All {{site.data.keyword.IBM_notm}} applications automatically recover and restart after any disaster event. For more information about disaster recovery see the [{{site.data.keyword.IBM_notm}} Disaster Recovery Plan](/docs/overview?topic=overview-zero-downtime#disaster-recovery).  | Customer can help meet DR objectives by deploying their project in a development or test environment before deploying to production. \n \n There are no other actions the customer needs to take to prepare for an event of a catastrophic failure in a region.   |
| Meet high availability objectives| {{site.data.keyword.IBM_notm}} is available globally and load balanced from a single URL. It is highly available and continues to run even if your resources are unavailable. For more information about high availability, please see the [{{site.data.keyword.IBM_notm}} service level objectives](/docs/overview?topic=overview-slo) and the [sample application architecture](/docs/overview?topic=overview-bcdr-app-recovery). | N/A |
| Back up project data| {{site.data.keyword.IBM_notm}} performs regular electronic backups of project data with Recovery Time Objective (RTO) and Recovery Point Objective (RPO) of hours as documented in the [{{site.data.keyword.IBM_notm}} Disaster Recovery Plan](/docs/overview?topic=overview-zero-downtime#disaster-recovery). | Customers are not responsible for backups for data stored by projects. However, customers can use project data via API, CLI, or by downloading the project.json from the UI and as a backup. |
| Restore project data| {{site.data.keyword.IBM_notm}} automatically restores project data from backups in case of a data loss event. | Customers can retrieve project data via API, CLI, or by downloading the project.json from the UI |
{: row-headers}
{: caption="Responsibilites for disaster recovery" caption-side="bottom"}
{: summary="The first column describes the task that the customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}
