---

copyright:
  years: 2023
lastupdated: "2024-08-08"

keywords: enterprise, enterprise account, multiple accounts, assign access, enterprise access, templates, enterprise managed, access, trusted profile, migrate version, upgrade version, new version

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Creating enterprise-managed trusted profile templates
{: #tp-template-create}

In an enterprise where you have many child accounts, it can be time-consuming and error-prone to manually configure trusted profiles in each account. Use trusted profile templates to dynamically grant federated users access to accounts in your enterprise.
{: shortdesc}

You can establish trust with only federated users when you use enterprise-managed trusted profile templates. To set up trusted profiles in child accounts for compute resources, {{site.data.keyword.cloud_notm}} services, and service IDs, see [Creating trusted profiles](/docs/account?topic=account-create-trusted-profile&interface=ui).
{: note}

A user doesn't need to be a member of an account to apply a trusted profile. A user can apply a profile if the user has the same identity provider (IdP) as the trusted profile and they meet the conditions of trust. For example, a user in your enterprise directory needs access to a group of accounts to accomplish a specific task. Create a trusted profile template with conditions of trust that targets that user and the policies that they need. Then, assign the trusted profile template to the accounts where they need access. Now, the user can switch between accounts by applying the trusted profile in each account, and they have consistent access across accounts.

Administrators on the Identity service can add trust relationships and policies to the enterprise-managed trusted profile in their account, but can't modify the ones that you define in the template.
{: important}

## Before you begin
{: #before-you-tp-template}

- Before you begin, you need to [Enable authentication from an external identity provider](/docs/account?topic=account-idp-integration).

- To learn about how enterprise-manged IAM templates make your enterprise more secure, see [How enterprise-managed IAM access works](/docs/secure-enterprise?topic=secure-enterprise-access-enterprises&interface=ui#how-enterprise-iam).

- You must be a member of the enterprise account to create and assign enterprise-managed IAM templates.

- To create an enterprise-managed IAM template, make sure that you're assigned the following access:
   * A policy with the Template Administrator role on All IAM Account Management services

- To assign an enterprise-managed IAM template to child accounts, make sure you're assigned the following access:
   * A policy with the Template Assignment Administrator role on All IAM Account Management services
   * A policy with at least the Viewer role on the Enterprise service

   By default, no users have the roles Template Administrator or Template Assignment Administrator, including the account owner.
   {: note}

- New and existing accounts in your enterprise must opt-in to enterprise-managed IAM. For more information, see [Opting in to enterprise-managed IAM](/docs/secure-enterprise?topic=secure-enterprise-enterprise-managed-opt-in).


## Creating a trusted profile template
{: #create-tp-template}
{: ui}

Consider using trusted profile templates when users need access to many child accounts or if a group of users need temporary access to multiple child accounts.

To create a trusted profile template, complete the following steps:

1. Go to **Manage > Access (IAM) > Templates** in the {{site.data.keyword.cloud_notm}} console.
1. Click **Create**.
1. Enter a name and description for the trusted profile template that describes its purpose for enterprise users.
1. Enter a name and description for the enterprise-managed trusted profile that describes its purpose for child account users.

   In the description, provide a list of actions available for this trusted profile.
   {: tip}

1. Click **Create**.

### (Optional) Add trust relationships
{: #add-tp-template}
{: ui}

Establish trust with federated users by creating conditions based on attributes from your corporate directory. When federated users meet the conditions that you define, they can apply the profile.

To establish trust, complete the following steps:

1. Click **Trust relationship > Add**.
1. Select **Users federated by IBMid** or **Users federated by IBM Cloud AppID** as the authentication method and input the default identity provider (IdP).
1. Add conditions based on your IdP data to define how and when federated users can apply the profile.
   1. Click Add a condition to define multiple conditions. Federated users must meet all the conditions to apply the trusted profile. For more information about the fields that are used to create conditions, see [IAM condition properties](/docs/account?topic=account-iam-condition-properties).
   1. View **Identity provider data** to search attribute names and values in your own personal data from your IdP. For more information, see [Using IdP data to build trusted profiles](/docs/account?topic=account-idp-integration#trusted-profiles-idp-data).
1. Define the session duration for how long a user can apply the profile before they must reauthenticate.
1. Click **Save**.



### (Optional) Add access policies
{: #add-access-tp-template}
{: ui}

Access policies grant access in child accounts to the federated users that can apply the profile.

1. Click **Access > Add**.
1. Enter a name and describe the policy that you want to assign.

   You create policy template with every policy that you configure for a trusted profile template. In the {{site.data.keyword.cloud_notm}} console, policy templates are view-only. You can reference a policy template to assign access in other enterprise IAM templates by using the CLI or API.
   {: note}

1. Select a service or group of services. Then, click **Next**.
1. Select any combination of roles to define the scope of access, and click **Next**.
1. (Optional) Add conditions to specify when you want the policy to grant access.
1. Click **Add**.

#### Remove access policies
{: #remove-policies-tp}
{: ui}

You can remove policies before a template is committed and assigned.



## Updating template details
{: #update-tp-template-name}
{: ui}

You can update the template name, trusted profile name, and descriptions at any time before you commit the template. To update template details, complete the following steps:

1. Go to **Manage > Access (IAM) > Templates**.
1. Select the trusted profile template that you want to update.
1. Click the **Edit** icon ![Edit icon](../icons/edit-tagging.svg "Edit") in the Details section.
1. Make necessary updates to the template name, trusted profile name, and descriptions.
1. Click **Save**.

If you need to make updates after you commit the template, create a new version.
{: tip}

## Reviewing your trusted profile template
{: #review-tp-template}
{: ui}

Review and commit the trusted profile template so that no further changes can be made to the version. This way, the Template Assignment Administrator can be sure that they assign the version only when you have confirmed that it's ready.

1. Click **Overview > Review**.
1. Verify that the trusted profile template is configured to your expectation.
1. Click the checkbox to confirm that you can't make changes to the version.
1. Click **Commit**.

## Assigning a trusted profile template to child accounts
{: #assign-tp-template}
{: ui}

Assign the trusted profile template to child accounts in your enterprise.

You can assign an IAM template to only child accounts and account groups, not the enteprise account.
{: note}

1. Click **Assign accounts**.
1. Select the accounts and account groups that you want to assign the template to.

   In each of the child accounts that you assign the template to, you create an enterprise-managed trusted profile. Users in the child account can determine that a trusted profile comes from an enterprise-managed IAM template by the [enterprise-managed]{: tag-cyan} tag on the profile.

1. Click **Assign**.

If an assignment fails, click **Retry**.
{: tip}

## Creating a new version
{: #new-version-tp-template}
{: ui}

If you want to make changes to a trusted profile template that's committed or assigned, create a new version. You can create a new version based on your latest version, or a different version that you select.

To create a new version of a trusted profile template, complete the following steps:
1. Go to **Manage > Access (IAM) > Templates** in the {{site.data.keyword.cloud_notm}} console.
1. Select your trusted profile template.
1. Click the **New version** icon ![New version icon](../icons/new-version.svg "New version").
1. Select the version that you want as the basis for your new version.
1. Enter a new template name and description or keep the ones that you're already using.

   Entering a new template name updates the template name for this version and all previous versions. The template description is stored separately for each version.
   {: note}

1. Enter a new trusted profile name and description or keep the ones you're already using.

    Entering a new trusted profile name replaces the previous trusted profile name that is shown in child accounts after you assign the new version.
    {: note}

1. Select the objects that you want to carry over to your new version.
   1. (Optional) Select **Trust relationships**.
   1. (Optional) Select **Access**.
1. Click **Create**.
1. Make any other adjustments to the configuration.
1. Click **Review** and commit the new version. For more information, see [Reviewing your trusted profile template](/docs/secure-enterprise?topic=secure-enterprise-tp-template-create&interface=ui#review-tp-template).

To assign a new version of a trusted profile template, complete the following steps:
1. Go to **Manage > Access (IAM) > Templates** in the {{site.data.keyword.cloud_notm}} console.
1. Click **Trusted profiles**.
1. Click the **Table expand** icon ![Table expand icon](../icons/table-expand.svg "Table expand") on the template that you want to work with.
1. Select the version that's currently assigned to child accounts.
1. Click **Assignments**.
1. Click **Update** on the account or account group assignment where you want to assign a different version.
1. Click **Select a version**.
   1. Select the version that you want to replace the current version.
1. Click **Update**.
1. Repeat these steps for each account or account group where you want to assign a different version.

The new template version that you assign replaces the old version. Learn more about [Assigning a new version](/docs/secure-enterprise?topic=secure-enterprise-working-with-versions#new-version).
{: note}

## Removing an assignment
{: #remove-assignment-tp}
{: ui}

You can remove a template assignment from one or more accounts where the template is assigned. You might want to do so if the template isn't working as intended. When you remove a template assignment from an account, by default the previous version of the template is reinstated. If the assignment you remove is for the first or only version of a template, the trusted profile in the child account is removed.

To remove an assignment, complete the following steps:
1. Go to **Manage > Access (IAM) > Templates** in the {{site.data.keyword.cloud_notm}} console.
1. Click **Trusted profiles** and select your trusted profile template.
1. Click **Update assignments**.
   1. To remove an assignment from one or a few accounts, deselect the accounts where you want to remove the template assignment.
   1. To remove an assignment from all accounts where the template is assigned, click **Unassign all**.

## Creating a trusted profile template by using the CLI
{: #create-trusted-profile-template-cli}
{: cli}

Consider using trusted profile templates when you have many child accounts that require the same trusted profile. For example, your organization might have internal standards or require compliance with industry regulations.

To create a trusted profile template by using the CLI, complete the following steps:

1. Create a JSON file that configures the trusted profile template definition. For more information about the attributes that you can use in your JSON file, see the [IAM Identity API](/apidocs/iam-identity-token-api#create-profile-template).

   The following example JSON file specifies the `account_id` of the enterprise account, the `name` of the template, and the `profile` configuration. This trusted profile is for databases administrators. A specific user is allowed to apply the template as defined in `identities`. `rules` is also defined, which grants access to the profile based on SAML attributes.

   ```json
      {
      "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
      "name": "dbadmintemplate",
      "profile": {
         "name": "Profile for DB Admins",
         "description": "allows users to admin db instances",
         "identities": [
               {
                  "type": "user",
                  "identifier": "IBMid-123456789",
                  "accounts": [
                     "5bbe28be34524sdbdaa34d37d1f2294a"
                  ]
               }
         ],
         "rules": [
               {
                  "type": "Profile",
                  "realm_name": "${IDP_REALM_NAME}",
                  "expiration": 43200,
                  "conditions": [
                     {
                           "claim": "group",
                           "operator": "EQUALS",
                           "value": "\"admins\""
                     }
                  ]
               }
         ]
      },
      "policy_template_references": [
         {
               "id": "Policy Template-12345",
               "version": 1
         }
      ]
   }
   ```
   {: codeblock}


1. Create the trusted profile template by using the `trusted-profile-template-create` command as shown in the following sample request:

   ```bash
   ibmcloud iam trustedprofile-template-create dbadmintemplate --file /path/to/db_trusted-profile_template.json
   ```
   {: codeblock}

## Updating a trusted profile template by using the CLI
{: #update-trusted-profile-template-cli}
{: cli}

You can update a trusted profile template at any time before you commit it. To update a specific version of a trusted profile template, complete the following steps:

1. Update your JSON file with the new trusted profile template definition. For more information about the attributes that you can use in your JSON file, see the [IAM Identity API](/apidocs/iam-identity-token-api#update-profile-template-version).

  ```json
      {
      "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
      "name": "DBAdministrator",
      "profile": {
         "name": "Profile for DB Admins",
         "description": "allows users to admin db instances",
         "identities": [
               {
                  "type": "user",
                  "identifier": "IBMid-123456789",
                  "accounts": [
                     "5bbe28be34524sdbdaa34d37d1f2294a"
                  ]
               }
         ],
         "rules": [
               {
                  "type": "Profile",
                  "realm_name": "${IDP_REALM_NAME}",
                  "expiration": 43200,
                  "conditions": [
                     {
                           "claim": "group",
                           "operator": "EQUALS",
                           "value": "\"admins\""
                     }
                  ]
               }
         ]
      },
      "policy_template_references": [
         {
               "id": "Policy Template-12345",
               "version": 1
         }
      ]
   }
   ```
   {: codeblock}

   When you update the template name, this updates the name for every version.
   {: note}

1. To update a specific version of a trusted profile template, use the `trusted-profile-template-version-update` command as shown in the following sample request:

   ```bash
   ibmcloud iam trustedpprofile-template-version-update DBAdministrator 1 --file /path/to/db_trusted-profile_template.json
   ```
   {: codeblock}

## Committing a trusted profile template by using the CLI
{: #commit-trusted-profile-template-cli}
{: cli}

Review the trusted profile template and commit it so that no further changes can be made to the version. Commiting a version is a necessary step before you can assign it to child accounts. This way, the Template Assignment Administrator can be sure that they assign the version only when you have confirmed that it's ready.

The following sample request commits version `1` of the trusted profile template named `DBAdministrator`:

```bash
ibmcloud iam trusted-profile-template-version-commit DBAdministrator 1
```
{: codeblock}

## Assigning a trusted profile template to child accounts by using the CLI
{: #assign-trusted-profile-template-cli}
{: cli}

Assign the trusted profile template to child accounts in your enterprise.

You can assign an IAM template to only child accounts and account groups, not the enteprise account.
{: note}

To create an assignment for a trusted profile template, complete the following steps:
1. List the trusted profile templates in your enterprise account and note the template name and version number for the trusted profile template that you want to assign to child accounts:

   ```bash
   ibmcloud iam trusted-profile-templates
   ```
   {: codeblock}

1. Assign the template to an `Account` or `AccountGroup` by using the `account-trusted profile-assignment-create` command.

   ```bash
   ibmcloud iam trusted-profile-assignment-create DBAdministrator 1 AccountGroup 955fc2274567474f8da802d5c376504b
   ```
   {: codeblock}

If an assignment fails, use the `trusted-profile-assignment-update` method to retry.
{: tip}

## Creating a new version by using the CLI
{: #new-version-trusted-profile-template}
{: cli}

If you want to make changes to a trusted profile template that's committed or assigned, create a new version.

1. Update your JSON file with the new trusted profile template definition. For more information about the attributes that you can use in your JSON file, see the [IAM Identity API](/apidocs/iam-identity-token-api#create-profile-template-version).
1. Use the `trusted-profile-template-version-create` method to create a new version. The following sample request creates a new version of the template `DBAdministrator`.

   ```bash
   ibmcloud iam trusted-profile-template-version-create DBAdministrator --file /path/to/db_trusted-profile_template.json
   ```
   {: codeblock}

## Updating an assignment by using the CLI
{: #update-assignment-trusted-profile-cli}
{: cli}

Update an assignment to migrate to a new version or retry a failed assignment.

The new template version that you assign replaces the old version. Learn more about [Assigning a new version](/docs/secure-enterprise?topic=secure-enterprise-working-with-versions#new-version).
{: note}

1. List the trusted profile assignments in your enterprise account and note the `template_id` for the assignment that you want to update:

   ```bash
   ibmcloud iam trusted-profile-assignments
   ```
   {: codeblock}

1. Update the trusted profile assignment. If you're retrying an assignment, use the same version number. The following sample request migrates the assignment `ProfileTemplate-cac1b203-5956-4981-bdec-0a4af4feab4d` to version 2.

   ```bash
   ibmcloud iam trusted-profile-assignment-update ProfileTemplate-cac1b203-5956-4981-bdec-0a4af4feab4d 2
   ```
   {: codeblock}

## Removing an assignment by using the CLI
{: #remove-assignment-trusted-profile-cli}
{: cli}

You can remove a template assignment from an account or account group where the template is assigned. You might want to do so if the template isn't working as intended. When you remove a template assignment from an account, by default the previous version of the template is reinstated. If the assignment you remove is for the first or only version of a template, the enterprise-managed trusted profile  in the child account are removed.

To remove an assignment, complete the following steps:

1. List the assignments in the account by using the `account-trusted profile-assignments` method. Take note of the `ASSIGNMENT_ID` for the assignment that you want to remove.
1. Remove the assignment by using the `account-trusted profile-assigment-delete` method. The following sample request removes the assignment `AccountSettingsAssignment-63d65ed159ff463b8ec09ea77d22a05b`.

   ```bash
   ibmcloud iam account-trusted profile-assignment-delete AccountSettingsAssignment-63d65ed159ff463b8ec09ea77d22a05b
   ```
   {: codeblock}

## Deleting a version by using the CLI
{: #delete-trusted-profile-template-version-cli}
{: cli}

Before you can delete a trusted profile template version, you must remove all assignments for that version of the template. Delete a specific version by completing the following steps:

1. List the trusted profile templates in your enterprise account and note the template name and version number for the version that you want to delete:

   ```bash
   ibmcloud iam account-trusted profile-templates
   ```
   {: codeblock}

1. Delete the version:

   ```bash
   ibmcloud iam account-trusted profile-template-delete AccountSettingsTemplateUpdated 2
   ```
   {: #codeblock}

1. To delete all versions, repeat these steps. Make sure that you remove the assignments for each version first.

## Creating a trusted profile template by using the API
{: #create-trusted-profile-template-api}
{: api}

Consider using trusted profile templates when you have many child accounts that require the same trusted profile. For example, your organization might have internal standards or require compliance with industry regulations.

To create a trusted profile template by using the API, complete the following steps:

1. Configure the trusted profile template definition. For more information about the attributes that you can use, see the [IAM Identity API](/apidocs/iam-identity-token-api#create-profile-template).

   The following example specifies the `account_id` of the enterprise account, the `name` of the template, and the `profile` configuration. This trusted profile is for databases administrators. A specific user is allowed to apply the template as defined in `identities`. `rules` is also defined, which grants access to the profile based on SAML attributes.
   {: curl}

   ```bash
      {
      "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
      "name": "dbadmintemplate",
      "profile": {
         "name": "Profile for DB Admins",
         "description": "allows users to admin db instances",
         "identities": [
               {
                  "type": "user",
                  "identifier": "IBMid-123456789",
                  "accounts": [
                     "5bbe28be34524sdbdaa34d37d1f2294a"
                  ]
               }
         ],
         "rules": [
               {
                  "type": "Profile",
                  "realm_name": "${IDP_REALM_NAME}",
                  "expiration": 43200,
                  "conditions": [
                     {
                           "claim": "group",
                           "operator": "EQUALS",
                           "value": "\"admins\""
                     }
                  ]
               }
         ]
      },
      "policy_template_references": [
         {
               "id": "Policy Template-12345",
               "version": 1
         }
      ]
   }
   ```
   {: curl}
   {: codeblock}

      ```java
      ProfileClaimRuleConditions condition = new ProfileClaimRuleConditions.Builder()
         .claim("blueGroups")
         .operator("EQUALS")
         .value("\"cloud-docs-dev\"")
         .build();
      List<ProfileClaimRuleConditions> conditions = new ArrayList<>();
      conditions.add(condition);

      TrustedProfileTemplateClaimRule claimRule = new TrustedProfileTemplateClaimRule.Builder()
         .name("My Rule")
         .realmName(realmName)
         .type(claimRuleType)
         .expiration(43200)
         .conditions(conditions)
         .build();

      TemplateProfileComponentRequest profile = new TemplateProfileComponentRequest.Builder()
         .addRules(claimRule)
         .name(profileTemplateProfileName)
         .description("Trusted profile created from a template")
         .build();

      CreateProfileTemplateOptions createProfileTemplateOptions = new CreateProfileTemplateOptions.Builder()
         .name(profileTemplateName)
         .description("IAM enterprise trusted profile template example")
         .accountId(enterpriseAccountId)
         .profile(profile)
         .build();

      Response<TrustedProfileTemplateResponse> response = service.createProfileTemplate(createProfileTemplateOptions).execute();
      TrustedProfileTemplateResponse trustedProfileTemplateResult = response.getResult();

      // Save the id for use by other test methods.
      profileTemplateId = trustedProfileTemplateResult.getId();
      profileTemplateVersion = trustedProfileTemplateResult.getVersion().longValue();

      System.out.println(trustedProfileTemplateResult);
      ```
      {: java}
      {: codeblock}

      ```javascript
      const condition = {
      claim: "blueGroups",
      operator: "EQUALS",
      value: "\"cloud-docs-dev\"",
      }
      const claimRule = {
         name: "My Rule",
         realm_name: realmName,
         type: 'Profile-SAML',
         expiration: 43200,
         conditions: [condition],
      }
      const profile = {
      rules: [claimRule],
      name: "Profile-From-Example-Template",
      description: "Trusted profile created from a template",
      }
      const templateParams = {
      name: "Example-Profile-Template",
      description: "IAM enterprise trusted profile template example",
      accountId: enterpriseAccountId,
      profile: profile,
      }

      try {
      const res = await iamIdentityService.createProfileTemplate(templateParams);
      profileTemplateEtag = res.headers.etag;
      const { result } = res;
      profileTemplateId = result.id;
      profileTemplateVersion = result.version;
      console.log(JSON.stringify(result, null, 2));
      } catch (err) {
      console.warn(err);
      }
      ```
      {: javascript}
      {: codeblock}

      ```python
      profile_claim_rule_conditions = {}
      profile_claim_rule_conditions['claim'] = 'blueGroups'
      profile_claim_rule_conditions['operator'] = 'EQUALS'
      profile_claim_rule_conditions['value'] = '\"cloud-docs-dev\"'

      profile_claim_rule = {}
      profile_claim_rule['name'] = 'My Rule'
      profile_claim_rule['realm_name'] = 'https://sdk.test.realm/1234'
      profile_claim_rule['type'] = 'Profile-SAML'
      profile_claim_rule['expiration'] = 43200
      profile_claim_rule['conditions'] = [profile_claim_rule_conditions]

      profile = {}
      profile['name'] = 'Profile-From-Example-Template'
      profile['description'] = 'Trusted profile created from a template'
      profile['rules'] = [profile_claim_rule]

      create_response = iam_identity_service.create_profile_template(
      name='Example-Profile-Template',
      description='IAM enterprise trusted profile template example',
      account_id=enterprise_account_id,
      profile=profile,
      )

      profile_template = create_response.get_result()
      print('\ncreate_profile_template() response: ', json.dumps(profile_template, indent=2))

      global profile_template_id
      profile_template_id = profile_template['id']
      global profile_template_version
      profile_template_version = profile_template['version']
      ```
      {: python}
      {: codeblock}

      ```go
      profileClaimRuleConditions := new(iamidentityv1.ProfileClaimRuleConditions)
      profileClaimRuleConditions.Claim = core.StringPtr("blueGroups")
      profileClaimRuleConditions.Operator = core.StringPtr("EQUALS")
      profileClaimRuleConditions.Value = core.StringPtr("\"cloud-docs-dev\"")

      profileTemplateClaimRule := new(iamidentityv1.TrustedProfileTemplateClaimRule)
      profileTemplateClaimRule.Name = core.StringPtr("My Rule")
      profileTemplateClaimRule.RealmName = &realmName
      profileTemplateClaimRule.Type = &claimRuleType
      profileTemplateClaimRule.Expiration = core.Int64Ptr(int64(43200))
      profileTemplateClaimRule.Conditions = []iamidentityv1.ProfileClaimRuleConditions{*profileClaimRuleConditions}

      profile := new(iamidentityv1.TemplateProfileComponentRequest)
      profile.Name = &profileTemplateProfileName
      profile.Description = core.StringPtr("Example Profile created from Profile Template")
      profile.Rules = []iamidentityv1.TrustedProfileTemplateClaimRule{*profileTemplateClaimRule}

      createOptions := &iamidentityv1.CreateProfileTemplateOptions{
      Name:        &profileTemplateName,
      Description: core.StringPtr("Example Profile Template"),
      AccountID:   &enterpriseAccountID,
      Profile:     profile,
      }

      createResponse, response, err := iamIdentityService.CreateProfileTemplate(createOptions)

      b, _ := json.MarshalIndent(createResponse, "", "  ")
      fmt.Println(string(b))

      // Grab the ID and Etag value from the response for use in the update operation
      profileTemplateId = *createResponse.ID
      profileTemplateVersion = *createResponse.Version
      profileTemplateEtag = response.GetHeaders().Get("Etag")
      ```
      {: go}
      {: codeblock}

Save the ProfileTemplate ID and entity_tag value from the response for use in update operations.
{ tip}

## Updating a trusted profile template by using the API
{: #update-trusted-profile-template-api}
{: api}

You can update a trusted profile template at any time before you commit it. For more information about the attributes that you can use in your JSON file, see the [IAM Identity API](/apidocs/iam-identity-token-api#update-profile-template-version). To update a specific version of a trusted profile template, complete the following steps:

1. List the trusted profile templates in your enteprise account and note the `ProfileTemplate` ID and ETag in the response for the template version that you want to update.

   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/profile_templates?account_id=5bbe28be34524sdbdaa34d37d1f2294a' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   ListProfileTemplatesOptions listOptions = new ListProfileTemplatesOptions.Builder()
      .accountId(enterpriseAccountId)
      .build();

   Response<TrustedProfileTemplateList> response = service.listProfileTemplates(listOptions).execute();
   TrustedProfileTemplateList listResult = response.getResult();
   System.out.println(listResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   }
   try {
   const res = await iamIdentityService.listProfileTemplates(params);
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   list_response = iam_identity_service.list_profile_templates(account_id=enterprise_account_id)

   profile_template_list = list_response.get_result()
   print('\nlist_profile_templates response: ', json.dumps(profile_template_list, indent=2))
   ```
   {: python}
   {: codeblock}

   ```go
   listOptions := &iamidentityv1.ListProfileTemplatesOptions{
   AccountID: &enterpriseAccountID,
   }
   listResponse, response, err := iamIdentityService.ListProfileTemplates(listOptions)

   b, _ := json.MarshalIndent(listResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}

1. Update the trusted profile template definition.

  ```bash
   curl -X PUT 'https://iam.test.cloud.ibm.com/v1/profile_templates/{template_id}/versions/{version}' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN' -d '{
      "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
      "name": "db admin template",
      "profile": {
         "name": "Profile for DB Admins",
         "description": "allows users to admin db instances",
         "rules": [
               {
                  "type": "Profile",
                  "realm_name": "${IDP_REALM_NAME}",
                  "expiration": 43200,
                  "conditions": [
                     {
                           "claim": "name",
                           "operator": "EQUALS",
                           "value": "\"My Name\""
                     }
                  ]
               }
         ]
      },
      "policy_template_references": [
         {
               "id": "Policy Template-12345",
               "version": 1
         }
      ]
   }'
   ```
   {: curl}
   {: codeblock}

   ```java
   UpdateProfileTemplateVersionOptions updateOptions = new UpdateProfileTemplateVersionOptions.Builder()
      .accountId(enterpriseAccountId)
      .templateId(profileTemplateId)
      .version(Long.toString(profileTemplateVersion))
      .ifMatch(profileTemplateEtag)
      .name(profileTemplateName)
      .description("IAM enterprise trusted profile template example - updated")
      .build();

   Response<TrustedProfileTemplateResponse> updateResponse = service.updateProfileTemplateVersion(updateOptions).execute();
   TrustedProfileTemplateResponse updateResult = updateResponse.getResult();

   // Grab the Etag value from the response for use in the update operation.
   profileTemplateEtag = updateResponse.getHeaders().values("Etag").get(0);

   System.out.println(updateResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   templateId: profileTemplateId,
   version: profileTemplateVersion,
   ifMatch: profileTemplateEtag,
   name: "Example-Profile-Template",
   description: "IAM enterprise trusted profile template example - updated",
   }
   try {
   const res = await iamIdentityService.updateProfileTemplateVersion(params);
   profileTemplateEtag = res.headers.etag;
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   {
   "id": "ProfileTemplate-767fc1f6-c77c-4196-b3d6-a009a5a536e9",
   "version": 1,
   "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
   "name": "db admin template",
   "committed": false,
   "profile": {
      "name": "Profile for DB Admins",
      "description": "allows users to admin db instances",
      "rules": [
         {
         "type": "Profile-SAML",
         "realm_name": "${IDP_REALM_NAME}",
         "expiration": 43200,
         "conditions": [
            {
               "claim": "name",
               "operator": "EQUALS",
               "value": "\"My Name\""
            }
         ]
         }
      ]
   },
   "policy_template_references": [
      {
         "id": "Policy Template-12345",
         "version": "1"
      }
   ],
   "created_at": "2023-03-07T13:55:33:428+0000",
   "created_by_id": "IBMid-12345678901",
   "last_modified_at": "2023-03-07T13:55:33:428+0000",
   "last_modified_by_id": "IBMid-12345678901",
   "entity_tag": "1-2da85a8f1172fc3527378318d3182778",
   "crn": "crn:v1:staging:public:iam-identity::a/5bbe28be34524sdbdaa34d37d1f2294a::template:ProfileTemplate-767fc1f6-c77c-4196-b3d6-a009a5a536e9"
   }
   ```
   {: python}
   {: codeblock}

   ```go
   {
   "id": "ProfileTemplate-767fc1f6-c77c-4196-b3d6-a009a5a536e9",
   "version": 1,
   "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
   "name": "db admin template",
   "committed": false,
   "profile": {
      "name": "Profile for DB Admins",
      "description": "allows users to admin db instances",
      "rules": [
         {
         "type": "Profile-SAML",
         "realm_name": "${IDP_REALM_NAME}",
         "expiration": 43200,
         "conditions": [
            {
               "claim": "name",
               "operator": "EQUALS",
               "value": "\"My Name\""
            }
         ]
         }
      ]
   },
   "policy_template_references": [
      {
         "id": "Policy Template-12345",
         "version": "1"
      }
   ],
   "created_at": "2023-03-07T13:55:33:428+0000",
   "created_by_id": "IBMid-12345678901",
   "last_modified_at": "2023-03-07T13:55:33:428+0000",
   "last_modified_by_id": "IBMid-12345678901",
   "entity_tag": "1-2da85a8f1172fc3527378318d3182778",
   "crn": "crn:v1:staging:public:iam-identity::a/5bbe28be34524sdbdaa34d37d1f2294a::template:ProfileTemplate-767fc1f6-c77c-4196-b3d6-a009a5a536e9"
   }
   ```
   {: go}
   {: codeblock}


   When you update the template name, this updates the name for every version.
   {: note}


## Committing a trusted profile template by using the API
{: #commit-trusted-profile-template-api}
{: api}

Review the trusted profile template and commit it so that you can't be make any more changes to the version. Commiting a version is a necessary step before you can assign it to child accounts. This way, the Template Assignment Administrator can be sure that they assign the version only when you confirm that it's ready.

1. Get the version of a trusted profile template that you want to review and commit.

   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/profile_templates/{template_id}/versions/{version}' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   GetProfileTemplateVersionOptions getProfileTemplateOptions = new GetProfileTemplateVersionOptions.Builder()
      .templateId(profileTemplateId)
      .version(Long.toString(profileTemplateVersion))
      .build();

   Response<TrustedProfileTemplateResponse> response = service.getProfileTemplateVersion(getProfileTemplateOptions).execute();
   TrustedProfileTemplateResponse profileTemplateResult = response.getResult();

   profileTemplateEtag = response.getHeaders().values("Etag").get(0);

   System.out.println(profileTemplateResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   templateId: profileTemplateId,
   version: profileTemplateVersion,
   }
   try {
   const res = await iamIdentityService.getProfileTemplateVersion(params);
   profileTemplateEtag = res.headers.etag;
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   get_response = iam_identity_service.get_profile_template_version(
   template_id=profile_template_id, version=str(profile_template_version)
   )

   profile_template = get_response.get_result()
   print('\nget_profile_template response: ', json.dumps(profile_template, indent=2))

   global profile_template_etag
   profile_template_etag = get_response.get_headers()['Etag']
   profile_template_etag is not None
   ```
   {: python}
   {: codeblock}

   ```go
   getOptions := &iamidentityv1.GetProfileTemplateVersionOptions{
   TemplateID: &profileTemplateId,
   Version:    core.StringPtr(strconv.FormatInt(profileTemplateVersion, 10)),
   }
   getResponse, response, err := iamIdentityService.GetProfileTemplateVersion(getOptions)

   b, _ := json.MarshalIndent(getResponse, "", "  ")
   fmt.Println(string(b))

   profileTemplateEtag = response.GetHeaders().Get("Etag")
   ```
   {: go}
   {: codeblock}

1. Review the response to confirm that it's ready to commit.
1. Commit the version of the trusted profile template.

   ```bash
   curl -X POST 'https://iam.test.cloud.ibm.com/v1/profile_templates/{template_id}/{version}/commit' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   CommitProfileTemplateOptions commitOptions = new CommitProfileTemplateOptions.Builder()
      .templateId(profileTemplateId)
      .version(Long.toString(profileTemplateVersion))
      .build();

   Response<Void> commitResponse = service.commitProfileTemplate(commitOptions).execute();
   ```
   {: java}
   {: codeblock}

   ```javascript
   const commitParams = {
   templateId: profileTemplateId,
   version: profileTemplateVersion,
   }
   try {
   const res = await iamIdentityService.commitProfileTemplate(commitParams);
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   commit_response = iam_identity_service.commit_profile_template(
   template_id=profile_template_id, version=str(profile_template_version)
   )
   ```
   {: python}
   {: codeblock}

   ```go
   commitOptions := &iamidentityv1.CommitProfileTemplateOptions{
   TemplateID: &profileTemplateId,
   Version:    core.StringPtr(strconv.FormatInt(profileTemplateVersion, 10)),
   }

   response, err := iamIdentityService.CommitProfileTemplate(commitOptions)
   ```
   {: go}
   {: codeblock}


## Assigning a trusted profile template to child accounts by using the API
{: #assign-trusted-profile-template-api}
{: api}

Assign the trusted profile template to child accounts in your enterprise.

You can assign an IAM template to only child accounts and account groups, not the enteprise account.
{: note}

To create an assignment for a trusted profile template, complete the following steps:

1. List the trusted profile templates in your enteprise account and note the `ProfileTemplate` ID and version in the response for the template version that you want to assign.

   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/profile_templates?account_id=5bbe28be34524sdbdaa34d37d1f2294a' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   ListProfileTemplatesOptions listOptions = new ListProfileTemplatesOptions.Builder()
      .accountId(enterpriseAccountId)
      .build();

   Response<TrustedProfileTemplateList> response = service.listProfileTemplates(listOptions).execute();
   TrustedProfileTemplateList listResult = response.getResult();
   System.out.println(listResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   }
   try {
   const res = await iamIdentityService.listProfileTemplates(params);
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   list_response = iam_identity_service.list_profile_templates(account_id=enterprise_account_id)

   profile_template_list = list_response.get_result()
   print('\nlist_profile_templates response: ', json.dumps(profile_template_list, indent=2))
   ```
   {: python}
   {: codeblock}

   ```go
   listOptions := &iamidentityv1.ListProfileTemplatesOptions{
   AccountID: &enterpriseAccountID,
   }
   listResponse, response, err := iamIdentityService.ListProfileTemplates(listOptions)

   b, _ := json.MarshalIndent(listResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}

   ```bash
   ibmcloud iam trusted-profile-templates
   ```
   {: codeblock}

1. Assign the template to an `Account` or `AccountGroup`.

   ```bash
   curl -X POST 'https://iam.test.cloud.ibm.com/v1/profile_assignments' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN' -d '{
      "template_id": "ProfileTemplate-cac1b203-5956-4981-bdec-0a4af4feab4d",
      "template_version": 1,
      "target_type": "Account",
      "target": "5bbe28be34524e88a34d37d1f2294a8a"
   }'
   ```
   {: curl}
   {: codeblock}

   ```java
   CreateTrustedProfileAssignmentOptions assignOptions = new CreateTrustedProfileAssignmentOptions.Builder()
      .templateId(profileTemplateId)
      .templateVersion(profileTemplateVersion)
      .targetType("Account")
      .target(enterpriseSubAccountId)
      .build();

   Response<TemplateAssignmentResponse> assignResponse = service.createTrustedProfileAssignment(assignOptions).execute();
   TemplateAssignmentResponse assignmentResponseResult = assignResponse.getResult();

   // Save the id for use by other test methods.
   profileTemplateAssignmentId = assignmentResponseResult.getId();
   // Grab the Etag value from the response for use in the update operation.
   profileTemplateAssignmentEtag = assignResponse.getHeaders().values("Etag").get(0);

   System.out.println(assignmentResponseResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const assignParams = {
   templateId: profileTemplateId,
   templateVersion: profileTemplateVersion,
   targetType: "Account",
   target: enterpriseSubAccountId,
   }

   try {
   const assRes = await iamIdentityService.createTrustedProfileAssignment(assignParams);
   const { result } = assRes;
   profileTemplateAssignmentId = result.id;
   profileTemplateAssignmentEtag= assRes.headers.etag;
   console.log(JSON.stringify(result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   assign_response = iam_identity_service.create_trusted_profile_assignment(
   template_id=profile_template_id,
   template_version=profile_template_version,
   target_type='Account',
   target=enterprise_subaccount_id,
   )
   assignment = assign_response.get_result()
   print('\ncreate_trusted_profile_assignment() response: ', json.dumps(assignment, indent=2))
   global profile_template_assignment_id
   profile_template_assignment_id = assignment['id']
   global profile_template_assignment_etag
   profile_template_assignment_etag = assign_response.get_headers()['Etag']
   ```
   {: python}
   {: codeblock}

   ```go
   assignOptions := &iamidentityv1.CreateTrustedProfileAssignmentOptions{
   TemplateID:      &profileTemplateId,
   TemplateVersion: &profileTemplateVersion,
   TargetType:      core.StringPtr("Account"),
   Target:          &enterpriseSubAccountID,
   }

   assignResponse, response, err := iamIdentityService.CreateTrustedProfileAssignment(assignOptions)

   b, _ := json.MarshalIndent(assignResponse, "", "  ")
   fmt.Println(string(b))

   // Grab the Etag and id for use by other test methods.
   profileTemplateAssignmentEtag = response.GetHeaders().Get("Etag")
   profileTemplateAssignmentId = *assignResponse.ID
   ```
   {: go}
   {: codeblock}


If an assignment fails, use the [Update assigment operation](/apidocs/iam-identity-token-api#update-trusted-profile-assignment) to retry.
{: tip}

## Creating a new version by using the API
{: #new-version-trusted-profile-template-api}
{: api}

If you want to make changes to a trusted profile template that's committed or assigned, create a new version.

   ```bash
   curl -X POST 'https://iam.test.cloud.ibm.com/v1/profile_templates/{template_id}/versions/' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN' -d '{
      "account_id": "5bbe28be34524sdbdaa34d37d1f2294a",
      "name": "db admin template",
      "profile": {
         "name": "Profile for DB Admins",
         "description": "allows users to admin db instances",
         "rules": [
               {
                  "type": "Profile",
                  "realm_name": "${IDP_REALM_NAME}",
                  "expiration": 43200,
                  "conditions": [
                     {
                           "claim": "name",
                           "operator": "EQUALS",
                           "value": "\"My Name\""
                     }
                  ]
               }
         ]
      },
      "policy_template_references": [
         {
               "id": "Policy Template-12345",
               "version": 1
         }
      ]
   }'
   ```
   {: curl}
   {: codeblock}

   ```java
   ProfileClaimRuleConditions condition = new ProfileClaimRuleConditions.Builder()
      .claim("blueGroups")
      .operator("EQUALS")
      .value("\"cloud-docs-dev\"")
      .build();
   List<ProfileClaimRuleConditions> conditions = new ArrayList<>();
   conditions.add(condition);

   TrustedProfileTemplateClaimRule claimRule = new TrustedProfileTemplateClaimRule.Builder()
      .name("My Rule")
      .realmName(realmName)
      .type(claimRuleType)
      .expiration(43200)
      .conditions(conditions)
      .build();

   List<String> accounts = new ArrayList<String>();
   accounts.add(enterpriseAccountId);
   ProfileIdentityRequest profileIdentity = new ProfileIdentityRequest.Builder()
      .identifier(iamId)
      .accounts(accounts)
      .type("user")
      .description("Identity description")
      .build();
   List<ProfileIdentityRequest> identities = new ArrayList<ProfileIdentityRequest>();
   identities.add(profileIdentity);

   TemplateProfileComponentRequest profile = new TemplateProfileComponentRequest.Builder()
      .addRules(claimRule)
      .name(profileTemplateProfileName)
      .description("Trusted profile created from a template - new version")
      .identities(identities)
      .build();

   CreateProfileTemplateVersionOptions createOptions = new CreateProfileTemplateVersionOptions.Builder()
      .accountId(enterpriseAccountId)
      .templateId(profileTemplateId)
      .name(profileTemplateName)
      .description("IAM enterprise trusted profile template example - new version")
      .profile(profile)
      .build();

   Response<TrustedProfileTemplateResponse> createResponse = service.createProfileTemplateVersion(createOptions).execute();
   TrustedProfileTemplateResponse createResult = createResponse.getResult();

   // Save the version for use by other test methods.
   profileTemplateVersion = createResult.getVersion().longValue();
   System.out.println(createResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const condition = {
      claim: "blueGroups",
      operator: "EQUALS",
      value: "\"cloud-docs-dev\"",
   }
   const claimRule = {
      name: "My Rule",
      realm_name: realmName,
      type: 'Profile-SAML',
      expiration: 43200,
      conditions: [condition],
   }
   const identity = {
      identifier: iamId,
      accounts: [enterpriseAccountId],
      type: "user",
      description: "Identity description",
   }
   const profile = {
      rules: [claimRule],
      name: "Profile-From-Example-Template",
      description: "Trusted profile created from a template - new version",
      identities: [identity],
   }
   const templateParams = {
      templateId: profileTemplateId,
      name: "Example-Profile-Template",
      description: "IAM enterprise trusted profile template example - new version",
      accountId: enterpriseAccountId,
      profile: profile,
   }

   try {
      const res = await iamIdentityService.createProfileTemplateVersion(templateParams);
      const { result } = res;
      profileTemplateVersion = result.version;
      console.log(JSON.stringify(result, null, 2));
   } catch (err) {
      console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   profile_claim_rule_conditions = {}
   profile_claim_rule_conditions['claim'] = 'blueGroups'
   profile_claim_rule_conditions['operator'] = 'EQUALS'
   profile_claim_rule_conditions['value'] = '\"cloud-docs-dev\"'

   profile_claim_rule = {}
   profile_claim_rule['name'] = 'My Rule'
   profile_claim_rule['realm_name'] = 'https://sdk.test.realm/1234'
   profile_claim_rule['type'] = 'Profile-SAML'
   profile_claim_rule['expiration'] = 43200
   profile_claim_rule['conditions'] = [profile_claim_rule_conditions]

   profile_identity = {}
   profile_identity['identifier'] = iam_id
   profile_identity['accounts'] = [enterprise_account_id]
   profile_identity['type'] = 'user'
   profile_identity['description'] = 'Identity description'

   profile = {}
   profile['name'] = 'Profile-From-Example-Template'
   profile['description'] = 'Trusted profile created from a template - new version'
   profile['rules'] = [profile_claim_rule]
   profile['identities'] = [profile_identity]

   create_response = iam_identity_service.create_profile_template_version(
   template_id=profile_template_id,
   name='Example-Profile-Template',
   description='IAM enterprise trusted profile template example - new version',
   account_id=enterprise_account_id,
   profile=profile,
   )

   profile_template = create_response.get_result()
   print('\ncreate_profile_template_version() response: ', json.dumps(profile_template, indent=2))

   global profile_template_version
   profile_template_version = profile_template['version']
   ```
   {: python}
   {: codeblock}

   ```go
   profileClaimRuleConditions := new(iamidentityv1.ProfileClaimRuleConditions)
   profileClaimRuleConditions.Claim = core.StringPtr("blueGroups")
   profileClaimRuleConditions.Operator = core.StringPtr("EQUALS")
   profileClaimRuleConditions.Value = core.StringPtr("\"cloud-docs-dev\"")

   profileTemplateClaimRule := new(iamidentityv1.TrustedProfileTemplateClaimRule)
   profileTemplateClaimRule.Name = core.StringPtr("My Rule")
   profileTemplateClaimRule.RealmName = &realmName
   profileTemplateClaimRule.Type = &claimRuleType
   profileTemplateClaimRule.Expiration = core.Int64Ptr(int64(43200))
   profileTemplateClaimRule.Conditions = []iamidentityv1.ProfileClaimRuleConditions{*profileClaimRuleConditions}

   profile := new(iamidentityv1.TemplateProfileComponentRequest)
   profile.Name = &profileTemplateProfileName
   profile.Description = core.StringPtr("Example Profile created from Profile Template - new version")
   profile.Rules = []iamidentityv1.TrustedProfileTemplateClaimRule{*profileTemplateClaimRule}

   createOptions := &iamidentityv1.CreateProfileTemplateVersionOptions{
   Name:        &profileTemplateName,
   Description: core.StringPtr("Example Profile Template - new version"),
   AccountID:   &enterpriseAccountID,
   TemplateID:  &profileTemplateId,
   Profile:     profile,
   }

   createResponse, response, err := iamIdentityService.CreateProfileTemplateVersion(createOptions)

   b, _ := json.MarshalIndent(createResponse, "", "  ")
   fmt.Println(string(b))

   // save the new version to be used in subsequent calls
   profileTemplateVersion = *createResponse.Version
   ```
   {: go}
   {: codeblock}


## Updating an assignment by using the API
{: #update-assignment-trusted-profile-api}
{: api}

Update an assignment to migrate to a new version or retry a failed assignment.

The new template version that you assign replaces the old version. Learn more about [Assigning a new version](/docs/secure-enterprise?topic=secure-enterprise-working-with-versions#new-version).
{: note}

1. List the trusted profile assignments in your enterprise account and note the `TemplateAssignment` ID and version for the assignment that you want to update:

   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/profile_assignments?account_id=5bbe28be34524sdbdaa34d37d1f2294a' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   ListTrustedProfileAssignmentsOptions listOptions = new ListTrustedProfileAssignmentsOptions.Builder()
      .accountId(enterpriseAccountId)
      .templateId(profileTemplateId)
      .build();

   Response<TemplateAssignmentListResponse> listResponse = service.listTrustedProfileAssignments(listOptions).execute();
   TemplateAssignmentListResponse listResult = listResponse.getResult();
   System.out.println(listResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   templateId: profileTemplateId,
   }
   try {
   const res = await iamIdentityService.listTrustedProfileAssignments(params);
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   list_response = iam_identity_service.list_trusted_profile_assignments(
   account_id=enterprise_account_id, template_id=profile_template_id
   )
   assignment_list = list_response.get_result()
   print('\nlist_trusted_profile_assignments() response: ', json.dumps(assignment_list, indent=2))
   ```
   {: python}
   {: codeblock}

   ```go
   listOptions := &iamidentityv1.ListTrustedProfileAssignmentsOptions{
   AccountID:  &enterpriseAccountID,
   TemplateID: &profileTemplateId,
   }

   listResponse, response, err := iamIdentityService.ListTrustedProfileAssignments(listOptions)

   b, _ := json.MarshalIndent(listResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}


1. Update the trusted profile assignment. If you're retrying an assignment, use the same version number. The following sample request migrates the assignment to version 2.

   ```bash
   curl -X PATCH 'https://iam.test.cloud.ibm.com/v1/profile_assignments/<assignment_id>' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN' -d '{
      "template_version": 2
   }'
   ```
   {: curl}
   {: codeblock}

   ```java
   UpdateTrustedProfileAssignmentOptions updateOptions = new UpdateTrustedProfileAssignmentOptions.Builder()
      .assignmentId(profileTemplateAssignmentId)
      .templateVersion(profileTemplateVersion)
      .ifMatch(profileTemplateAssignmentEtag)
      .build();

   Response<TemplateAssignmentResponse> updateResponse = service.updateTrustedProfileAssignment(updateOptions).execute();
   TemplateAssignmentResponse updateResult = updateResponse.getResult();

   // Grab the Etag value from the response for use in the update operation.
   profileTemplateAssignmentEtag = updateResponse.getHeaders().values("Etag").get(0);

   System.out.println(updateResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const assignParams = {
   assignmentId: profileTemplateAssignmentId,
   templateVersion: profileTemplateVersion,
   ifMatch: profileTemplateAssignmentEtag,
   }

   try {
   const assRes = await iamIdentityService.updateTrustedProfileAssignment(assignParams);
   console.log(JSON.stringify(assRes.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   assign_response = iam_identity_service.update_trusted_profile_assignment(
   assignment_id=profile_template_assignment_id,
   template_version=profile_template_version,
   if_match=profile_template_assignment_etag,
   )
   assignment = assign_response.get_result()
   print('\nupdate_profile_template_assignment response: ', json.dumps(assignment, indent=2))
   profile_template_assignment_etag = assign_response.get_headers()['Etag']
   ```
   {: python}
   {: codeblock}

   ```go
   updateOptions := &iamidentityv1.UpdateTrustedProfileAssignmentOptions{
   AssignmentID:    &profileTemplateAssignmentId,
   TemplateVersion: &profileTemplateVersion,
   IfMatch:         &profileTemplateAssignmentEtag,
   }

   updateResponse, response, err := iamIdentityService.UpdateTrustedProfileAssignment(updateOptions)

   b, _ := json.MarshalIndent(updateResponse, "", "  ")
   fmt.Println(string(b))

   // Grab the Etag and id for use by other test methods.
   profileTemplateAssignmentEtag = response.GetHeaders().Get("Etag")
   ```
   {: go}
   {: codeblock}


## Removing an assignment by using the API
{: #remove-assignment-trusted-profile-api}
{: api}

You can remove a template assignment from an account or account group where the template is assigned. You might want to do so if the template isn't working as intended or isn't needed anymore. When you remove a template assignment from an account, by default the previous version of the template is reinstated if one exists. If the assignment you remove is for the first or only version of a template, the enterprise-managed trusted profile in the child accounts are removed.

To remove an assignment, complete the following steps:

1. List the trusted profile assignments in your enterprise account and note the `TemplateAssignment` ID for the assignment that you want to remove.

   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/profile_assignments?account_id=5bbe28be34524sdbdaa34d37d1f2294a' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   ListTrustedProfileAssignmentsOptions listOptions = new ListTrustedProfileAssignmentsOptions.Builder()
      .accountId(enterpriseAccountId)
      .templateId(profileTemplateId)
      .build();

   Response<TemplateAssignmentListResponse> listResponse = service.listTrustedProfileAssignments(listOptions).execute();
   TemplateAssignmentListResponse listResult = listResponse.getResult();
   System.out.println(listResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   templateId: profileTemplateId,
   }
   try {
   const res = await iamIdentityService.listTrustedProfileAssignments(params);
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   list_response = iam_identity_service.list_trusted_profile_assignments(
   account_id=enterprise_account_id, template_id=profile_template_id
   )
   assignment_list = list_response.get_result()
   print('\nlist_trusted_profile_assignments() response: ', json.dumps(assignment_list, indent=2))
   ```
   {: python}
   {: codeblock}

   ```go
   listOptions := &iamidentityv1.ListTrustedProfileAssignmentsOptions{
   AccountID:  &enterpriseAccountID,
   TemplateID: &profileTemplateId,
   }

   listResponse, response, err := iamIdentityService.ListTrustedProfileAssignments(listOptions)

   b, _ := json.MarshalIndent(listResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}

1. Remove the assignment.

Removing an assignment might cause a previous assignment to become active. For more information, see [Working with template versions](/docs/secure-enterprise?topic=secure-enterprise-working-with-versions#remove-assignment).
{: note}

   ```bash
   curl -X DELETE 'https://iam.test.cloud.ibm.com/v1/profile_assignments/<assignment_id>' -H 'Authorization: Bearer $TOKEN' }'
   ```
   {: curl}
   {: codeblock}

   ```java
   DeleteTrustedProfileAssignmentOptions deleteOptions = new DeleteTrustedProfileAssignmentOptions.Builder()
      .assignmentId(profileTemplateAssignmentId)
      .build();

   Response<ExceptionResponse> deleteResponse = service.deleteTrustedProfileAssignment(deleteOptions).execute();
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   assignmentId: profileTemplateAssignmentId,
   }
   try {
   const res = await iamIdentityService.deleteTrustedProfileAssignment(params);
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   delete_response = iam_identity_service.delete_trusted_profile_assignment(
   assignment_id=profile_template_assignment_id
   )
   ```
   {: python}
   {: codeblock}

   ```go
   deleteOptions := &iamidentityv1.DeleteTrustedProfileAssignmentOptions{
   AssignmentID: &profileTemplateAssignmentId,
   }
   excResponse, response, err := iamIdentityService.DeleteTrustedProfileAssignment(deleteOptions)
   ```
   {: go}
   {: codeblock}


## Deleting a version by using the API
{: #delete-trusted-profile-template-version}
{: api}

Before you can delete a trusted profile template version, you must remove all assignments for that version of the template. Delete a specific version by completing the following steps:

1. List the trusted profile templates in your enteprise account and note the `ProfileTemplate` ID and version in the response for the template version that you want to delete.

   ```bash
   curl -X GET 'https://iam.test.cloud.ibm.com/v1/profile_templates?account_id=5bbe28be34524sdbdaa34d37d1f2294a' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   ListProfileTemplatesOptions listOptions = new ListProfileTemplatesOptions.Builder()
      .accountId(enterpriseAccountId)
      .build();

   Response<TrustedProfileTemplateList> response = service.listProfileTemplates(listOptions).execute();
   TrustedProfileTemplateList listResult = response.getResult();
   System.out.println(listResult);
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   accountId: enterpriseAccountId,
   }
   try {
   const res = await iamIdentityService.listProfileTemplates(params);
   console.log(JSON.stringify(res.result, null, 2));
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   list_response = iam_identity_service.list_profile_templates(account_id=enterprise_account_id)

   profile_template_list = list_response.get_result()
   print('\nlist_profile_templates response: ', json.dumps(profile_template_list, indent=2))
   ```
   {: python}
   {: codeblock}

   ```go
   listOptions := &iamidentityv1.ListProfileTemplatesOptions{
   AccountID: &enterpriseAccountID,
   }
   listResponse, response, err := iamIdentityService.ListProfileTemplates(listOptions)

   b, _ := json.MarshalIndent(listResponse, "", "  ")
   fmt.Println(string(b))
   ```
   {: go}
   {: codeblock}

1. Delete the version:

   ```bash
   curl -X DELETE 'https://iam.test.cloud.ibm.com/v1/profile_templates/{template_id}/versions/{version}' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   DeleteProfileTemplateVersionOptions deleteOptions = new DeleteProfileTemplateVersionOptions.Builder()
      .templateId(profileTemplateId)
      .version("1")
      .build();

   Response<Void> deleteResponse = service.deleteProfileTemplateVersion(deleteOptions).execute();
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   templateId: profileTemplateId,
   version: 1,
   }
   try {
   const res = await iamIdentityService.deleteProfileTemplateVersion(params);
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   delete_response = iam_identity_service.delete_profile_template_version(
   template_id=profile_template_id, version='1'
   )
   ```
   {: python}
   {: codeblock}

   ```go
   deleteOptions := &iamidentityv1.DeleteProfileTemplateVersionOptions{
   TemplateID: &profileTemplateId,
   Version:    core.StringPtr("1"),
   }

   response, err := iamIdentityService.DeleteProfileTemplateVersion(deleteOptions)
   ```
   {: go}
   {: codeblock}

### Deleting all versions by using the API
{: #delete-all-tp-api}

Make sure that you remove the assignments for each version first.

   ```bash
   curl -X DELETE 'https://iam.test.cloud.ibm.com/v1/profile_templates/ProfileTemplate-767fc1f6-c77c-4196-b3d6-a009a5a536e9' -H 'Content-Type: application/json' -H 'Authorization: Bearer $TOKEN'
   ```
   {: curl}
   {: codeblock}

   ```java
   DeleteAllVersionsOfProfileTemplateOptions deleteTeplateOptions = new DeleteAllVersionsOfProfileTemplateOptions.Builder()
      .templateId(profileTemplateId)
      .build();

   Response<Void> deleteResponse = service.deleteAllVersionsOfProfileTemplate(deleteTeplateOptions).execute();
   ```
   {: java}
   {: codeblock}

   ```javascript
   const params = {
   templateId: profileTemplateId,
   }
   try {
   const res = await iamIdentityService.deleteAllVersionsOfProfileTemplate(params);
   } catch (err) {
   console.warn(err);
   }
   ```
   {: javascript}
   {: codeblock}

   ```python
   delete_response = iam_identity_service.delete_all_versions_of_profile_template(
   template_id=profile_template_id
   )
   ```
   {: python}
   {: codeblock}

   ```go
   deleteOptions := &iamidentityv1.DeleteAllVersionsOfProfileTemplateOptions{
   TemplateID: &profileTemplateId,
   }

   response, err := iamIdentityService.DeleteAllVersionsOfProfileTemplate(deleteOptions)
   ```
   {: go}
   {: codeblock}
