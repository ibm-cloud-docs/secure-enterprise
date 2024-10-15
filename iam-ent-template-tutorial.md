---

copyright:
  years: 2023
lastupdated: "2024-10-09"

keywords: enterprise, enterprise account, multiple accounts, assign access, enterprise access, templates, enterprise managed, access, enterprise access group

subcollection: secure-enterprise

content-type: tutorial
account-plan: paid
completion-time: 15m

---

{{site.data.keyword.attribute-definition-list}}

# Centrally managing access for compliance focals in an enterprise
{: #enterprise-iam-ag-tutorial}
{: toc-content-type="tutorial"}
{: toc-completion-time="15m"}

This tutorial walks you through setting up an access group template that grants compliance focals access to child accounts in your enterprise. This way, you can centrally manage access in your organization and your compliance team can complete daily security and compliance tasks with reliable access in each account.
{: shortdesc}

Compliance focals play a crucial role in helping your enterprise adhere to regulations and standards while you take advantage of the benefits of cloud technology. Their responsibilities are to minimize risks, verify data security, and foster a culture of compliance within your organization. Compliance professionals know that centralized access management is fundamental to an effective security and compliance strategy. They can lead by example by having access that is managed from the root enterprise account.

Managing access centrally streamlines administrative tasks. It's more efficient to add, modify, or revoke user access in one centralized system rather than managing it individually across various accounts, applications, and services.

Compliance focals need read access to resources in many accounts in your enterprise so that they can assess risk, detect and respond to incidents, and verify that sensitive data is encrypted. Read access enables the compliance focal to detect and investigate security incidents, unusual behaviors, or unauthorized access attempts. By analyzing logs and access data, they can identify suspicious activities that might indicate a security breach.

The tutorial uses a fictitious company that is called *Example Corp* to create an access group template for compliance focals in an enterprise with the following structure. As you complete the tutorial, adapt each step to match your organization's accounts and structure.

![A four-tier enterprise that groups accounts according to department in an organization. For example, account groups are named Marketing, Development, and Sales. The account groups contain accounts for teams within those departments. For example, the Sales account group contains accounts for Direct, Online, and Enablement.](images/enterprise-by-dept.svg "An enterprise that organizes accounts according to department in the organization."){: caption="An enterprise that is organized by department" caption-side="bottom"}


## Before you begin
{: #before-ent-iam-tutorial}

- Check out [Best practices for assigning access in an enterprise](/docs/secure-enterprise?topic=secure-enterprise-access-enterprises) to learn more about the features, concepts, and components of the enterprise-managed access system.

- Verify that you're in the root enterprise account by going to **Manage > Enterprise** in the {{site.data.keyword.cloud_notm}} console.

- Verify that you're assigned the following {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) roles:
    * Template Administrator on All IAM Account Management services
    * Template Assignment Administrator on All IAM Account Management services
    * Viewer role on the Enterprise service

## Create the access group template
{: #ag-template}
{: step}

An access group template establishes consistent access for group members across your multi-account cloud environment. It is a predefined set of permissions and policies that you can apply consistently across multiple accounts for a specific group of users.

Access group templates reduce policy drift in common access groups in your enterprise.
{: tip}

1. Go to **Manage > Access (IAM) > (Enterprise) Templates** in the {{site.data.keyword.cloud_notm}} console.
1. Click **Create**.
1. Name the access group template **Compliance focal template**.
1. Enter a description such as "Compliance focals need read access to all resources in child accounts to complete their day to day tasks like incident response and verifying encryption."

   The template name and description are shown only in your enterprise account.
   {: note}

1. Name the access group **Compliance focal group**.
1. Enter a description such as "Enterprise compliance focals have Read access to all resources in this account to complete their day to day tasks like incident response and verifying encryption. This is an enterprise-managed access group."

   The access group name and description are shown in child accounts and are visible to all users in that account.
   {: note}

1. Click **Create**.

After the access group template is created, you are directed to the template dashboard. From here, you can view the template details and customize the template for your compliance team.

## Add your compliance team to the template
{: #ag-members}
{: step}

You can add users to an access group template that are members of the enterprise account. Compliance focals are members of the root enterprise account because they monitor the security and compliance posture from a holistic perspective. They need access to child accounts to gather evidence that can be accessed from within only those accounts.

1. Go to **Members**.
1. Click **Add**.
1. Select the compliance focals from the list of enterprise users.
1. Click **Add**.
1. Repeat the steps to add more members.


Leave the action controls for **Add members** and **Remove members** set to **No** so that access group administrators in child accounts can't add local members to or remove enterprise members from the enterprise-managed access group in their account.
{: note}

## Define the access
{: #ag-access}
{: step}

Define the set of roles that compliance focals can use in multiple accounts. Complete the following steps to add an access policy to your access group template:

1. Go to **Access**.
1. Click **Add**.
1. Click **Create**.
   1. Name the policy "Compliance focal". It's recommended to choose a policy name that clearly indicates the purpose.
   1. Enter the description "Read access on all resources and Viewer access on resource groups" and click **Next**.
   1. Select **All Identity and Access enabled services** and click **Next**.
   1. Select **All resources** click **Next**.
   1. Select the **Reader** role so that compliance focals can perform read-only actions within a service such as viewing service-specific resources.
   1. Select the **Viewer** role for resource group access and click **Next**. The Viewer role grants compliance focals the ability to view all resource groups in the account, which they might need if there is a security incident.
   1. Select the **Viewer** role and click **Next**. This way compliance focals can view service instances, but can't modify them.
   1. Click **Add** to create the policy template.
1. Click **Add** to add the policy template to the access group template.

Compliance focals need only this one policy to see the overall compliance posture for a group of sevices in child accounts. Leave the action control for **Add policies** set to **No** so that access group administrators in child accounts can't add more policies to the enterprise-managed access group in their account.

## Review and commit your access group template
{: #ag-review}
{: step}

You can't change a template's configuration when it's committed. The commitment step makes sure that the access that is defined at the enterprise level remains consistent and secure. Any new changes require a new version, which is managed intentionally.

1. Click **Review**.
1. Take a minute to review the details of the access group template. Committing a template can't be undone, so make sure that everything is correct.
1. Click the checkbox to confirm that you understand the impact of committing your template.
1. Click **Commit**.

After you commit the template, the Template Assignment Administrator can assign the access group template to the child accounts where compliance focals need access.

## Assign your access group template to child accounts
{: #ag-assign}
{: step}

When you assign the access group template to child accounts, an instance of the access group is created in each account. To assign an access group template to child accounts, complete the following steps:

1. Click **Assign accounts**.
1. Select the accounts where compliance focals need access. For *Example Corp*, compliance focals need access in the entire Development account group, the Sales account group, and the Web account within the Marketing account group.
1. Click **Assign accounts**.

After the access group template is assigned, you are directed to the Assignment reports. From here, you can view the assignment details and manage where the template is assigned for your compliance team.

## Invite compliance focals to child accounts
{: #invite-compliance}
{: step}

For the compliance team to gain access in the child accounts where the access group template is assigned, you must invite them to those accounts.

1. Select which {{site.data.keyword.cloud_notm}} account you use by using the account switcher in the console.
1. Click the **Web** account.
1. Go to **Manage > Access (IAM) > Users**.
1. Click **Invite users**.
1. Enter the email addresses of your compliance team members.
1. Click **Invite**.

When the invitation is sent, each user must accept it to join the account. After they accept the invitation, your compliance team is granted the correct access in the Web account. Repeat these steps for each account in the **Development** and **Sales** account groups.

## Manage the lifecycle of your access group template
{: #ag-lifecycle}
{: step}

As your team changes and expands, you might need to add or remove members from the access group template for compliance focals. To update a template, you must create a new version and update the assignment to use the latest version.

1. Click **Create new version**.
1. Enter the same access group name and description as the previous version.
1. Select which objects you want to carry over to the new version.
   1. Select **Members** and **Access**.
1. Click **Create**.
1. Go to **Members**. In this example, a compliance team member has left the company, so you need to remove them from the access group template.

   Even if a user no longer has access to their email and accounts, failing to remove their permissions might pose a potential security risk. Maintaining a clear record of user access and removal is essential for auditing since many regulatory standards require you to show that access to systems and data is controlled and monitored.
   {: tip}

   1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") for the team member who left the company.
   1. Click **Remove** and confirm that you want to remove this member.
1. Click **Review**.
1. Click the checkbox to confirm that you understand the impact of committing your template.
1. Click **Commit**.

Now that the new version is committed, you can update the assignment to use this template. To do so, complete the following steps:

1. Go to **Manage > Access (IAM) > (Enterprise) Templates**.
1. Click the **Table expand** icon ![Table expand icon](../icons/table-expand.svg "Table expand") on the "Compliance focal" access group template.
1. Click `v1` of the template that is assigned in child accounts.
1. Go to **Assignments**.
1. Click **Update** on the **Web** account assignment.
1. Select `v2` of the access group template, which you committed.
1. Click **Update**.
1. Repeat these steps for the **Development** and **Sales** account groups.

When you update the assignments, you're directed back to the Assignment reports for `v1`. Notice that the `v1` is no longer assigned to the **Web** account or the **Development** and **Sales** account groups. To view the assignment reports for `v2`, click the **Versions icon** ![Versions icon](../icons/version.svg "Versions"), select `v2`, and click the Assignments tab.

## Next steps
{: #next-steps-ent-ag}

Now that you learned how to set up enterprise-managed access groups that are customized for compliance focals, you can continue to create access group templates for other teams. For more information, see [Creating enterprise-managed access group templates](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create&interface=ui).

Along with access to resources in child accounts, compliance focals might need additional access in the enterprise account. For more information, see [Assigning access to Security and Compliance Center](/docs/security-compliance?topic=security-compliance-assign-roles).
