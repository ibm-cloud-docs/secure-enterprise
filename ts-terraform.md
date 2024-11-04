---

copyright:
  years: 2023
lastupdated: "2024-11-01"

keywords: question about schematics failures, failed validation, project failure, terraform version, terraform run time version

subcollection: secure-enterprise

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# How do I update my Terraform version?
{: #troubleshoot-terraform-version}
{: troubleshoot}
{: support}

If you try to validate a configuration, and the validation fails due to a plan failure, you might need to edit the settings in your {{site.data.keyword.bplong}} workspace.
{: shortdesc}

You tried to validate a configuration, but the validation failed due to a plan failure. In the {{site.data.keyword.bpshort}} logs, you see the following error:
{: tsSymptoms}

> Error: Unsupported Terraform Core version

{{site.data.keyword.bpshort}} is using a version of Terraform that is no longer supported.
{: tsCauses}

Update the settings of the {{site.data.keyword.bpshort}} workspace to use a supported Terraform version.
{: tsResolve}

Complete the following steps:

1. From the projects list, select a project.
1. Go to the **Configurations** tab, and select a deployable architecture configuration.
1. Select the **Workspace** URL to open the {{site.data.keyword.bpshort}} logs.
1. Click **Settings**.
1. In the Details section, select **Update** for the Offering version to update the Terraform version that your workspace is using.
