---

copyright:
  years: 2023, 2024
lastupdated: "2024-09-18"

keywords: enterprise, enterprise account, multiple accounts, assign access, enterprise access, templates, enterprise managed, access, settings, migrate version, upgrade version, new version

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Creating enterprise-managed settings templates
{: #settings-template-create}

In an enterprise where you have many child accounts, it can be time-consuming and error-prone to manually configure IAM settings for each account. Make sure that all accounts in your enterprise adhere to your organization's security requirements by using settings templates.
{: shortdesc}

You can assign only one settings template to an account or account group. Create a settings template that applies to the most accounts and create a new one only for accounts that have special requirements. For example, you might have stricter multifactor authentication (MFA) requirements in production environment accounts.
{: note}

When you assign a settings template, child account users with the Administrator role on the [IAM identity service](/docs/account?topic=account-account-services&interface=ui#identity-service-account-management) can still manage settings for their account. If there are conflicting settings between the template-defined settings and settings that are managed within the account, the stricter setting is applied to the account. This means that the more restrictive configuration takes precedence and is enforced for that particular account.

## Before you begin
{: #before-you-settings-template}

- To learn about how enterprise-manged IAM templates make your enterprise more secure, see [How enterprise-managed IAM access works](/docs/secure-enterprise?topic=secure-enterprise-access-enterprises&interface=ui#how-enterprise-iam).

- Be a member of the enterprise account to create and assign enterprise-managed IAM templates.

- To create an enterprise-managed IAM template, make sure you're assigned the following access:
   * A policy with the Template Administrator role on All IAM Account Management services

- To assign an enterprise-managed IAM template to child accounts, make sure you're assigned the following access:
   * A policy with the Template Assignment Administrator role on All IAM Account Management services
   * A policy with at least the Viewer role on the Enterprise service

   By default, no users have the roles Template Administrator or Template Assignment Administrator, including the account owner.
   {: note}

- New and existing accounts in your enterprise must opt-in to enterprise-managed IAM. For more information, see [Opting in to enterprise-managed IAM](/docs/secure-enterprise?topic=secure-enterprise-enterprise-managed-opt-in).


## Creating a settings template
{: #create-settings-template}
{: ui}

Consider using settings templates when you have many child accounts that require the same IAM settings. For example, your organization might have internal standards or require compliance with industry regulations.

To create a settings template, complete the following steps:

1. Go to **Manage > Access (IAM) > Templates** in the {{site.data.keyword.cloud_notm}}.
1. Click **Settings > Create**.
1. Enter a name and description for the settings template that describes its purpose for enterprise users.
1. Click **Create**.

### (Optional) Adding IAM account settings
{: #iam-account-settings-template}
{: ui}

Customize your enterprise to meet business and regulatory requirements by defining IAM account settings in child accounts.



#### Restricting users from creating API keys
{: #restrict-api-key-template}
{: ui}

By default, all members of an account can create {{site.data.keyword.cloud}} API keys. However, by using the API key creation setting, that access can be restricted so that only members with the correct access can create these user API keys.

Restricting the ability to create API keys makes sense if you want users in specific child account to always log in interactively, meaning that you don't want automation scripts to run that can log in users automatically by using an API key. For more information about API keys, see [Managing user API keys](/docs/account?topic=account-userapikey).

If you turn on the API key creation setting, users in your account require specific access to create API keys, including the account owner. To restrict who can create API keys, use the following steps:

Enabling this setting affects only the creation of user API keys. It does not affect API keys for service IDs.
{: note}

1. Enable the **API key creation** setting in the Account section.
1. Click **Yes** to confirm.

Now that the setting is enabled to restrict users from creating API keys, you can create an access group template that grants the required access to enable specific users to continue creating user API keys. Remember, the account owner is also required to have this explicit access. For more information, see [Assigning access to create API keys with restrictions enabled](/docs/account?topic=account-allow-api-create&interface=ui#restrict-api-create-access).
{: important}

#### Restricting users from creating service IDs
{: ##restrict-service-id}
{: ui}

By default, all members of an account can create service IDs. However, by using the Service ID creation setting, access can be restricted so that only members with the correct access can create service IDs. For more information about Service IDs, see [Creating and working with service IDs](/docs/account?topic=account-serviceids).

If you enable the Service ID creation setting, users in child accounts require specific access to create service IDs, including the account owner. To restrict who can create service IDs, use the following steps:

1. Enable the **Service ID creation** setting in the Account section.
1. Click **Yes** to confirm.

Now that the setting is enabled to restrict users from creating service IDs, you can create an access group template that grants the required access to enable specific users to continue creating service IDs. Remember, the account owner is also required to have this explicit access. For more information, see [Assigning access to create service IDs with restrictions enabled](/docs/account?topic=account-restrict-service-id-create&interface=ui#assign-access-create-service-id-restrict)
{: important}

#### Allowing specific IP addresses
{: #specific-ips-template}
{: ui}

By default, all IP addresses can be used to log in to the {{site.data.keyword.cloud}} console and access classic infrastructure APIs. You can specify which IP addresses have access and which IP addresses are restricted. You can specify this access for child accounts. Currently, only public IP addresses are supported.

When you allow only specific IP addresses to access an {{site.data.keyword.cloud_notm}} account, users can't access the {{site.data.keyword.cloud_notm}} Shell CLI. This is because the Cloud Shell is hosted on a shared platform that can't satisfy the IP address allowlist.

Child account administrators can set an IP address restriction for individual users. When an IP address restriction is defined for both the account and the user, the IP address needs to match both specifications to be able to generate an IAM token.
{: note}

To restrict all users to using only specific IP addresses, complete the following steps:

1. From the Account section, enable the **IP address access** setting.
1. Enter the IP addresses. The IP addresses listed are the only ones from which users in the child accounts can log in to {{site.data.keyword.Bluemix}}.

   You can enter a single IP address `17.5.7.8`, an IP address range `17.5.7.8 - 17.5.9.5`, or IP subnets `17.5.7.8.0/16`, or a [network zone](/docs/account?topic=account-context-restrictions-whatis#network-zones-whatis) `networkZoneName`. Make sure to use IPv4 or IPv6 addresses, and to separate multiple values with a comma. If there is already an IP address restriction that exists, the resource overrides the restriction.
   {: note}

1. Click **Save**.


### (Optional) Adding authentication setting
{: #authentication-settings-template}
{: ui}

The following information is helpful to consider before you enable MFA in child accounts to ensure that you know how it affects all users in the child accounts:

- Enabling MFA for all users in your account affects all members of the account. If users in your account are members of multiple {{site.data.keyword.cloud_notm}} accounts, they must enroll for MFA at their next login even if they don't intend to use resources in the account where MFA is enabled.
- API keys for users and service IDs continue to work after MFA is enabled.
- MFA for your child accounts applies to user logins, but does not apply to API calls. If a user has permission to make API calls to resources in the account, the user can do so without completing MFA. If the user belongs to other accounts, the user can make API calls to resources in your account by using an API key from an account that did not require MFA.
- Plan a communication and support strategy for users in your account:
   - Choose a date and time that you plan to enable MFA that results in the least impact to your business.
   - Notify the users in your account after you enable MFA with information on how to get set up.

Enabling MFA does not affect users that are already logged in because MFA takes effect only at the time of login. Make sure that you notify child account users that MFA is enabled, and describe the impact to users at their next login.
{: tip}

To enable MFA in a settings template, complete the following steps:

1. Go to the Authentication section.
1. Select the type of MFA that you want to enable in your account. For more information about the MFA options, see [MFA options](/docs/account?topic=account-types#mfa-options).





### (Optional) Adding login session settings
{: #login-session-settings-template}
{: ui}

Improve the security of the accounts in your enterprise by requiring users to enter their login credentials at customized intervals. You can select the time that a user's active session can last before they need to enter their credentials again. You can also choose the duration that a user is inactive before they are signed out of their session and are required to enter their credentials again.

If a user is a member of multiple accounts, the lowest value of each setting is applied to their session. For example, a user is a member of two accounts: `dev account` and `prod account`. If `prod account` has a 15-minute inactivity timeout, and `dev account` has a 30-minute inactivity timeout, the 15-minute inactivity timeout is applied to both accounts.
{: note}



#### Set the duration of active sessions
{: #active-sessions-template}
{: ui}

An active session is how long a user is continuously working in their account. How an active session is gauged also depends on the duration of your session sign out due to inactivity limit. For instance, if you set the sign out to 2 hours, then the user would need to interact with the account one time between those 2 hours.

To define the active sessions settings, complete the following steps:

1. Click **Manage > Access (IAM) > Templates > Settings** in the {{site.data.keyword.cloud}} console.
1. Select your settings template.
1. Click **Login sessions**.
1. Go to the Login session section, click the **Edit** icon ![Edit icon](../icons/icon_write.svg "Edit") from the Active sessions tile.
1. Enter the time limit. The longest a session can last is 720 hours.
1. Click Save.


## Reviewing your settings template
{: #review-settings-template}
{: ui}

Review the settings template and commit it so that no further changes can be made to the version. This way, the Template Assignment Administrator can be sure that they assign the version when you confirm that it's ready.

1. Click **Overview > Review**.
1. Verify that the settings template is configured to your expectation.
1. Click the checkbox to confirm that you can't edit the version.
1. Click **Commit**.

## Assigning a settings template to child accounts
{: #assign-settings-template}
{: ui}

You can assign the versions of only one settings template at a time in your enterprise. You can assign different versions of the same settings template, but you can't use multiple settings templates in an enterprise. You might want to assign different versions to accounts with different requirements, like production and test accounts.

You can't assign an IAM template to the enteprise account, only child accounts.
{: note}

To assign a settings template, complete the following steps:

1. Click **Manage > Access (IAM) > Templates > Settings** in the {{site.data.keyword.cloud}} console.
1. Select your settings template.
1. Click **Assign accounts**.
1. Select specific accounts or entire account groups in your enterprise.

When you select an account group, all future accounts that you add to that account group inherit the settings you assign.
{: note}

1. Click **Assign accounts**.

If an assignment fails, click **Retry**.
{: tip}

## Creating a new version
{: #new-version-create-ui}
{: ui}

To create a new version of a settings template, complete the following steps:

1. Click **Manage > Access (IAM) > Templates > Settings** in the {{site.data.keyword.cloud}} console.
1. Select your settings template.
1. Click **New version** icon ![New version icon](../icons/new-version.svg "New version").
1. Enter a new template name and description or keep the ones that you're already using.

    Entering a new template name updates the template name for this version and all previous versions. The template description is stored separately for each version.
    {: note}

1. Select the settings that you want to carry over to your new version.
1. Click **Create**.
1. Make any other adjustments to the configuration.
1. Click **Review** and commit the new version. For more information, see [Reviewing your access group template](/docs/secure-enterprise?topic=secure-enterprise-settings-template-create&interface=ui#review-settings-template).

To assign a new version of a settings template, complete the following steps:
1. Go to **Manage > Access (IAM) > Templates** in the {{site.data.keyword.cloud_notm}} console.
1. Click **Settings**.
1. Click the **Table expand** icon ![Table expand icon](../icons/table-expand.svg "Table expand") on the template that you want to work with.
1. Select the version that's assigned to child accounts.
1. Click **Assignments**.
1. Click **Update** on the account or account group assignment where you want to assign a different version.
1. Click **Select a version**.
   1. Select the version that you want to replace the current version.
1. Click **Update**.
1. Repeat these steps for each account or account group where you want to assign a different version.

The new template version that you assign replaces the old version. Learn more about [Assigning a new version](/docs/secure-enterprise?topic=secure-enterprise-working-with-versions#new-version).
{: note}



## Removing an assignment
{: #remove-assignment-settings}
{: ui}

You can remove a template assignment from an account or account group where the template is assigned. You might want to do so if the template isn't working as intended. When you remove a template assignment from an account, by default the previous version of the template is reinstated. If the assignment you remove is for the first or only version of a template, the settings preferences in the child account are removed.

To remove an assignment, complete the following steps:
1. Click **Manage > Access (IAM) > Templates > Settings** in the {{site.data.keyword.cloud}} console.
1. Click **Settings** and select your settings template.
1. Click **Assignments**.
1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") **> Remove** on the account or account group where you want to remove the assignment.


## Deleting a settings template
{: #delete-settings-template}
{: ui}

Before you can delete a settings template, you must remove all assignments for all versions of the template. Deleting a settings template deletes all versions.

To delete a settings template, complete the following steps:
1. Click **Manage > Access (IAM) > Templates > Settings** in the {{site.data.keyword.cloud}} console.
1. Click **Settings**.
1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") on the settings template that you want to delete.
1. Click **Delete template**.
1. Confirm that you want to delete the settings template.

### Deleting a version
{: #delete-settings-template-version-ui}
{: ui}

You can also delete a specific version by completing the following steps:
To delete a settings template, complete the following steps:
1. Click **Manage > Access (IAM) > Templates > Settings** in the {{site.data.keyword.cloud}} console.
1. Click **Settings**.
1. Click the **Table expand** icon ![Table expand icon](../icons/table-expand.svg "Table expand") on the template that you want to work with.
1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") on the version that you want to delete.
1. Click **Delete version**.
1. Confirm that you want to delete the version.


## Creating a settings template by using the CLI
{: #create-settings-template-cli}
{: cli}

Consider using settings templates when you have many child accounts that require the same IAM settings. For example, your organization might have internal standards or require compliance with industry regulations.

To create a settings template by using the CLI, complete the following steps:

1. Create a JSON file that configures the settings template definition. For more information about the attributes that you can use in your JSON file, see the [IAM Identity API](/apidocs/iam-identity-token-api#create-account-settings-template).

   The following example JSON file specifies the `account_id` of the enterprise account, the `name` of the template, and the `account_settings`. The creation of API keys is restricted to users with the API Key Creator role on the Identity Service. The creation of service IDs is not restricted, so all members can create service IDs in the accounts where the settings template is assigned. The maximum sessions per identity is set to `5`, and the multifactor authentication is set to `LEVEL3`, which requires all users to use U2F MFA.
   ```json
   {
      "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
      "name": "AccountSettingsTemplate",
      "account_settings": {
         "restrict_create_platform_apikey": "RESTRICTED",
         "restrict_create_service_id":  "NOT_RESTRICTED",
         "max_sessions_per_identity": 5,
         "mfa": "LEVEL3",
      }
   }
   ```
   {: codeblock}

1. Create the settings template by using the `account-settings-template-create` command as shown in the following sample request:

   ```bash
   ibmcloud iam account-settings-template-create AccountSettingsTemplate --file /path/to/account_settings_template.json
   ```
   {: codeblock}

## Updating a settings template by using the CLI
{: #update-settings-template-cli}
{: cli}

You can update a settings template at any time before you commit it. To update a specific version of an account settings template, complete the following steps:

1. Update your JSON file with the new settings template configuration. For more information about the attributes that you can use in your JSON file, see the [IAM Identity API](/apidocs/iam-identity-token-api#update-account-settings-template-version).

   The following example JSON file specifies the `account_id` of the enterprise account, the `name` of the template, and the `account_settings`. The new configuration resets MFA to `NONE` and removes the other settings.

   ```json
   {
    "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
    "name": "AccountSettingsTemplateUpdated",
    "account_settings": {
        "mfa": "NONE",
    },
   }
   ```
   {: codeblock}

   When you update the template name, this updates the name for every version.
   {: note}

1. To update a specific version of an account settings template, use the `account-settings-template-version-update` command as shown in the following sample request:

   ```bash
   ibmcloud iam account-settings-template-version-update AccountSettingsTemplateUpdated 1 --file /path/to/account_settings_template.json
   ```
   {: codeblock}

## Committing a settings template
{: #commit-settings-template-cli}
{: cli}

Review the settings template and commit it so that no further changes can be made to the version. Commiting a version is a necessary step before you can assign it to child accounts. This way, the Template Assignment Administrator can be sure that they assign the version only when you have confirmed that it's ready.

The following sample request commits version `1` of the settings template named `AccountSettingsTemplateUpdated`:

```bash
ibmcloud iam account-settings-template-commit AccountSettingsTemplateUpdated 1
```
{: codeblock}

## Assigning a settings template to child accounts by using the CLI
{: #assign-settings-template-cli}
{: cli}

You can assign the versions of only one settings template at a time in your enterprise. You can assign different versions of the same settings template, but you can't use multiple settings templates in an enterprise. You might want to assign different versions to accounts with different requirements, like production and test accounts.

You can't assign an IAM template to the enteprise account, only child accounts.
{: note}

To create an assignment for an account settings template, complete the following steps:
1. List the settings templates in your enterprise account and note the template name and version number for the settings template that you want to assign to child accounts:

   ```bash
   ibmcloud iam account-settings-templates
   ```
   {: codeblock}

1. Assign the template to an `Account` or `AccountGroup` by using the `account-settings-assignment-create` command.

   ```bash
   ibmcloud iam account-settings-assignment-create AccountSettingsTemplateUpdated 1 AccountGroup 955fc2274567474f8da802d5c376504b
   ```
   {: codeblock}

If an assignment fails, use the `account-settings-assigment-update` method to retry.
{: tip}

## Creating a new version by using the CLI
{: #new-version-create-cli}
{: cli}

Create a new version of an account settings template when you need to make updates to a committed version. To create a new version, complete the following steps.

1. Update your JSON file with the new settings template configuration. For more information about the attributes that you can use in your JSON file, see the [IAM Identity API](/apidocs/iam-identity-token-api#update-account-settings-template-version).
1. Use the `account-settings-template-version-create` method to create a new version. The following sample request creates a new version of the template `AccountSettingsUpdated`.

   ```bash
   ibmcloud iam account-settings-template-version-create AccountSettingsUpdated --file /path/to/account_settings_template.json
   ```
   {: codeblock}

## Updating an assignment by using the CLI
{: #update-assignment-settings-cli}
{: cli}

Update an assignment to retry a failed assignment or migrate to a new version.

The new template version that you assign replaces the old version. Learn more about [Assigning a new version](/docs/secure-enterprise?topic=secure-enterprise-working-with-versions#new-version).
{: note}

1. List the assignments in the account by using the `account-settings-assignments` method. Take note of the `ASSIGNMENT_ID` for the assignment that you want to update.
1. Update the account settings assignment. If you're retrying an assignment, use the same version number. The following sample request migrates the assignment `AccountSettingsAssignment-63d65ed159ff463b8ec09ea77d22a05b` to version 2.

   ```bash
   ibmcloud iam account-settings-assignment-update AccountSettingsAssignment-63d65ed159ff463b8ec09ea77d22a05b 2
   ```
   {: codeblock}

## Removing an assignment by using the CLI
{: #remove-assignment-settings-cli}
{: cli}

You can remove a template assignment from an account or account group where the template is assigned. You might want to do so if the template isn't working as intended. When you remove a template assignment from an account, by default the previous version of the template is reinstated. If the assignment you remove is for the first or only version of a template, the enterprise-managed settings preferences in the child account are removed.

To remove an assignment, complete the following steps:

1. List the assignments in the account by using the `account-settings-assignments` method. Take note of the `ASSIGNMENT_ID` for the assignment that you want to remove.
1. Remove the assignment by using the `account-settings-assigment-delete` method. The following sample request removes the assignment `AccountSettingsAssignment-63d65ed159ff463b8ec09ea77d22a05b`.

   ```bash
   ibmcloud iam account-settings-assignment-delete AccountSettingsAssignment-63d65ed159ff463b8ec09ea77d22a05b
   ```
   {: codeblock}

## Deleting a version by using the CLI
{: #delete-settings-template-version-cli}
{: cli}

Before you can delete a settings template version, you must remove all assignments for that version of the template. Delete a specific version by completing the following steps:

1. List the settings templates in your enterprise account and note the template name and version number for the version that you want to delete:

   ```bash
   ibmcloud iam account-settings-templates
   ```
   {: codeblock}

1. Delete the version:

   ```bash
   ibmcloud iam account-settings-template-delete AccountSettingsTemplateUpdated 2
   ```
   {: #codeblock}

1. To delete all versions, repeat these steps. Make sure that you remove the assignments for each version first.

## Creating a settings template by using the API
{: #create-settings-template-api}
{: api}

Consider using settings templates when you have many child accounts that require the same IAM settings. For example, your organization might have internal standards or require compliance with industry regulations.

To create a settings template by using the API, complete the following steps:

1. Configure the settings template definition. For more information about the attributes that you can use in your JSON file, see the [IAM Identity API](/apidocs/iam-identity-token-api#create-account-settings-template).

   The following example JSON file specifies the `account_id` of the enterprise account, the `name` of the template, and the `account_settings`. The creation of API keys is restricted to users with the API Key Creator role on the Identity Service. The creation of service IDs is not restricted, so all members can create service IDs in the accounts where the settings template is assigned. The maximum sessions per identity is set to `5`, and the multifactor authentication is set to `LEVEL3`, which requires all users to use U2F MFA.
   {: curl}

   ```bash
   curl -X POST 'https://iam.test.cloud.ibm.com/v1/account_settings_templates' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN' -d '{
      "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
      "name": "my template",
      "account_settings": {
         "restrict_create_platform_apikey": "RESTRICTED",
         "restrict_create_service_id":  "NOT_RESTRICTED",
         "max_sessions_per_identity": 5,
         "mfa": "LEVEL3",
      },
   }'
   ```
   {: curl}
   {: codeblock}

   ```java
   AccountSettingsComponent accountSettings = new AccountSettingsComponent.Builder()
      .mfa("LEVEL1")
      .systemAccessTokenExpirationInSeconds("3000")
      .build();

   CreateAccountSettingsTemplateOptions createOptions = new CreateAccountSettingsTemplateOptions.Builder()
      .accountId(enterpriseAccountId)
      .name(accountSettingsTemplateName)
      .description("IAM enterprise account settings template example")
      .accountSettings(accountSettings)
      .build();

   Response<AccountSettingsTemplateResponse> createResponse = service.createAccountSettingsTemplate(createOptions).execute();
   AccountSettingsTemplateResponse createResult = createResponse.getResult();

   // Save the id for use by other test methods.
   accountSettingsTemplateId = createResult.getId();
   accountSettingsTemplateVersion = createResult.getVersion().longValue();

   System.out.println(createResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const settings = {
   mfa: "LEVEL1",
   system_access_token_expiration_in_seconds: "3000",
   }
   const templateParams = {
   name: "Example-Account-Settings-Template",
   description: "IAM enterprise account settings template example",
   accountId: enterpriseAccountId,
   accountSettings: settings,
   }

   try {
   const res = await iamIdentityService.createAccountSettingsTemplate(templateParams);
   accountSettingsTemplateEtag = res.headers.etag;
   const { result } = res;
   accountSettingsTemplateId = result.id;
   accountSettingsTemplateVersion = result.version;
   console.log(JSON.stringify(result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   account_settings = {}
   account_settings['mfa'] = 'LEVEL1'
   account_settings['system_access_token_expiration_in_seconds'] = 3000

   create_response = iam_identity_service.create_account_settings_template(
   name='Example-Account-Settings-Template',
   description='IAM enterprise account settings template example',
   account_id=enterprise_account_id,
   account_settings=account_settings,
   )

   account_settings_template = create_response.get_result()
   print('\ncreate_account_settings_template() response: ', json.dumps(account_settings_template, indent=2))

   global account_settings_template_id
   account_settings_template_id = account_settings_template['id']
   global account_settings_template_version
   account_settings_template_version = account_settings_template['version']
   ```
   {: python}
   {: codeblock}

   ```go
   settings := &iamidentityv1.AccountSettingsComponent{
   Mfa:                                  core.StringPtr("LEVEL1"),
   SystemAccessTokenExpirationInSeconds: core.StringPtr("3000"),
   }

   createOptions := &iamidentityv1.CreateAccountSettingsTemplateOptions{
   Name:            &accountSettingsTemplateName,
   Description:     core.StringPtr("GoSDK test Account Settings Template"),
   AccountID:       &enterpriseAccountID,
   AccountSettings: settings,
   }

   createResponse, response, err := iamIdentityService.CreateAccountSettingsTemplate(createOptions)

   b, _ := json.MarshalIndent(createResponse, "", "  ")
   fmt.Println(string(b))

   // Grab the ID and Etag value from the response for use in the update operation.
   accountSettingsTemplateId = *createResponse.ID
   accountSettingsTemplateVersion = *createResponse.Version
   accountSettingsTemplateEtag = response.GetHeaders().Get("Etag")
   ```
   {: go}
   {: codeblock}


## Updating a settings template by using the API
{: #update-settings-template-api}
{: api}

You can update a settings template at any time before you commit it. To update a specific version of an account settings template, complete the following steps:

1. List the settings templates in your enteprise account and note the `AccountSettingsTemplate` ID and ETag in the response for the template version that you want to update.

   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/account_settings_templates?account_id=5bbe28be34524sdbdaa34d37d1f2294a' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   ListAccountSettingsTemplatesOptions listOptions = new ListAccountSettingsTemplatesOptions.Builder()
      .accountId(enterpriseAccountId)
      .build();

   Response<AccountSettingsTemplateList> response = service.listAccountSettingsTemplates(listOptions).execute();
   AccountSettingsTemplateList result = response.getResult();

   System.out.println(result);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   }
   try {
   const res = await iamIdentityService.listAccountSettingsTemplates(params);
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   list_response = iam_identity_service.list_account_settings_templates(account_id=enterprise_account_id)

   account_settings_template_list = list_response.get_result()
   print('\nlist_account_settings_templates response: ', json.dumps(account_settings_template_list, indent=2))
   ```
   {: python}
   {: codeblock}

   ```go
   listOptions := &iamidentityv1.ListAccountSettingsTemplatesOptions{
   AccountID: &enterpriseAccountID,
   }

   listResponse, response, err := iamIdentityService.ListAccountSettingsTemplates(listOptions)

   b, _ := json.MarshalIndent(listResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}

1. Update the settings template definition.

   When you update the template name, this updates the name for every version.
   {: note}

   ```bash
   curl -X POST 'https://iam.test.cloud.ibm.com/v1/account_settings_templates/{template_id}/version' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN' -d '{
      "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
      "name": "my template name",
      "account_settings": {
         "mfa": "NONE",
      },
   }'
   ```
   {: curl}
   {: codeblock}

   ```java
   AccountSettingsComponent accountSettings = new AccountSettingsComponent.Builder()
      .mfa("LEVEL1")
      .systemAccessTokenExpirationInSeconds("3000")
      .build();
   UpdateAccountSettingsTemplateVersionOptions updateOptions = new UpdateAccountSettingsTemplateVersionOptions.Builder()
      .accountId(enterpriseAccountId)
      .templateId(accountSettingsTemplateId)
      .version(Long.toString(accountSettingsTemplateVersion))
      .ifMatch(accountSettingsTemplateEtag)
      .name(accountSettingsTemplateName)
      .description("IAM enterprise account settings template example - updated")
      .accountSettings(accountSettings)
      .build();

   Response<AccountSettingsTemplateResponse> updateResponse = service.updateAccountSettingsTemplateVersion(updateOptions).execute();
   AccountSettingsTemplateResponse updateResult = updateResponse.getResult();

   accountSettingsTemplateEtag = updateResponse.getHeaders().values("Etag").get(0);

   System.out.println(updateResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const settings = {
   mfa: "LEVEL1",
   system_access_token_expiration_in_seconds: "3000",
   }
   const params = {
   accountId: enterpriseAccountId,
   templateId: accountSettingsTemplateId,
   version: accountSettingsTemplateVersion,
   ifMatch: accountSettingsTemplateEtag,
   name: "Example-Account-Settings-Template",
   description: "IAM enterprise account settings template example - updated",
   accountSettings: settings,
   }
   try {
   const res = await iamIdentityService.updateAccountSettingsTemplateVersion(params);
   accountSettingsTemplateEtag = res.headers.etag;
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   account_settings = {}
   account_settings['mfa'] = 'LEVEL1'
   account_settings['system_access_token_expiration_in_seconds'] = 3000

   update_response = iam_identity_service.update_account_settings_template_version(
   account_id=enterprise_account_id,
   template_id=account_settings_template_id,
   version=str(account_settings_template_version),
   if_match=account_settings_template_etag,
   name='Example-Account-Settings-Template',
   description='IAM enterprise account settings template example - updated',
   account_settings=account_settings,
   )

   account_settings_template = update_response.get_result()
   print('\nupdate_account_settings_template() response: ', json.dumps(account_settings_template, indent=2))

   account_settings_template_etag = update_response.get_headers()['Etag']
   ```
   {: python}
   {: codeblock}

   ```go
   settings := &iamidentityv1.AccountSettingsComponent{
   Mfa:                                  core.StringPtr("LEVEL1"),
   SystemAccessTokenExpirationInSeconds: core.StringPtr("3000"),
   }

   updateOptions := &iamidentityv1.UpdateAccountSettingsTemplateVersionOptions{
   AccountID:       &enterpriseAccountID,
   TemplateID:      &accountSettingsTemplateId,
   Version:         core.StringPtr(strconv.FormatInt(accountSettingsTemplateVersion, 10)),
   IfMatch:         &accountSettingsTemplateEtag,
   Name:            &accountSettingsTemplateName,
   Description:     core.StringPtr("GoSDK test Account Settings Template - updated"),
   AccountSettings: settings,
   }

   updateResponse, response, err := iamIdentityService.UpdateAccountSettingsTemplateVersion(updateOptions)

   b, _ := json.MarshalIndent(updateResponse, "", "  ")
   fmt.Println(string(b))

   accountSettingsTemplateEtag = response.GetHeaders().Get("Etag")
   ```
   {: go}
   {: codeblock}


## Committing a settings templateby using the API
{: #commit-settings-template-api}
{: api}

Review the settings template and commit it so that no further changes can be made to the version. Commiting a version is a necessary step before you can assign it to child accounts. This way, the Template Assignment Administrator can be sure that they assign the version only when you have confirmed that it's ready.

1. Get the version of a trusted profile template that you want to review and commit.

   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/account_settings_templates/AccountSettingsTemplate-767fc1f6-c77c-4196-b3d6-a009a5a536e9/versions/1' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   GetAccountSettingsTemplateVersionOptions getOptions = new GetAccountSettingsTemplateVersionOptions.Builder()
      .templateId(accountSettingsTemplateId)
      .version(Long.toString(accountSettingsTemplateVersion))
      .build();

   Response<AccountSettingsTemplateResponse> response = service.getAccountSettingsTemplateVersion(getOptions).execute();
   AccountSettingsTemplateResponse getResult = response.getResult();

   // Grab the Etag value from the response for use in the update operation.
   accountSettingsTemplateEtag = response.getHeaders().values("Etag").get(0);

   System.out.println(getResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   get_response = iam_identity_service.get_account_settings_template_version(
   template_id=account_settings_template_id, version=str(account_settings_template_version)
   )

   account_settings_template = get_response.get_result()
   print('\nget_account_settings_template response: ', json.dumps(account_settings_template, indent=2))

   global account_settings_template_etag
   account_settings_template_etag = get_response.get_headers()['Etag']
   ```
   {: javascript}
   {: codeblock}

   ```python
   get_response = iam_identity_service.get_account_settings_template_version(
   template_id=account_settings_template_id, version=str(account_settings_template_version)
   )

   account_settings_template = get_response.get_result()
   print('\nget_account_settings_template response: ', json.dumps(account_settings_template, indent=2))

   global account_settings_template_etag
   account_settings_template_etag = get_response.get_headers()['Etag']
   ```
   {: python}
   {: codeblock}

   ```go
   getOptions := &iamidentityv1.GetAccountSettingsTemplateVersionOptions{
   TemplateID: &accountSettingsTemplateId,
   Version:    core.StringPtr(strconv.FormatInt(accountSettingsTemplateVersion, 10)),
   }

   getResponse, response, err := iamIdentityService.GetAccountSettingsTemplateVersion(getOptions)

   b, _ := json.MarshalIndent(getResponse, "", "  ")
   fmt.Println(string(b))

   // Grab the Etag value from the response for use in the update operation.
   accountSettingsTemplateEtag = response.GetHeaders().Get("Etag")
   ```
   {: go}
   {: codeblock}

1. Review the response to confirm that it's ready to commit.
1. Commit the version of the trusted profile template.

   ```bash
   curl -X POST 'https://iam.test.cloud.ibm.com/v1/account_settings_templates/{template_id}/{version}/commit' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   CommitAccountSettingsTemplateOptions commitOptions = new CommitAccountSettingsTemplateOptions.Builder()
      .templateId(accountSettingsTemplateId)
      .version(Long.toString(accountSettingsTemplateVersion))
      .build();

   Response<Void> commitResponse = service.commitAccountSettingsTemplate(commitOptions).execute();
   ```
   {: java}
   {: codeblock}

   ```javascript
   const commitParams = {
   templateId: accountSettingsTemplateId,
   version: accountSettingsTemplateVersion,
   }
   try {
   const res = await iamIdentityService.commitAccountSettingsTemplate(commitParams);
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   commit_response = iam_identity_service.commit_account_settings_template(
   template_id=account_settings_template_id, version=str(account_settings_template_version)
   )
   ```
   {: python}
   {: codeblock}

   ```go
   commitOptions := &iamidentityv1.CommitAccountSettingsTemplateOptions{
   TemplateID: &accountSettingsTemplateId,
   Version:    core.StringPtr(strconv.FormatInt(accountSettingsTemplateVersion, 10)),
   }

   response, err := iamIdentityService.CommitAccountSettingsTemplate(commitOptions)
   ```
   {: go}
   {: codeblock}


## Assigning a settings template to child accounts by using the API
{: #assign-settings-template-api}
{: api}

You can assign the versions of only one settings template at a time in your enterprise. You can assign different versions of the same settings template, but you can't use multiple settings templates in an enterprise. You might want to assign different versions to accounts with different requirements, like production and test accounts.

You can't assign an IAM template to the enteprise account, only child accounts.
{: note}

To create an assignment for an account settings template, complete the following steps:

1. List the settings templates in your enteprise account and note the `AccountSettingsTemplate` ID and ETag in the response for the template version that you want to assign to child accounts.


   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/account_settings_templates?account_id=5bbe28be34524sdbdaa34d37d1f2294a' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   ListAccountSettingsTemplatesOptions listOptions = new ListAccountSettingsTemplatesOptions.Builder()
      .accountId(enterpriseAccountId)
      .build();

   Response<AccountSettingsTemplateList> response = service.listAccountSettingsTemplates(listOptions).execute();
   AccountSettingsTemplateList result = response.getResult();

   System.out.println(result);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   }
   try {
   const res = await iamIdentityService.listAccountSettingsTemplates(params);
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   list_response = iam_identity_service.list_account_settings_templates(account_id=enterprise_account_id)

   account_settings_template_list = list_response.get_result()
   print('\nlist_account_settings_templates response: ', json.dumps(account_settings_template_list, indent=2))
   ```
   {: python}
   {: codeblock}

   ```go
   listOptions := &iamidentityv1.ListAccountSettingsTemplatesOptions{
   AccountID: &enterpriseAccountID,
   }

   listResponse, response, err := iamIdentityService.ListAccountSettingsTemplates(listOptions)

   b, _ := json.MarshalIndent(listResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}


1. Assign the settings template to an `Account` or `AccountGroup`.

   ```bash
   curl -X POST 'https://iam.test.cloud.ibm.com/v1/account_settings_assignments' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN' -d '{
      "template_id": "AccountSettingsTemplate-cac1b203-5956-4981-bdec-0a4af4feab4d",
      "template_version": 1,
      "target_type": "Account",
      "target": "5bbe28be34524e88a34d37d1f2294a8a"
   }'
   ```
   {: curl}
   {: codeblock}

   ```java
   CreateAccountSettingsAssignmentOptions assignOptions = new CreateAccountSettingsAssignmentOptions.Builder()
      .templateId(accountSettingsTemplateId)
      .templateVersion(accountSettingsTemplateVersion)
      .targetType("Account")
      .target(enterpriseSubAccountId)
      .build();

   Response<TemplateAssignmentResponse> assignResponse = service.createAccountSettingsAssignment(assignOptions).execute();
   TemplateAssignmentResponse assignmentResult = assignResponse.getResult();

   // Save the id for use by other test methods.
   accountSettingsTemplateAssignmentId = assignmentResult.getId();
   // Grab the Etag value from the response for use in the update operation.
   accountSettingsTemplateAssignmentEtag = assignResponse.getHeaders().values("Etag").get(0);

   System.out.println(assignmentResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const assignParams = {
   templateId: accountSettingsTemplateId,
   templateVersion: accountSettingsTemplateVersion,
   targetType: "Account",
   target: enterpriseSubAccountId,
   }

   try {
   const assRes = await iamIdentityService.createAccountSettingsAssignment(assignParams);
   const { result } = assRes;
   accountSettingsTemplateAssignmentId = result.id;
   accountSettingsTemplateAssignmentEtag= assRes.headers.etag;
   console.log(JSON.stringify(result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   assign_response = iam_identity_service.create_account_settings_assignment(
   template_id=account_settings_template_id,
   template_version=account_settings_template_version,
   target_type='Account',
   target=enterprise_subaccount_id,
   )
   assignment = assign_response.get_result()
   print('\ncreate_account_settings_assignment() response: ', json.dumps(assignment, indent=2))
   global account_settings_template_assignment_id
   account_settings_template_assignment_id = assignment['id']
   global account_settings_template_assignment_etag
   account_settings_template_assignment_etag = assign_response.get_headers()['Etag']
   ```
   {: python}
   {: codeblock}

   ```go
   assignOptions := &iamidentityv1.CreateAccountSettingsAssignmentOptions{
   TemplateID:      &accountSettingsTemplateId,
   TemplateVersion: &accountSettingsTemplateVersion,
   TargetType:      core.StringPtr("Account"),
   Target:          &enterpriseSubAccountID,
   }

   assignResponse, response, err := iamIdentityService.CreateAccountSettingsAssignment(assignOptions)

   b, _ := json.MarshalIndent(assignResponse, "", "  ")
   fmt.Println(string(b))

   accountSettingsTemplateAssignmentEtag = response.GetHeaders().Get("Etag")
   accountSettingsTemplateAssignmentId = *assignResponse.ID
   ```
   {: go}
   {: codeblock}


If an assignment fails, use the `account-settings-assigment-update` method to retry.
{: tip}

## Creating a new version by using the API
{: #new-version-create-api}
{: api}

If you want to make changes to a settings template that's committed or assigned, create a new version.

   ```bash

   ```
   {: curl}
   {: codeblock}

   ```java

   ```
   {: java}
   {: codeblock}

   ```javascript

   ```
   {: javascript}
   {: codeblock}

   ```python

   ```
   {: python}
   {: codeblock}

   ```go
   settings := &iamidentityv1.AccountSettingsComponent{
   Mfa:                                  core.StringPtr("LEVEL1"),
   SystemAccessTokenExpirationInSeconds: core.StringPtr("2600"),
   RestrictCreatePlatformApikey:         core.StringPtr("RESTRICTED"),
   RestrictCreateServiceID:              core.StringPtr("RESTRICTED"),
   }

   createOptions := &iamidentityv1.CreateAccountSettingsTemplateVersionOptions{
   Name:            &accountSettingsTemplateName,
   Description:     core.StringPtr("GoSDK test Account Settings Template - new version"),
   AccountID:       &enterpriseAccountID,
   TemplateID:      &accountSettingsTemplateId,
   AccountSettings: settings,
   }

   createResponse, response, err := iamIdentityService.CreateAccountSettingsTemplateVersion(createOptions)

   b, _ := json.MarshalIndent(createResponse, "", "  ")
   fmt.Println(string(b))

   accountSettingsTemplateVersion = *createResponse.Version
   ```
   {: go}
   {: codeblock}

Save the new version's `AccountSettingsTemplate` ID and version to use in subsequent calls.
{: tip}


## Updating an assignment by using the API
{: #update-assignment-settings-api}
{: api}

Update an assignment to migrate to a new version or retry a failed assignment.

The new template version that you assign replaces the old version. Learn more about [Assigning a new version](/docs/secure-enterprise?topic=secure-enterprise-working-with-versions#new-version).
{: note}

1. List the settings template assignments in your enterprise account and note the assignment ID, ETag and version for the assignment that you want to update:

   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/account_settings_assignments?account_id=5bbe28be34524sdbdaa34d37d1f2294a' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   ListAccountSettingsTemplatesOptions listOptions = new ListAccountSettingsTemplatesOptions.Builder()
      .accountId(enterpriseAccountId)
      .build();

   Response<AccountSettingsTemplateList> listResponse = service.listAccountSettingsTemplates(listOptions).execute();
   AccountSettingsTemplateList listResult = listResponse.getResult();

   System.out.println(listResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   templateId: accountSettingsTemplateId,
   }
   try {
   const res = await iamIdentityService.listAccountSettingsAssignments(params);
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   list_response = iam_identity_service.list_account_settings_assignments(
   account_id=enterprise_account_id, template_id=account_settings_template_id
   )
   assignment_list = list_response.get_result()
   print('\ncreate_account_settings_assignment() response: ', json.dumps(assignment_list, indent=2))
   ```
   {: python}
   {: codeblock}

   ```go
   listOptions := &iamidentityv1.ListAccountSettingsAssignmentsOptions{
   AccountID:  &enterpriseAccountID,
   TemplateID: &accountSettingsTemplateId,
   }

   listResponse, response, err := iamIdentityService.ListAccountSettingsAssignments(listOptions)

   b, _ := json.MarshalIndent(listResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}


1. Update the account settings assignment. If you're retrying an assignment, use the same version number.

   ```bash
   curl -X PATCH 'https://iam.test.cloud.ibm.com/v1/account_settings_assignments/<assignment_id>' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN' -d '{
      "template_version": 2
   }'
   ```
   {: curl}
   {: codeblock}

   ```java
   UpdateAccountSettingsAssignmentOptions updateOptions = new UpdateAccountSettingsAssignmentOptions.Builder()
      .assignmentId(accountSettingsTemplateAssignmentId)
      .templateVersion(accountSettingsTemplateVersion)
      .ifMatch(accountSettingsTemplateAssignmentEtag)
      .build();

   Response<TemplateAssignmentResponse> updateResponse = service.updateAccountSettingsAssignment(updateOptions).execute();
   TemplateAssignmentResponse updateResult = updateResponse.getResult();

   // Grab the Etag value from the response for use in the update operation.
   accountSettingsTemplateAssignmentEtag = updateResponse.getHeaders().values("Etag").get(0);

   System.out.println(updateResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const assignParams = {
   assignmentId: accountSettingsTemplateAssignmentId,
   templateVersion: accountSettingsTemplateVersion,
   ifMatch: accountSettingsTemplateAssignmentEtag,
   }

   try {
   const assRes = await iamIdentityService.updateAccountSettingsAssignment(assignParams);
   console.log(JSON.stringify(assRes.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   assign_response = iam_identity_service.update_account_settings_assignment(
   assignment_id=account_settings_template_assignment_id,
   template_version=account_settings_template_version,
   if_match=account_settings_template_assignment_etag,
   )
   assignment = assign_response.get_result()
   print('\nupdate_account_settings_template_assignment response: ', json.dumps(assignment, indent=2))
   account_settings_template_assignment_etag = assign_response.get_headers()['Etag']
   ```
   {: python}
   {: codeblock}

   ```go
   updateOptions := &iamidentityv1.UpdateAccountSettingsAssignmentOptions{
   AssignmentID:    &accountSettingsTemplateAssignmentId,
   TemplateVersion: &accountSettingsTemplateVersion,
   IfMatch:         &accountSettingsTemplateAssignmentEtag,
   }

   updateResponse, response, err := iamIdentityService.UpdateAccountSettingsAssignment(updateOptions)

   b, _ := json.MarshalIndent(updateResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}


## Removing an assignment by using the API
{: #remove-assignment-settings-api}
{: api}

You can remove a template assignment from an account or account group where the template is assigned. You might want to do so if the template isn't working as intended. When you remove a template assignment from an account, by default the previous version of the template is reinstated. If the assignment you remove is for the first or only version of a template, the enterprise-managed settings preferences in the child account are removed.

To remove an assignment, complete the following steps:

1. List the assignments in the account and note of the assignment ID for the assignment that you want to remove.


   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/account_settings_assignments?account_id=5bbe28be34524sdbdaa34d37d1f2294a' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   ListAccountSettingsTemplatesOptions listOptions = new ListAccountSettingsTemplatesOptions.Builder()
      .accountId(enterpriseAccountId)
      .build();

   Response<AccountSettingsTemplateList> listResponse = service.listAccountSettingsTemplates(listOptions).execute();
   AccountSettingsTemplateList listResult = listResponse.getResult();

   System.out.println(listResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   templateId: accountSettingsTemplateId,
   }
   try {
   const res = await iamIdentityService.listAccountSettingsAssignments(params);
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   list_response = iam_identity_service.list_account_settings_assignments(
   account_id=enterprise_account_id, template_id=account_settings_template_id
   )
   assignment_list = list_response.get_result()
   print('\ncreate_account_settings_assignment() response: ', json.dumps(assignment_list, indent=2))
   ```
   {: python}
   {: codeblock}

   ```go
   listOptions := &iamidentityv1.ListAccountSettingsAssignmentsOptions{
   AccountID:  &enterpriseAccountID,
   TemplateID: &accountSettingsTemplateId,
   }

   listResponse, response, err := iamIdentityService.ListAccountSettingsAssignments(listOptions)

   b, _ := json.MarshalIndent(listResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}


1. Remove the assignment.

   Removing an assignment might cause a previous assignment to become active. For more information, see [Working with template versions](/docs/secure-enterprise?topic=secure-enterprise-working-with-versions#remove-assignment).
   {: note}

   ```bash
   curl -X DELETE 'https://iam.test.cloud.ibm.com/v1/account_settings_assignments/<assignment_id>' -H 'Authorization: Bearer $TOKEN' }'
   ```
   {: curl}
   {: codeblock}

   ```java
   DeleteAccountSettingsAssignmentOptions deleteOptions = new DeleteAccountSettingsAssignmentOptions.Builder()
      .assignmentId(accountSettingsTemplateAssignmentId)
      .build();

   Response<ExceptionResponse> deleteResponse = service.deleteAccountSettingsAssignment(deleteOptions).execute();
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   assignmentId: accountSettingsTemplateAssignmentId,
   }
   try {
   const res = await iamIdentityService.deleteAccountSettingsAssignment(params);
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   delete_response = iam_identity_service.delete_account_settings_assignment(
   assignment_id=account_settings_template_assignment_id
   )
   ```
   {: python}
   {: codeblock}

   ```go
   deleteOptions := &iamidentityv1.DeleteAccountSettingsAssignmentOptions{
   AssignmentID: &accountSettingsTemplateAssignmentId,
   }

   excResponse, response, err := iamIdentityService.DeleteAccountSettingsAssignment(deleteOptions)
   ```
   {: go}
   {: codeblock}


## Deleting a version by using the API
{: #delete-settings-template-version-api}
{: api}

Before you can delete a settings template version, you must remove all assignments for that version of the template. Delete a specific version by completing the following steps:

1. List the settings templates in your enterprise account and note the settings template ID and version number for the version that you want to delete:

```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/account_settings_templates?account_id=5bbe28be34524sdbdaa34d37d1f2294a' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   ListAccountSettingsTemplatesOptions listOptions = new ListAccountSettingsTemplatesOptions.Builder()
      .accountId(enterpriseAccountId)
      .build();

   Response<AccountSettingsTemplateList> response = service.listAccountSettingsTemplates(listOptions).execute();
   AccountSettingsTemplateList result = response.getResult();

   System.out.println(result);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   }
   try {
   const res = await iamIdentityService.listAccountSettingsTemplates(params);
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   list_response = iam_identity_service.list_account_settings_templates(account_id=enterprise_account_id)

   account_settings_template_list = list_response.get_result()
   print('\nlist_account_settings_templates response: ', json.dumps(account_settings_template_list, indent=2))
   ```
   {: python}
   {: codeblock}

   ```go
   listOptions := &iamidentityv1.ListAccountSettingsTemplatesOptions{
   AccountID: &enterpriseAccountID,
   }

   listResponse, response, err := iamIdentityService.ListAccountSettingsTemplates(listOptions)

   b, _ := json.MarshalIndent(listResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}

1. Delete the version:

   ```bash

   ```
   {: curl}
   {: codeblock}

   ```java

   ```
   {: java}
   {: codeblock}

   ```javascript

   ```
   {: javascript}
   {: codeblock}

   ```python

   ```
   {: python}
   {: codeblock}

   ```go

   ```
   {: go}
   {: codeblock}

### Deleting all versions by using the API
{: #delete-all-settings-api}

Make sure that you remove the assignments for each version first.

```bash
curl -X DELETE 'https://iam.test.cloud.ibm.com/v1/account_settings_templates/AccountSettingsTemplate-767fc1f6-c77c-4196-b3d6-a009a5a536e9' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
```
{: curl}
{: codeblock}

```java
DeleteAllVersionsOfAccountSettingsTemplateOptions deleteTeplateOptions = new DeleteAllVersionsOfAccountSettingsTemplateOptions.Builder()
    .templateId(accountSettingsTemplateId)
    .build();

Response<Void> deleteResponse = service.deleteAllVersionsOfAccountSettingsTemplate(deleteTeplateOptions).execute();
```
{: java}
{: codeblock}

```javascript
const params = {
  templateId: accountSettingsTemplateId,
}
try {
  const res = await iamIdentityService.deleteAllVersionsOfAccountSettingsTemplate(params);
} catch (err) {
  console.warn(err);
}
```
{: javascript}
{: codeblock}

```python
delete_response = iam_identity_service.delete_all_versions_of_account_settings_template(
  template_id=account_settings_template_id
)
```
{: python}
{: codeblock}

```go
deleteOptions := &iamidentityv1.DeleteAllVersionsOfAccountSettingsTemplateOptions{
  TemplateID: &accountSettingsTemplateId,
}

response, err := iamIdentityService.DeleteAllVersionsOfAccountSettingsTemplate(deleteOptions)
```
{: go}
{: codeblock}

## Settings template limitations
{: #settings-template-limits}

Only one settings template can be assigned in an enteprise. You can assign multiple versions of the template, but not different templates.
