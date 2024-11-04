---

copyright:
  years: 2023
lastupdated: "2024-11-01"

keywords: question about projects, project, resource project, tainted resource, tainted project

subcollection: secure-enterprise

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why are my resources tainted?
{: #troubleshoot-project-resource}
{: troubleshoot}
{: support}

When you view your resource list, there are resources with a tainted status.
{: shortdesc}

A resource that you deployed is marked as tainted.
{: tsSymptoms}

Terraform marks a resource that isn't fully functional as tainted. This might be due to a provisioning error, or because Terraform is unable to determine if the resources were successfully or partially created.
{: tsCauses}

Terraform will delete and re-create the resource at the start of the next deployment. You can manually remove the taint status by deleting the resource and re-creating it.

For more information, see [Managing resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle&interface=ui#update-resources).
{: tsResolve}
