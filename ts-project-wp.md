---

copyright:
  years: 2025
lastupdated: "2025-12-15"

keywords: scc, workload protection, compliance, zone, scan

subcollection: secure-enterprise

content-type: troubleshoot

ai-gen-assist: wca

---

{{site.data.keyword.attribute-definition-list}}

# Why is my {{site.data.keyword.sysdigsecure_short}} zone missing?
{: #troubleshoot-project-wp}
{: troubleshoot}
{: support}

When you configure an architecture for deployment, you're unable to use a policy from your instance of {{site.data.keyword.sysdigsecure_short}} to scan your configuration for compliance. 
{: shortdesc}

When you edit a configuration in a project and select your instance of {{site.data.keyword.sysdigsecure_short}}, a zone that you expect to select for compliance scanning is not available.
{: tsSymptoms}

The zone might no longer exist, or your {{site.data.keyword.sysdigsecure_short}} instance is not enabled for Cloud Security Posture Management (CSPM). 
{: tsCauses}

Make sure that the zone exists, and that your account is enabled for CSPM. For more information, go to [Implementing CSPM for {{site.data.keyword.cloud_notm}}](/docs/workload-protection?topic=workload-protection-cspm-implement&interface=ui).
{: tsResolve}
