---

copyright:
  years: 2022, 2023
lastupdated: "2026-11-09"

keywords: best practice projects, manage projects, needs attention

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Viewing needs attention items to monitor project lifecycle events
{: #needs-attention-projects}

Needs attention items are events that specifically impact the project lifecycle. By viewing the needs attention items, you can monitor and address all project lifecycle events. They are located in the Needs attention widget, a centralized place to view and control these events. Events include changes that need to be reviewed, validation and deployment failures, configuration and compliance issues, and new available versions of deployable architectures.
{: shortdesc}

If you want to send and receive notifications about a project by using email, SMS, or other supported delivery channels, you can [enable event notifications for projects](/docs/secure-enterprise?topic=secure-enterprise-event-notifications-events&interface=ui).
{: tip}

Complete the following steps to view needs attention items:

1. Go to the [Projects](/projects) page, and select a project.
1. On the Overview tab, you can view the needs attention items.
1. Select the item that needs attention to view more information about it.

You can also check the needs attention column on the [Projects](/projects) page.
{: tip}

The following needs attention items can occur in a project.

## Needs attention failures
{: #na-create-fail}

You can address any failures that occur during your project's lifecycle by using the needs attention widget. For more information on fixing failures that need your attention, go to [How do I fix project failures?](/docs/secure-enterprise?topic=secure-enterprise-ts-na-failures)

## New deployable architecture version available
{: #na-version-update}

A new deployable architecture version is available. Edit the configuration to update it to the new version.

If the new version is a major version update, for example, going from version `1.0.0` to `2.0.0`, review the readme file for the deployable architecture to learn about any special handling that might be required before you deploy the new version.
{: important}

It's a best practice to deploy new versions in your testing environment first to check for issues. Then, deploy to your production environment. You might also want to deploy the new version to a single region first to check for issues before you deploy to all regions.

To update your configuration, complete the following steps:

1. From the projects list, select a project.
1. Go to the **Configurations** tab, and select a deployable architecture configuration.
1. Click **Edit**.
1. In the Select inputs section, use the **Version** menu to select the new version of the deployable architecture.
1. Click **Save**.
1. Validate and approve your changes before you deploy.

## Changes not deployed
{: #na-config-added}

After your save and validate your changes, you are notified that the changes are not deployed. Your changes must be approved before you deploy your configuration. You can also go to the **Activity tab** for more details on this update.

## Validation successful
{: #na-check-success}

The validation of your changes is complete. Issues didn't occur during the validation process, and you can deploy your changes.
