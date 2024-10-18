---

copyright:
  years: 2024
lastupdated: "2024-10-18"

keywords: troubleshooting projects, manage projects

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting for projects
{: #troubleshooting-for-projects}

Troubleshooting for {{site.data.keyword.cloud}} projects might include questions about configurations, access to projects, validation, and deployment.
{: shortdesc}

## Why can't I access a project?
{: #troubleshoot-project-access}
{: troubleshoot}
{: support}

You try to find a project that you created or a project that you thought you had access to from the Projects page in the console, but it isn't listed.
{: shortdesc}

A project that you expected to find in the console from the **Menu** ![Menu icon](../icons/icon_hamburger.svg "Menu") > **Projects** page isn't included in your list of projects.
{: tsSymptoms}

Your project might have been deleted by another member of your team, or you don't have the required level of access to view projects.
{: tsCauses}

Make sure that you have the correct access to view projects. For more information, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project). If your project was deleted, create a new one. If you have the {{site.data.keyword.at_full}} service set up for your account, you can use that to [audit events for a project](/docs/secure-enterprise?topic=secure-enterprise-at_events), including project deletions.
{: tsResolve}

## Why can't I create a project?
{: #troubleshoot-project-create}
{: troubleshoot}
{: support}

You try to create a project from the Projects page in the console, but you encounter an error.
{: shortdesc}

When you click **Create** from the Projects page in the console, you encounter this error:
{: tsSymptoms}

>Additional access required

You don't have the required level of access to create a project.
{: tsCauses}

Assign yourself access, or request access from your account administrator. For more information, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).
{: tsResolve}

## Why can't I delete my project?
{: #ts-delete-project}
{: troubleshoot}

You tried to delete a project, but it doesn't delete successfully.
{: shortdesc}

The project displays a **Needs attention** alert and a message displays on the **Activity** tab that states that the project deletion failed.
{: tsSymptoms}

One of the reasons a project might not delete successfully is due to an issue with the project tooling services.
{: tsCauses}

Project tooling is required to delete a project. If an error occurs with the project tooling service, you can’t delete the project. Try again later or contact {{site.data.keyword.cloud_notm}} Support.
{: tsResolve}

## Why can't I deploy my changes to a configuration?
{: #troubleshoot-deploy-changes}
{: troubleshoot}
{: support}

If you're unable to deploy changes to a configuration, you might need an Editor to approve your changes.
{: shortdesc}

You can edit and save changes to a configuration, but you can't deploy them.
{: tsSymptoms}

After you save changes to a configuration, they must be validated. Then, a user who is assigned to the Editor role for projects must approve the changes before they can be deployed.
{: tsCauses}

Contact an Editor on your team to approve the changes to the configuration. Contact your account administrator if you need Editor access to the {{site.data.keyword.cloud_notm}} Projects service. For more information, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).
{: tsResolve}

## Why can't I validate my configuration?
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

## How do I address a failed validation when using projects?
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

## How do I address a failed deployment when using projects?
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
* Duplicate input values: Some input values, like an {{site.data.keyword.cos_short}} bucket name or IP address, must be unique. Destroy the existing resource, or modify your input values to be unique.
* Timeout: If the deployment runs too long, it might time out and cause a failure. Try deploying the configuration again.
* A service was down during the deployment: Check the {{site.data.keyword.bpshort}} logs for `400` or `500` errors that indicate a problem with the service. Check the [Status page](/status){: external} for the service, and try to deploy again when the service is back up. For more information on the `500` errors in {{site.data.keyword.bpshort}}, see [Why am I getting 5xx errors in {{site.data.keyword.bpshort}}?](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-server-errors)
* Reaching a quota limit: Ensure that sufficient quota space is available in the account. For example, you can have only one instance of a service that uses a Lite plan.
* Manual changes were made to the account that conflict with the deployable architecture. We recommend avoiding manual changes to the account during a deployment.
{: tsResolve}

## Why were my resources not destroyed?
{: #ts-destroy-failed}
{: troubleshoot}

You tried to destroy resources, but it failed.
{: shortdesc}

The project displays a **Needs attention** alert and a message displays on the **Activity** tab that states that the resources were not destroyed.
{: tsSymptoms}

Go to the needs attention widget on the **Overview** tab and select **Destroy resources failed** to view the {{site.data.keyword.bpshort}} logs, which provide more information about the failure.
{: tsCauses}

* Permissions for the API Key or trusted profile were changed: For more information about the required access for trusted profiles, see [Using trusted profiles to authorize projects](/docs/secure-enterprise?topic=secure-enterprise-tp-project).
* A service was down when you tried to destroy resources: Check the [Status page](/status){: external} for the service, and try to destroy resources again when the service is back up.
* The deployable architecture that your configuration is based on contains bugs that caused a failure when you tried to destroy resources.
* Though not recommended, some resources might need to be destroyed one by one in the {{site.data.keyword.bpshort}} resource list. For more information, see [Destroying resources created by projects](/docs/secure-enterprise?topic=secure-enterprise-best-practices-projects#project-resources-destroy).
{: tsResolve}

## How do I update my Terraform version?
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

## Why can't I view my project resources?
{: #troubleshoot-resource-view}
{: troubleshoot}
{: support}

Some or all resources aren't listed on the Resources tab in a configuration.
{: shortdesc}

You open a configuration and go to **Resources**. Some or all of the resources you expected to see aren't included in the list of resources.
{: tsSymptoms}

If your configuration's deployment was successful, but some resources are missing from the Resources tab, they might have been destroyed individually from the {{site.data.keyword.bplong}} workspace. If your configuration failed to deploy, some resources might have been created, but not all of the resources that are needed for a full deployment.
{: tsCauses}

Destroy the existing resources and try deploying your configuration again.
{: tsResolve}

## Why are my resources tainted?
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
