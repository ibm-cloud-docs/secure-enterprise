---

copyright:
  years: 2023
lastupdated: "2024-11-01"

keywords: troubleshoot projects, validate config, changes failed validation, needs attention failure, validation failure, needs attention, failure, validation

subcollection: secure-enterprise

content-type: troubleshoot
---

{{site.data.keyword.attribute-definition-list}}

# How do I address a failed validation when using projects?
{: #ts-na-failures}
{: troubleshoot}

You tried to validate changes to a configuration in a project, but the validation failed.
{: shortdesc}

The project displays a **Needs attention** alert and a message displays on the **Activity** tab that states that the validation failed.
{: tsSymptoms}

An issue occurred with the configuration during the validation process. You can't deploy your changes until issues are resolved. Go to the needs attention widget on the **Overview** tab and select **Changes failed validation** to view more information about the failure.
{: tsCauses}

* If the plan failed, it likely means that a required input value, such as an authentication method, hasn't been supplied yet. Or, one of the provided inputs isn't valid. The plan can also fail if the deployable architecture that your configuration is based on is invalid. Determine the exact problem from the logs, and update the input values if needed. If the logs indicate an error with the Terraform version, your settings in {{site.data.keyword.bplong}} might need to be updated. For more information, see [How do I update my Terraform version?](/docs/secure-enterprise?topic=secure-enterprise-troubleshoot-terraform-version&interface=ui)
* If the compliance check failed, the deployment is not compliant with the compliance profile. The failure might be caused by input values or because of compliance issues with the current version of the deployable architecture that you're configuring. Review which controls failed and adjust the inputs or update the deployable architecture until the validation is successful. If it's necessary, [an administrator can override and approve changes](/docs/secure-enterprise?topic=secure-enterprise-approve-failed-validation) that failed due to the Code Risk Analyzer scan.
{: tsResolve}
