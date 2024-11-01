---

copyright:
  years: 2023, 2024
lastupdated: "2024-11-01"

keywords: troubleshoot projects, validate config, unable to validate your configuration, needs attention failure, validation failure, needs attention, failure, validation, troubleshoot

subcollection: secure-enterprise

content-type: troubleshoot
---

{{site.data.keyword.attribute-definition-list}}

# Why can't I validate my configuration?
{: #ts-cant-validate}
{: troubleshoot}

You tried to validate changes to a configuration in a project, but an error prevented the validation from completing.
{: shortdesc}

You tried to validate changes to a configuration in a project, but the following error displays:
{: tsSymptoms}

> Unable to validate your configuration.

An availability issue occurred, which prevented validation.
{: tsCauses}

* A problem occurred with the project's authentication method. The API key that is used in a secret might have been deleted, or the trusted profile was not set up properly. For more information, go to [Defining an authentication menthod](/docs/secure-enterprise?topic=secure-enterprise-best-practices-projects&interface=ui#best-practice-auth).
{: tsResolve}

* If the error message provides information about a {{site.data.keyword.bpshort}} cart order failure with a status code `403`, then access is not granted between the {{site.data.keyword.cloud_notm}} Projects service and other {{site.data.keyword.cloud_notm}} services. For more information, go to [Granting access between the {{site.data.keyword.cloud_notm}} Projects service and other {{site.data.keyword.cloud_notm}} services](/docs/secure-enterprise?topic=secure-enterprise-access-project&interface=ui#user-create-role).

* A service might have been down during the validation. Check the [Status page](/status){: external} for the service, and try to validate your configuration again when the service is back up.

* A timeout might have occurred during the validation, which caused a failure. Try validating your configuration again.

* If the error message provides information about a `bad request` or `parameters`, you might need to edit your input values, then validate your configuration again.
