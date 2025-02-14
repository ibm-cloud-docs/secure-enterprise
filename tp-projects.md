---

copyright:

  years: 2023, 2025

lastupdated: "2025-02-10"

keywords: trusted profile, projects trusted profile, authorization, project auth, project security

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Using trusted profiles to authorize a project to deploy an architecture
{: #tp-project}

Some services cannot fully configure and deploy architectures by using trusted profiles. For more information, see [Known issues and limitations for projects](/docs/secure-enterprise?topic=secure-enterprise-known-issues#auth-known-issue).
{: important}

When you configure your deployable architecture, you are required to select an authentication method. A project can apply a [trusted profile](/docs/account?topic=account-create-trusted-profile), which grants the project access to deploy an architecture in the account where the trusted profile exists. This way, you can securely deploy an architecture without the need for key rotation.
{: shortdesc}

The project uses the trusted profile to create a service ID with the same permissions as the trusted profile and a fresh API key for that service ID to authorize each deployment. Because the temporary API key exists only for the lifetime of the operation, this improves security because it's harder to misuse. The trusted profile needs access to create a service ID and create and delete API keys for the service ID, as well as access to deploy the deployable architecture.

## Deploying an architecture in your account or another account
{: #deploy-accross-accounts}

You can deploy an architecture in your own account or in another account, also called a target account, by using trusted profiles.

Depending on your organization, deploying an architecture might require access to another account by using a trusted profile and coordinating with administrators in multiple accounts. If the {{site.data.keyword.cloud_notm}} Projects service in another account needs access to your account to deploy an architecture, use trusted profiles and service IDs to authorize deployments in your account.

## Before you begin
{: #before-tp-project}

Make sure that you create the trusted profile in the account where you want to deploy the architecture. If you have the following access, you can create trusted profiles:

- Account owner
- Administrator role on all account management services
- Administrator role on the IAM Identity Service. For more information, see [IAM Identity service](/docs/account?topic=account-account-services#identity-service-account-management).

All users have access to create a service ID in an account to which they are a member.

## Creating the trusted profile
{: #create-projects-tp}

Create a trusted profile that can do the following:
- Create a service ID
- Create and delete API keys for the service ID
- Deploy the deployable architecture

Complete the following steps:

1. Find the project CRN. The CRN is used to authorize deployments to a target acccount.
   - To find the project CRN while you're [editing a project configuration](/docs/secure-enterprise?topic=secure-enterprise-config-project#how-to-config), click the tooltip icon on the `trusted_profile_id` field and copy the CRN.
   - Otherwise, go to **Menu** ![Menu icon](../icons/icon_hamburger.svg "Menu") > **Projects** and click the relevant project. Click **Manage** > **Details** and copy the CRN.
1. Confirm that you are in the target account to which the project deploys.
2. In the {{site.data.keyword.cloud}} console, click **Manage** > **Access (IAM)**, and select **Trusted profiles**.
3. Click **Create**.
4. Describe your profile by providing a name and a description, then click **Continue**.

   In the description, provide a list of actions available for this trusted profile.
   {: tip}

1. Select **{{site.data.keyword.cloud_notm}} services**.
1. Input the CRN from step 1.
1. In the description, enter the project name and any relevant notes.
1. Click **Continue**.
1. Assign access.
   1. Select **Access policy**.
   1. Create a policy that grants the trusted profile access to create service IDs and manage service ID API keys:
      1. Select the **IAM Identity Service** and click **Next**.
      1. Select **All resources** and click **Next**.
      1. Select the Service ID Creator role and the Administrator role and click **Next**.
      1. Click **Add**.

      This enables the project to generate a unique, temporary API key for each deployment, avoiding the need to manually rotate API keys. For more information, see [Required access for managing service ID API keys](/docs/account?topic=account-serviceidapikeys&interface=ui#required-access-serviceid-keys)

   1. Create a policy that grants the trusted profile access to deploy the deployable architecture.

      You can choose from a couple of approaches to grant the service ID access to authorize deployments in your account. See [Granting wide-ranging access](/docs/secure-enterprise?topic=secure-enterprise-tp-project#serviceid-access-wide) or [Granting specific access](/docs/secure-enterprise?topic=secure-enterprise-tp-project#serviceid-access-specific) for more information.

   1. Click **Add**.
1. Click **Create**. 

### Granting wide-ranging access
{: #serviceid-access-wide}

Grant the trusted profile Administrator access to everything in the account by assigning two policies. Consider this option if you plan to deploy many deployable architectures to the same target account. Deployable architectures usually require extensive privileges in the target account since they typically deploy and configure a wide range of services and IAM policies on those services. You can use the same trusted profile for different deployable architectures across projects, eliminating the need to continuously update the trusted profile's access policies.

It's secure and convenient to give the trusted profile a wide range of access because the profile contains only a platform service, and not users. Projects also have many [governance checks](/docs/secure-enterprise?topic=secure-enterprise-understanding-projects#project-use) already in place, including pre-deployment validation and a required approval process. By granting Administrator access now, you don't need to update the policy for the multiple deployable architectures that you might use that require different levels of access. This is more secure than directly authorizing a user to have any privileges in the target account.
{: note}

1. To create the first policy, select **All Identity and Access enabled services** and click **Next**.
   1. Select **All resources** and click **Next**.
   1. For the resource group access, select the Administrator role and click **Next**.
   1. Select the Manager service role and the Administrator platform role.
   1. Click **Add**.
1. For the second policy, select **All Account Management services** and click **Next**.
   1. Select the Administrator role and click **Next**.
   1. Click **Add**.
1. Click **Create**. 

### Granting specific access based on the deployable architecture
{: #serviceid-access-specific}

Grant the trusted profile the minimum required access role for the configuration that you're deploying. Choose this option if you have one or only a few deployable architectures with the same access requirements that you plan to deploy to the same target account.

View the catalog page for specific access roles that are required for a given deployable architecture.
1. In the {{site.data.keyword.cloud_notm}} console, click **Catalog**.
1. Search for and select the deployable architecture that you're deploying.
1. Click **Deploy with {{site.data.keyword.bpfull_notm}}** to view the required access roles.

   You're not deploying yet. This is a quick way to view the required access roles.
   {: note}

1. Continue by assigning the trusted profile the required access roles that you viewed in the previous step.

   For more information about assigning access, see [Creating the service ID](/docs/secure-enterprise?topic=secure-enterprise-tp-project#serviceid-auto-tp).

1. Click **Create**.

### Granting specific access to existing resources
{: #serviceid-access-existing-resources}

If you are using a trusted profile to [organize existing resources in a project](/docs/secure-enterprise?topic=secure-enterprise-organize-resources), you can grant the trusted profile access to specific resources, as opposed to all of them. Choose this option if you want to limit which existing resources a project can manage.

1. To create the first policy, select **All Identity and Access enabled services** and click **Next**.
   1. Select **Specific resources**, scope the access to the resources you want, and click **Next**.
   1. For the resource group access, select the Administrator role and click **Next**.
   1. Select the Manager service role and the Administrator platform role.
   1. Click **Add**.
1. For the second policy, select **All Account Management services** and click **Next**.
   1. Select the Administrator role.
   1. Click **Add**.
1. Click **Create**.

## Creating the service ID
{: #serviceid-auto-tp}

After you create the trusted profile, it auto-generates a service ID. The service ID name begins with `iam-Profile`, ends with `platform-project-access`, and includes the ID of the trusted profile in between. If the service ID is ever deleted, it's re-created the next time that the trusted profile is used.

## Coordinating with the administrator on the {{site.data.keyword.cloud_notm}} Projects service
{: #coordinate-projects}

The project user who edits the architecture configuration needs identifying information for the trusted profile that you created to complete the authorization. Users need the Operator role or higher on the {{site.data.keyword.cloud_notm}} Projects service to edit a configuration.

To retrieve the trusted profile ID value, complete the following steps.

### Finding the trusted profile ID
{: #find-tp-id}

1. In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**, and select **Trusted profiles**.
1. Select the profile that you created for the deployable architecture authorization.
1. Click **Details**.
1. Copy the Profile ID that begins with `Profile`.
1. Give this ID to the relevant project user.
