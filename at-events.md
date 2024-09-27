---

copyright:
  years: 2023, 2024
lastupdated: "2024-09-27"

keywords: projects, audit, events, activity tracker

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Activity tracking events for a project
{: #at_events}

As a security officer, auditor, or manager, you can use the {{site.data.keyword.at_full}} service to track how users and applications interact with the {{site.data.keyword.cloud_notm}} Projects service.
{: shortdesc}

Activity tracking events report on activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use the events to investigate abnormal activity and critical actions and to comply with regulatory audit requirements.

You can use {{site.data.keyword.atracker_full_notm}}, a platform service, to route auditing events in your account to destinations of your choice by configuring targets and routes that define where activity tracking events are sent. For more information, see [About {{site.data.keyword.atracker_full_notm}}](/docs/atracker?topic=atracker-about).

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.

As of 28 March 2024, the {{site.data.keyword.at_full_notm}} service is deprecated and will no longer be supported as of 30 March 2025. Customers will need to migrate to {{site.data.keyword.logs_full_notm}} before 30 March 2025. During the migration period, customers can use {{site.data.keyword.at_full_notm}} along with {{site.data.keyword.logs_full_notm}}. Activity tracking events are the same for both services. For information about migrating from {{site.data.keyword.at_full_notm}} to {{site.data.keyword.logs_full_notm}} and running the services in parallel, see [migration planning](/docs/cloud-logs?topic=cloud-logs-migration-intro).
{: important}

## Locations where activity tracking events are generated
{: #at-locations}

{{site.data.keyword.cloud_notm}} Projects sends {{site.data.keyword.at_full_notm}} events in the following regions: Sydney, Frankfurt, and Washington. 

## Locations where activity tracking events are sent to {{site.data.keyword.at_full_notm}} hosted event search
{: #at-legacy-locations}



{{site.data.keyword.cloud_notm}} Projects sends activity tracking events to {{site.data.keyword.at_full_notm}} hosted event search in the followng regions: Sydney, Frankfurt, and Washington. 



## Viewing activity tracking events for {{site.data.keyword.cloud_notm}} Projects
{: #at-viewing}

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.

### Launching {{site.data.keyword.logs_full_notm}} from the Observability page
{: #log-launch-standalone}

For information on launching the {{site.data.keyword.logs_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.logs_full_notm}} documentation.](/docs/cloud-logs?topic=cloud-logs-instance-launch)

## List of management events
{: #at_actions}



{{site.data.keyword.cloud_notm}} Projects supports the management events that are indicated in the following table. 

| Action             | Description      |
|--------------------|------------------|
| `project.project.create` | Create a project.     |
| `project.project.read` | Read a project.     |
| `project.project.list` | List all projects under the account.     |
| `project.project.update` | Update a project.     |
| `project.project.delete` | Delete a project.     |
| `project.config.create` | Create a project config.     |
| `project.config.read` | Read a project config.     |
| `project.config.update` | Update a project config.     |
| `project.config.validate` | Validate a project config.     |
| `project.config.list` | List all project configs under the account.     |
| `project.config.update` | Update a project config.     |
| `project.config.approve` | Approve a project config draft.     |
| `project.config.force-approve` | Force approve a project config draft.     |
| `project.config.delete` | Delete a project config.     |
| `project.config.deploy` | Deploy a project config.     |
| `project.config.undeploy` | Undeploy (destroy) a project config.     |
| `project.config.manual-tag` | Add a tag to a config.     |
| `project.config.export-stack-definition` | [Experimental]{: tag-purple} Add a deployable architecture stack to a private catalog.     |
| `project.environment.create` | Create a project environment.     |
| `project.environment.read` | Read a project environment.     |
| `project.environment.list` | List all project environments under the account.     |
| `project.environment.update` | Update a project environment.     |
| `project.environment.delete` | Delete a project environment.     |
{: caption="Table 1. Actions that generate management events" caption-side="bottom"}

For a complete list of custom request and response parameters for each event, see the [Project API](https://{DomainName}/apidocs/projects). The update actions don't provide information about the delta, only the new value is provided.
{: note}

## Viewing events
{: #at_ui}

Events that are generated by an instance of the {{site.data.keyword.cloud_notm}} Projects service are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, see [Launching the UI](/docs/activity-tracker?topic=activity-tracker-launch).
