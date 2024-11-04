---

copyright:
  years: 2023, 2024
lastupdated: "2024-11-01"

keywords: question about project resources, resources

subcollection: secure-enterprise

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't I view my project resources?
{: #troubleshoot-resource-view}
{: troubleshoot}
{: support}

Some or all resources aren't listed on the Resources tab in a configuration.
{: shortdesc}

You open a configuration and go to **Resources**. Some or all of the resources you expected to see aren't included in the list of resources.
{: tsSymptoms}

If your configuration's deployment was successful, but some resources are missing from the Resources tab, they might have been destroyed individually from the {{site.data.keyword.bplong}} workspace. If your configuration failed to deploy, some resources might have been created, but not all of the resources that are needed for a full deployment.
{: tsCauses}

Undeploy the configuration and try deploying your configuration again.
{: tsResolve}
