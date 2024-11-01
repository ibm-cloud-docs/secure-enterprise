---

copyright:
  years: 2023
lastupdated: "2024-11-01"

keywords: troubleshoot projects, deploy config, changes failed to deploy, needs attention failure, deployment failure, needs attention, failure, deploy

subcollection: secure-enterprise

content-type: troubleshoot
---

{{site.data.keyword.attribute-definition-list}}

# How do I address a failed deployment when using projects?
{: #ts-deploy-failed}
{: troubleshoot}

You tried to deploy a configuration from a project, but the deployment failed, preventing a successful deployment to your target environment.
{: shortdesc}

The project displays a **Needs attention** alert and a message displays on the **Activity** tab that states that the deployment failed.
{: tsSymptoms}

The changes that you made to your resources failed to deploy. Go to the needs attention widget on the **Overview** tab and select **Changes failed to deploy** to view more information about the failure.
{: tsCauses}

* Lack of permissions: Ensure that the project has sufficient privileges in the target account.
* Invalid input value: Check the logs for details.
* Duplicate input values: Some input values, like an {{site.data.keyword.cos_short}} bucket name or IP address, must be unique. Undeploy the existing resource, or modify your input values to be unique.
* Timeout: If the deployment runs too long, it might time out and cause a failure. Try deploying the configuration again.
* A service was down during the deployment: Check the {{site.data.keyword.bpshort}} logs for `400` or `500` errors that indicate a problem with the service. Check the [Status page](/status){: external} for the service, and try to deploy again when the service is back up. For more information on the `500` errors in {{site.data.keyword.bpshort}}, see [Why am I getting 5xx errors in {{site.data.keyword.bpshort}}?](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-server-errors)
* Reaching a quota limit: Ensure that sufficient quota space is available in the account. For example, you can have only one instance of a service that uses a Lite plan.
* Manual changes were made to the account that conflict with the deployable architecture. We recommend avoiding manual changes to the account during a deployment.
{: tsResolve}
