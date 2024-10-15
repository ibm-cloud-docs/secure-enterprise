---

copyright:

  years: 2023, 2024

lastupdated: "2024-10-09"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Evaluating resource configuration with {{site.data.keyword.compliance_full}}
{: #security-compliance-scanning}

For highly regulated industries, such as financial services, achieving continuous compliance within a cloud environment is an important first step toward protecting customer and application data. Historically, that process was difficult and manual, which placed your organization at risk. But, with {{site.data.keyword.compliance_full}}, you can integrate daily, automatic compliance checks into your development lifecycle to help minimize that risk.
{: shortdesc}

A new and improved experience of {{site.data.keyword.compliance_short}} is here! Be sure that you're working with the latest architecture to avoid migration issues later. To learn more, see [How it works](/docs/security-compliance?topic=security-compliance-posture-management).
{: tip}


## Before you begin
{: #scc-before-begin}

Before you get started, be sure that you have the following prerequistes:

- Resources in your account to evaluate.
- An [{{site.data.keyword.cos_full_notm}} bucket](/docs/security-compliance?topic=security-compliance-storage) for results storage.
- The [proper access](/docs/security-compliance?topic=security-compliance-assign-roles) to perform scans.

Scanning your resources does not ensure regulatory compliance. An evaluation provides a point in time statement of your current posture for a specific resource. It is your responsibility to review and interpret the results to ensure that your organization is adhering to the controls that are required for your industry.
{: important}


## Configuring results storage
{: #scc-storage}

Before you can start evaluating your resources for compliance, you must configure an {{site.data.keyword.cos_short}} bucket where {{site.data.keyword.compliance_short}} can forward your results data for long-term storage. For more information about bucket requirements, see [Storing and processing data in {{site.data.keyword.compliance_short}}](/docs/security-compliance?topic=security-compliance-storage).

To connect your {{site.data.keyword.cos_short}} bucket, you can use the {{site.data.keyword.compliance_short}} UI.

1. Go to the {{site.data.keyword.compliance_short}} by clicking the Menu icon in the {{site.data.keyword.cloud_notm}} console and selecting Security and Compliance.
2. In the navigation, click **Settings**.
3. On the **Storage** tile, click **Connect**.
4. Ensure that the service-to-service policy between {{site.data.keyword.cos_short}} and {{site.data.keyword.compliance_short}} is configured. If a policy is already in place, this screen is not shown and you can skip to the next step.
5. Select an instance of {{site.data.keyword.cos_short}}.
6. From the table, select the bucket that you want to use.
7. Click **Connect**.


## Creating an attachment
{: #scc-attachment}

An attachment is how you target a specific grouping of your resources to evaluate against a specific profile.

1. In the {{site.data.keyword.compliance_short}} navigation, click **Dashboard** Then, click **Get started**.
2. Select the **Profile** that you want to use to evaluate compliance.

   If you don't see a profile that meets your specific needs, you can always create [a custom profile](/docs/security-compliance?topic=security-compliance-build-custom-profiles).

3. Target your attachment by selecting a **Scope** and identifying any resources that you want to **Exclude**. Then, click **Next**.
4. Optional: Customize the evaluations in your scan by editing the default parameters to match your specific use case.
5. Click **Next**.
6. Select the frequency at which you want to evaluate your attachment.
7. Optional: Configure notifications.
   1. If you want to receive notifications, toggle **Notify me** to **On**.
   2. By default, when notifications are enabled, you are alerted when 15% or more of your controls fail in a single scan. You can change this by adjusting the **Threshold** percentage.

      For example, if you have a profile with 100 controls and you want to be notified if 5 of them fail, you would select 5% as your threshold.

   3. Optional: Select specific controls that you want to be notified about.
      If there are high priority controls that pertain specifically to your job role, you might want to be notified every time that they fail. You can identify up to 15 controls per scan that you can receive individual notifications for. These notifications are sent regardless of whether the threshold identified in the previous step has been met.
      1. Click **Select control**.
      2. Select the controls that you want to be notified about by checking the box next to the control.
      3. Click **Ok**.
8. Review your choices and click **Create**.

When you create your attachment, a scan is scheduled and results are available within 24 hours. When the scan completes your results are available on the **Dashboard** in the {{site.data.keyword.compliance_short}} UI.




## Interpreting results
{: #scc-results}

When your scan completes, the results are available on the **Dashboard** tab in the {{site.data.keyword.compliance_short}} UI. The results are provided in both a graphical and detailed format.

When you visit the dashboard, you can see three graphical representations of data that has been aggregated from your scans.

![A visual representation of the service dashboard. The concepts are fully explained in the surrounding text.](images/dashboard.svg){: caption="Example dashboard" caption-side="bottom"}


The three graphs that you can see are:

Success rate
:   The rate at which your configurations pass the evaluation that is conducted. **Note:** The number of evaluations conducted does not always match the number of billable evaluations, as there is no charge for assessments evaluated as unable to perform. Be sure to look for the billable evaluations in each scan result if you need to estimate your cost.

Total controls
:   The total number of controls that have been evaluated in the past 30 days.

Total evaluations
:   The total number of evaluations that have been run in the past 30 days. An evaluation is the check of one resource against one assessment.

From the dashboard, you can drill into your results to see more detailed information about each evaluation that was conducted.


## Next steps
{: #scc-next}

While you wait for your scan to complete, learn more about adding {{site.data.keyword.compliance_short}} into your pipelines through DevSecOps.
