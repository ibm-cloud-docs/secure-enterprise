---

copyright:
  years: 2023, 2024
lastupdated: "2024-10-09"

keywords: enterprise, enterprise account, multiple accounts, assign access, enterprise access, templates, enterprise managed, access, enterprise settings

subcollection: secure-enterprise

content-type: tutorial
completion-time: 15m
---

{{site.data.keyword.attribute-definition-list}}

# Customizing IAM settings for an enterprise
{: #enterprise-iam-settings-tutorial}
{: toc-content-type="tutorial"}
{: toc-completion-time="15m"}

Customize the IAM settings for all of the accounts in your enterprise to meet compliance and internal standards. You can centrally manage IAM settings like API key creation, authentication, and active sessions for new and existing accounts with enterprise-managed settings templates.
{: shortdesc}

The tutorial uses a fictitious company that's called *Example Corp*, which [set up an enterprise](/docs/secure-enterprise?topic=secure-enterprise-enterprise-tutorial) with the following structure. As you complete the tutorial, adapt each step to match your organization's security setting requirements.

![A four-tier enterprise that groups accounts according to department in an organization. For example, account groups are named Marketing, Development, and Sales. The account groups contain accounts for teams within those departments. For example, the Sales account group contains accounts for Direct, Online, and Enablement.](images/enterprise-by-dept.svg "An enterprise that organizes accounts according to department in the organization."){: caption="An enterprise that is organized by department" caption-side="bottom"}

## Before you begin
{: #before-settings-iam-tutorial}

Read [How enterprise-managed IAM access works](/docs/secure-enterprise?topic=secure-enterprise-access-enterprises#how-enterprise-iam) and [Creating enterprise-managed settings templates](/docs/secure-enterprise?topic=secure-enterprise-settings-template-create) to learn the basics of enterprise-managed IAM.

Verify that you have the required access in an Enterprise account, which is the root account from which you assign IAM templates to child accounts. To create IAM templates, you must have the Template Administrator role on All IAM Account Management services. To assign IAM templates to child accounts, you must have the Template Assignment Administrator role on All IAM Account Management services and the Viewer role or higher on the Enterprise service.

New and existing accounts in your enterprise must turn on the Enterprise-managed IAM setting to be eligible for IAM template assignments. For more information, see [Opting in to enterprise-managed IAM](/docs/secure-enterprise?topic=secure-enterprise-enterprise-managed-opt-in).
{: important}

## Create the settings template
{: #create_template_tutorial}
{: step}

Settings templates are created in the Enterprise account. The versions of only one setting template can be assigned in an enterprise. Create two versions of the same template to assign to account groups that have different account security requirements.

1. In the {{site.data.keyword.cloud}} console, go to **Manage > Access (IAM) > Templates**.
1. Click **IAM settings > Create**.
1. Enter the name of the settings template, such as AccountSecuritySettings, to identify the settings template in your enterprise account.
1. Enter a description for this version of the template. For example, let's say `v1` is for the Development department account group. Your description might include the department name, level of multifactor authentication that is required, and other settings that you define in the template.

   The template name is consistent across versions, while the description can be unique to differentiate between versions and their purpose.
   {: note}

1. Click **Create**.
1. Click **Account**.
   1. Allow developers to create API keys by leaving the setting **API key creation** cleared.
   1. Restrict developers from creating service IDs by enabling the setting **Service ID creation**.
   1. Restrict the IP addresses that developers can use to access their accounts by entering the IP addresses that the Development department uses. This way, attempts to access the Development accounts outside of this list are blocked.
1. Click **Authentication**.
   1. Select **MFA for a user with or without an IBMid > U2F MFA**. Use the highest level of MFA for developers because they have high levels of access to critical resources.
1. Click **Login session**.
   1. Configure **Active sessions** to 12 hours to make sure the users are logging in at least once during their work day.
   1. Configure the **Sign out due to inactivity** setting to 30 minutes to make sure that idle sessions require reauthentication within this time limit.

   Your configuration is saved automatically.
   {: note}

Any settings that aren't defined in the template can be managed by the child account. Before you can create the second version, review and commit `v1` by completing the following steps.

1. Click **Review**.
1. Verify that the settings template is configured to your expectation.
1. Click the checkbox to confirm that you can't edit the version.
1. Click **Commit**.

## Create the second version
{: #create_v2_tutorial}
{: step}

Create another version of the settings template for the Marketing and Sales department groups. These departments don't have as much access to resources as developers, so they don't need the same strict settings as the Development department groups.

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM) > Templates**.
1. Click **IAM settings** and select the AccountSecuritySettings template.
1. Click the **New version** icon ![New version icon](../icons/new-version.svg "New version").
1. Enter the Marketing and Sales department in the description, the level of multifactor authentication that is required, and other settings that you define in the template.
1. Click **Create**.
1. Click **Account**.
   1. Don't allow the Marketing and Sales department users to create API keys by enabling the setting that restricts **API key creation**.
   1. Don't allow the Marketing and Sales department users to create service IDs by enabling the setting that restricts **Service ID creation**.
   1. Restrict the IP addresses that Marketing and Sales department users can use to access their accounts by entering the IP addresses that they use. This way attempts to access the Marketing and Sales accounts outside of this list are blocked.
1. Click **Authentication**.
   1. Select **MFA for a user with or without an IBMid > TOTP MFA**. Use the middle level of MFA for marketing and sales to protect the resources in those accounts.
1. Click **Review**.
1. Verify that the settings template is configured to your expectation.
1. Click the checkbox to confirm that you can't edit the version.
1. Click **Commit**.
1. Click the **Templates** breadcrumb to go back to the list of IAM settings templates.

Now, you're ready to assign both `v1` and `v2` to their corresponding account groups.

## Assign versions to account groups
{: #assign_settings_tutorial}
{: step}

Assign `v1` to the Development account group and `v2` to the Marketing and Sales account groups. When you assign a template to an account group, accounts that you move into the account group or new accounts that you create in the group inherit the settings template automatically.

### Assign `v1` to the Development group
{: #assign_settings_v1}
{: step}

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM) > Templates**.
1. Click **IAM settings**.
1. Go to the AccountSecuritySettings template and click the **Table expand** icon ![Table expand icon](../icons/table-expand.svg "Table expand").
1. Click `v1` of the AccountSecuritySettings template.
1. Click **Assign accounts**.
1. Select the **Development** account group.
1. Click **Assign accounts**.

After the IAM settings template is assigned, you are directed to the Assignment reports. From here, you can view the assignment details and manage where `v1` of the template is assigned.

### Assign `v2` to the Marketing and Sales group
{: #assign_settings_v2}
{: step}

1. Click the **Versions icon** ![Versions icon](../icons/version.svg "Versions") and select `v2` of the AccountSecuritySettings template.
1. Click **Assign accounts**.
1. Select the **Marketing** and **Sales** account groups.
1. Click **Assign accounts**.

After the IAM settings template is assigned, you are directed to the Assignment reports. From here, you can view the assignment details and manage where `v2` of the template is assigned.

## Next steps
{: #settings-template-next}

Now that you learned how to set up accounts in your enterprise that are customized consistently, you can continue to add accounts and account groups as your teams and cloud workloads grow. For more information, see [Creating enterprise-managed settings templates](/docs/secure-enterprise?topic=secure-enterprise-settings-template-create).
