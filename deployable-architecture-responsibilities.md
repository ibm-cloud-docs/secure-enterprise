---

copyright:

  years: 2023, 2025

lastupdated: "2025-05-27"

subcollection: secure-enterprise

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Understanding your responsibilities when you use {{site.data.keyword.IBM_notm}} deployable architectures
{: #responsibilities-deployable-architectures}

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.IBM}} deployable architectures. For a high-level view of the service types in {{site.data.keyword.cloud}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for using {{site.data.keyword.cloud_notm}} products](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use an {{site.data.keyword.IBM_notm}} deployable architecture. For the overall terms of use, see [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview?topic=overview-terms).

## Incident and operations management
{: #incident-and-ops-da}

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Monitor the status of a deployable architecture | {{site.data.keyword.IBM_notm}} provides the ability for customers to monitor the lifecycle of the deployable architecture.  | Use the [needs attention widget](/docs/secure-enterprise?topic=secure-enterprise-needs-attention-projects) or [enable Event Notifications](/docs/secure-enterprise?topic=secure-enterprise-event-notifications-events) to monitor events that specifically impact the lifecycle of your deployable architecture.  |
| Monitor the status of a product spun up by your deployable architecture | IBM provides the ability for customers to monitor the lifecycle of the instances. | Use the resource list, service instance pages, or the [Status](https://cloud.ibm.com/status){: external} page to monitor events that specifically impact your service instance. |
{: row-headers}
{: caption="Responsibilities for incident and operations" caption-side="bottom"}
{: summary="The first column describes the task that a customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Change management
{: #change-management-da}

Change management includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

| Task  | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Creation of the IBM Cloud deployable architectures | {{site.data.keyword.IBM_notm}} provides the base pattern as a deployable architecture for instantiation through Terraform.  | N/A  |
| Must use supported version of [IBM Cloud Terraform Provider](/docs/secure-enterprise?topic=secure-enterprise-responsibilities-deployable-architectures#change-management-da). | {{site.data.keyword.IBM_notm}} publishes Terraform provider of all Terraform enabled services on IBM Cloud.  | Customers should use the latest major version. Terraform Providers version requirements are documented within the `version.tf` file for each deployable architecture. |
| Third-party Terraform Providers used within templates | N/A  | Customer is responsible for any use of third-party Terraform code that is used with the deployable architecture.  |
| Running default configuration (out of the box)| {{site.data.keyword.IBM_notm}} provides the ability for customers to create and deploy configurations of deployable architectures by using projects, Terraform-as-a-service, or the projects CLI.  | Use projects to [configure and deploy a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-config-project).  |
| Running templates locally by using Terraform directly | N/A  | Customer can run the deployable architecture on their local system.  |
| Customize modules or deployable architectures with pre-supported modules | {{site.data.keyword.IBM_notm}} provides and supports base Terraform modules for services on IBM Cloud, and provides preset JSON configuration overrides for templates. | Customer can use these base Terraform modules to extend their base deployable architecture pattern.  |
| Workload Management (Application Migration and Backup/Restore) | N/A | Customer responsibility to manage and migrate application workloads. |
| Fixes, new features, and updates to the next major deployable architecture release  | {{site.data.keyword.IBM_notm}} provides regular updates, bug fixes, and new features in a continuous delivery model that is apparent to the customer. {{site.data.keyword.IBM_notm}} provides a migration path when possible. | N/A |
| Keep deployed services and resources up to date | N/A | Apply fixes and updates to the compute resources that are created from the deployable architecture. These resources are not updated through the deployable architecture unless otherwise indicated. |
| Issues found in IBM-provided versions of Terraform modules | {{site.data.keyword.IBM_notm}} provides a way for customers to open issues. If the issue is with a deployable architecture, open a case. If the issue is with a module, open an issue in the module [GitHub repo](https://github.com/terraform-ibm-modules){: external}. | Customer provides information to reproduce any problem. |
| Issues with IBM container images | {{site.data.keyword.IBM_notm}} provides a way for customers to open issues. | Customer provides information to reproduce any problem. |
| Issues with third party and open source container images | N/A | Customer resolves with third-party vendor or open source community. |
| Issues with {{site.data.keyword.cloud_notm}}-provided stock operating system images | N/A | Customer must get a compatible stock image from the vendor. |
| Issues with the services that the Terraform creates from a deployable architecture | {{site.data.keyword.cloud_notm}} resolves issues with the services. | N/A |
| {{site.data.keyword.cloud_notm}} resource outages or issues that occur during automated template execution by using {{site.data.keyword.cloud_notm}} Terraform Provider | IBM reports outages for any cloud resources on the [Status](https://cloud.ibm.com/status){: external} page. | Customers can redeploy after the issue is resolved.|
| {{site.data.keyword.cloud_notm}} catalog and private catalog support | {{site.data.keyword.IBM_notm}} provides a way for you to discover available deployable architectures in our public catalog and save your versions to a private catalog. | N/A |
| Provide ability for drift detection | {{site.data.keyword.IBM_notm}} notifies you if your instantiated resources differ from the base pattern. | Customer decides when to remediate any configurations detected in drift detection. |
| Pulling deployable architecture changes into a project| {{site.data.keyword.IBM_notm}} provides the ability for customers to update the version of a deployable architecture in a project if a new version becomes available. | Customers are [notified when a new deployable architecture version is available](/docs/secure-enterprise?topic=secure-enterprise-needs-attention-projects#na-version-update) so they can update their project. \n\n Customers can save their existing project data through an API, CLI, or by [exporting the project.json](/docs/secure-enterprise?topic=secure-enterprise-json-project&interface=ui#json-export) from the console. The saved information can be used as a backup or as a rollover plan if an issue exists.  \n\n Customers can then test the deployable architecture changes by deploying in a development or test environment before they deploy to production. These actions can all be completed within the same project.  |
| Provide notice of end of support | {{site.data.keyword.IBM_notm}} provides notice through regular channels. | N/A |
{: row-headers}
{: caption="Responsibilities for change management" caption-side="bottom"}
{: summary="The first column describes the task that a customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}


## Identity and access management
{: #iam-responsibilities-da}

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Accessing deployed deployable architectures | {{site.data.keyword.IBM_notm}} provides the ability to control user access to resources provisioned through projects.  | Use Identity and Access Management (IAM) to [assign users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project). |
| Authorize a project to deploy a deployable architecture configuration| {{site.data.keyword.IBM_notm}} provides the ability to authorize a project to deploy a deployable architecture configuration.  | Choose an authentication method to authorize a project to deploy in an account. It’s recommended to use a trusted profile, but you can use an API key or an existing secret to [authorize a project to deploy](/docs/secure-enterprise?topic=secure-enterprise-authorize-project) in an account.  |
{: row-headers}
{: caption="Responsibilities for identity and access management" caption-side="bottom"}
{: summary="The first column describes the task that a customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Security and regulation compliance
{: #security-compliance-da}

Security and regulation compliance includes tasks such as security controls implementation and compliance certification.

|  Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Apply patches and security updates to operating system in customer instances | IBM notifies you of updates. | Customer must apply all updates. |
| Install software and OS patches into customer-managed virtual machines | N/A | Customer must apply all patches. |
| Meet security and compliance objectives| Provide a secure deployable architecture that complies with declared standards. For more information about data security, see [How do I know that my data is safe?](/docs/overview?topic=overview-security).  | Secure your workloads and data. Integrate tools into your toolchains that satisfy your security and compliance requirements. To learn more about securing your cloud apps, see [What is cloud native?](https://www.ibm.com/think/topics/cloud-native){: external}  |
{: row-headers}
{: caption="Responsibilities for security and regulation compliance" caption-side="bottom"}
{: summary="The first column describes the task that a customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}

## Disaster recovery
{: #disaster-recovery-da}

Disaster recovery includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
| Meet disaster recovery objectives| {{site.data.keyword.IBM_notm}} follows best practices for disaster recovery for the {{site.data.keyword.cloud_notm}} platform and services. For more information about disaster recovery, see the [{{site.data.keyword.IBM_notm}} Disaster Recovery Plan](/docs/resiliency?topic=resiliency-ha-redundancy#disaster-recovery). | Customers can help meet disaster recovery objectives when using deployable architectures by [understanding their responsibilities](/docs/resiliency?topic=resiliency-understanding-dr) and maintaining a disaster recovery plan for services and software that are deployed by using deployable architectures and customer-owned data stored within those services or software.   |
| Meet high availability objectives| {{site.data.keyword.cloud_notm}} is available globally and load balanced from a single URL. It is highly available and continues to be available even if your resources are unavailable. For more information about high availability, see the [{{site.data.keyword.IBM_notm}} service level objectives](/docs/resiliency?topic=resiliency-slo). | {{site.data.keyword.IBM_notm}} deployable architectures represent best practice deployment patterns. It is the customer's responsibility to [understand high availability considerations](/docs/resiliency?topic=resiliency-understanding-ha) when selecting and configuring deployable architectures. |
{: row-headers}
{: caption="Responsibilities for disaster recovery" caption-side="bottom"}
{: summary="The first column describes the task that a customer or IBM might be responsibility for. The second column describes {{site.data.keyword.IBM_notm}} responsibilities for that task. The third column describes your responsibilities as the customer for that task."}
