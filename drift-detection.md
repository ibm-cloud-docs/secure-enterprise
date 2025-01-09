---

copyright:
  years: 2024, 2025
lastupdated: "2025-01-09"

keywords: drift detection, deploying architecture, drift, deployable, project, scan, monitor, monitor drift

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Managing drift
{: #manage-drift}

Drift happens when the state of your deployed resources differs from the configurations that you defined in your project. You can set up your project to automatically detect drift. Then, if drift is detected, you can override the changes by redeploying the configuration. Or you can modify your configuration to adopt the changes.
{: shortdesc}

This is an experimental feature that is available for evaluation and testing purposes and might change without notice.
{: experimental}

## Setting up automatic drift detection
{: #automatic-detection}

You or members of your team can enable automatic drift detection by updating the settings of your project. With drift detection, you can run a daily check to compare your configurations to your deployed resources to detect any difference.

1. Navigate to the project dashboard, and open the **Manage** tab.
2. Click **Settings**.
3. Toggle **Detect drift** to **On**.

If your use case requires it, you can run a drift detection scan manually from the {{site.data.keyword.bplong}} workspace job page. You can trigger the scan manually without worrying about any impact to the schedule for automatic drift detection. For more information, check out [Detecting drift in workspaces](/docs/schematics?topic=schematics-drift-note).
{: note}

## Fixing drift
{: #fix-drift}

You can review all the historical entries of the daily drift detection scan in the **Activity** tab of your project. The **Needs attention** widget also displays the most current update if drift is detected or the scan fails to run. If no drift is detected, only the **Activity** log is updated. Complete the following steps to fix drift.

1. Review the **Needs attention** widget for a **Drift detected** entry.
2. Click the drift-related item to view the logs.
3. Review the changes that are identified.
    1. To override the changes to your deployed resources, click **Redeploy**.
    2. Update your configuration or the deployable architecture to adopt the changes.
