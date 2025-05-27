---

copyright:
  years: 2023, 2025
lastupdated: "2025-05-27"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Learning about project architecture and workload isolation
{: #compute-isolation}

Review the following sample architecture for projects and learn more about different isolation levels so you can choose the solution that best meets the requirements of the workloads that you want to run in the cloud.
{: shortdesc}

## Project architecture
{: #project-architecture}

Projects are an {{site.data.keyword.Bluemix}} platform feature that's supported by a highly available multi-region infrastructure. The project UI and API services are managed microservices, which are deployed to a minimum of three multi-zone regions around the world. A single zone failure results in service instances in other zones that are in the same region taking over. A regional failure results in another region taking over. Project microservices use {{site.data.keyword.IBM_notm}} managed instances of {{site.data.keyword.contdelivery_short}} Toolchains in at least three regions to run multi-step validation, deployments, and destroy resource jobs.

Projects create instances of {{site.data.keyword.bplong_notm}} workspaces in your account and uses them to perform plan, deploy, and destory resource jobs. This ensures that Terraform state storage and any access to private resources is completely under customer control. The {{site.data.keyword.bpshort}} workspace UI, API, and CLI can be used, if needed, to provide additional control and visibility over how Infrastructure as Code (IaC) is managed by projects.

Deleting a {{site.data.keyword.bpshort}} workspace that's used by a project can cause the loss of the Terraform state for that configuration, which can lead to unexpected behavior for project users, for example, causing the project to create duplicate resources on the next deployment. Project-created {{site.data.keyword.bpshort}} workspaces are tagged with the related projects and configurations.
{: note}

## Project data storage
{: #data-storage}

Projects are a multi-tenant service that uses a regional data storage model where the data is stored in the region that is selected for the project. Specifically, project configuration data (the deployable architecture reference and selected inputs) is stored in a multi-tenant {{site.data.keyword.IBM_notm}} managed {{site.data.keyword.cloudant_short_notm}} database in the project region. The {{site.data.keyword.bpshort}} instances that are created in the customer account for a project are also located in the project region. This ensures that all configuration data is stored in the customer selected region. However, basic project metadata, like the name, description, and state is replicated globally to enable global search and the ability to list all projects across regions.

## Project data protection
{: #project-protect-data}

Project's are a multi-tenant platform service where all project data is encrypted at rest and in flight. Project primary storage uses separate documents for each project in {{site.data.keyword.cloudantfull}}. {{site.data.keyword.cloudant_short_notm}} provides robust data protection and security. For example, {{site.data.keyword.cloudant_short_notm}} encrypts all data at rest and automatically stores three copies of every document to different nodes in a cluster. In addition, project data is periodically backed up to {{site.data.keyword.cos_full}}, which is also encrypted. Access to project data is protected by IAM authentication and authorization policies that ensure client isolation. All project data is encrypted in flight by using HTTPS with TLS1.2+.

For more information on {{site.data.keyword.cloudant_short_notm}} data protection, see [Securing your data in {{site.data.keyword.cloudant_short_notm}}](/docs/Cloudant?topic=Cloudant-securing-your-data-in-cloudant). For more information on {{site.data.keyword.cos_short}} data protection, see [Data security](/docs/cloud-object-storage?topic=cloud-object-storage-security).
