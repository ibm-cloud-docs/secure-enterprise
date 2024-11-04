---

copyright:
  years: 2023
lastupdated: "2024-11-01"

keywords: troubleshoot projects, needs attention failure, resource failure, needs attention, failure, destroy, resources, undeploy configurations, undeploy fail, undeploy configurations failed

subcollection: secure-enterprise

content-type: troubleshoot
---

{{site.data.keyword.attribute-definition-list}}

# Why were my resources not destroyed when I undeployed a configuration?
{: #ts-destroy-failed}
{: troubleshoot}

You tried to undeploy a configuration, but it failed.
{: shortdesc}

The project displays a **Needs attention** alert and a message displays on the **Activity** tab that states that the configuration failed to undeploy.
{: tsSymptoms}

Go to the needs attention widget on the **Overview** tab and select **Failed to undeploy** to view the {{site.data.keyword.bpshort}} logs, which provide more information about the failure.
{: tsCauses}

* Permissions for the API Key or trusted profile were changed: For more information about the required access for trusted profiles, see [Using trusted profiles to authorize projects](/docs/secure-enterprise?topic=secure-enterprise-tp-project).
* A service was down when you tried to undeploy a configuration: Check the [Status page](/status){: external} for the service, and try to undeploy the configuration again when the service is back up.
* The deployable architecture that your configuration is based on contains bugs that caused a failure when you tried to undeploy it.
* Though not recommended, some resources might need to be destroyed one by one in the {{site.data.keyword.bpshort}} resource list. For more information, see [Undeploying resources created by projects](/docs/secure-enterprise?topic=secure-enterprise-best-practices-projects#project-resources-destroy).
{: tsResolve}
