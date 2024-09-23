---

copyright:
  years: 2023, 2024
lastupdated: "2024-09-18"

keywords: enterprise, enterprise account, multiple accounts, enterprise access, policy templates, enterprise managed, policies, enterprise policy, template

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Creating enterprise-managed policy templates
{: #policy-template-create}

Configuring an access policy every time you need to grant access in an access group or trusted profile template can be time-consuming and error prone. Simplify the process and maintain uniformity in your policies by using enterprise-managed access policy templates to assign access or [create service to service authorizations](/docs/account?topic=account-serviceauth).
{: shortdesc}

Access policy templates define a policy without requiring a subject, and you can use them to grant access to multiple subjects.

For example, you might need to assign an access policy that grants administrator access on All IAM enabled services to multiple access groups or trusted profile templates so that members can create instances of IAM-enabled services. To reduce policy misconfigurations, create an access policy template called `Admin on All IAM enabled services`. Select the preconfigured policy when you need to assign that level of access in an access group or trusted profile template.

While a policy subject isn't required if you use the CLI or API, you do need one in the {{site.data.keyword.cloud_notm}} console. Each time that you assign an access policy to a trusted profile or access group template in the {{site.data.keyword.cloud_notm}} console, you create a policy template. You can reference policy templates to grant access in trusted profile and access group templates by using the console, CLI or API. View policy by going to **Manage > Access (IAM) > Templates > Access policies**.


Access policy templates support only the `v2/policies` schema. For more information, see the [IAM Policy Management API change log](/docs/account?topic=account-api-change-log).
{: note}

Policy templates have limited availability in the {{site.data.keyword.cloud_notm}} console. Use the API or CLI to create policy templates.
{: important}
{: ui}

## Creating a policy template in the console
{: ui}
{: #create-policy-template-ui}

Policy templates that you create in the {{site.data.keyword.cloud_notm}} console must be associated with an access group or trusted profile template. Learn more about adding access policies to [access group templates](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create&interface=ui#add-access-ag-template) and [trusted profile templates](/docs/secure-enterprise?topic=secure-enterprise-tp-template-create&interface=ui#add-access-tp-template).

## Deleting a policy template in the console
{: #delete-policy-template-ui}
{: ui}

Deleting a policy template deletes all version of the template. You can delete a policy template that's referenced in a **Draft** or **Committed** trusted profile or access group template. However, the policy template reference becomes invalid and you must [remove](/docs/secure-enterprise?topic=secure-enterprise-policy-template-create&interface=cli#remove-policy-templte-cli) it before you can assign the trusted profile or access group template to child accounts.

In the case that you want to delete a policy template that's referenced in a trusted profile or access group template that are assigned to child accounts, you must remove the trusted profile or access group template assignment from those child accounts. For more information, see [Removing an assignment for access group templates](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create#remove-assignment-ag) and [trusted profile templates](/docs/secure-enterprise?topic=secure-enterprise-tp-template-create#remove-assignment-tp).

To delete a policy template in the console, complete the following steps:

1. Go to **Manage > Access (IAM) > Templates** in the {{site.data.keyword.cloud_notm}}.
1. Click **Policies**.
1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Delete template**.
1. Confirm that you want to delete the template.

### Deleting a policy template version in the console
{: #delete-policy-template-version-ui}
{: ui}

To delete only a specific version, complete the following steps:

1. Go to **Manage > Access (IAM) > Templates** in the {{site.data.keyword.cloud_notm}}.
1. Click **Policies**.
1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Delete version**.
1. Confirm that you want to delete the version.

## Create an access policy template by using the CLI
{: #create-policy-template-cli}
{: cli}

If multiple access group or trusted profile templates require the same policy, create an access policy template.

To create an access policy template by using the CLI, complete the following steps:

1. Create a JSON file that configures the policy template definition. For more information about the attributes that you can use in your JSON file, see the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy-template).

   The following example JSON file specifies the `name` and `description` of the template and the `account_id` of the enteprise account. Then, the `policy` definition is defined. This policy template grants an access policy with Editor permissions on the {{site.data.keyword.compliance_long}} service. The policy includes a time-based rule that grants access from Monday to Friday all day, repeating weekly. For more information, see [Time-based conditions](/docs/account?topic=account-iam-condition-properties&interface=ui#policy-condition-properties) and [Limiting access with time-based conditions](/docs/account?topic=account-iam-time-based).

    ```json
    {
      "name": "SCCEditor",
      "description": "Grant Editor Role on Security and Compliance Center service",
      "account_id": "ACCOUNT_ID",
      "policy": {
        "type": "access",
        "description": "Grant Editor Role on Security and Compliance Center service",
        "control": {
            "grant": {
            "roles": [
              {
                "role_id": "crn:v1:bluemix:public:iam::::role:Editor"
              }
            ],
          },
        },
        "resource": [
          {
            "attributes": [
              {
                "key": "serviceName",
                "operator": "stringEquals",
                "value": "compliance"
              }
            ]
          }
        ],
        "rule" :{
          "key": "{{environment.attributes.day_of_week}}",
          "operator": "dayOfWeekAnyOf",
          "value": ["1+00:00", "2+00:00", "3+00:00", "4+00:00", "5+00:00"]
        },
        "pattern": "time-based-conditions:weekly:all-day"
      }
    }
    ```
    {: codeblock}

1. Use the `ap-template-create` command as shown in the following sample request:

```bash
imcloud iam access-policy-template-create --file /path/to/security_editor_access_policy_template.json
```
{: codeblock}

## Updating an access policy template by using the CLI
{: #update-policy-template-cli}
{: cli}

You can update the roles, resources, name, and description for a policy template at any time before you commit it.

To update policy template by using the CLI, complete the following steps:

1. Update your JSON file with the new policy template definition. For more information about the attributes that you can use in your JSON file, see the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy-template).

   When you update the template name, this updates the name for every version.
   {: note}

1. Use the `ap-template-version-update` command as shown in the following sample request:

   ```bash
   ibmcloud iam access-policy-template-update SCCEditor 1 --file /path/to/security_editor_access_policy_template.json
   ```
   {: codeblock}

   This updates version `1` of the `SCCEditor` policy template by reffering to the updated JSON file.

If you need to make updates after you commit the template, create a new version.
{: tip}

## Committing a policy template version by using the CLI
{: #commit-policy-version-cli}
{: cli}

Review the policy template and commit it so that no further changes can be made to the version. This way, the Template Assignment Administrator can be sure that they assign the version only when you have confirmed that it's ready.

1. Review the policy template before you commit it by using the `access-policy-template-version` method to view the specific verison.

   ```bash
   ibmcloud iam access-policy-template-version SCCEditor 1
   ```
   {: codeblock}

1. To commit a policy template by using the CLI, use the `ap-template-version-commit` method as shown in the following sample request:

   ```bash
   ibmcloud iam access-policy-template-version-commit SCCEditor 1
   ```
   {: codeblock}

## Granting access with a policy template by using the CLI
{: #grant-access-template-cli}
{: cli}

Unlike regular access policies, policy templates don't have an explicit subject that you're granting access to when you create it.

You can reference a policy template to assign access in an access group or trusted profile template by using the CLI.

1. List all access policy templates and note the ID of the policy template by using the `ap-templates` command as shown in the following sample request:

   ```bash
   ibmcloud iam access-policy-templates
   ```
   {: codeblock}

1. Use the policy template ID to assign access in access group templates and trusted profile templates.
   1. In the JSON file that configures an access group template or a trusted profile template, there's a `policy_template_references` parameter. Use the `id` and `version` of the policy template that you want to reference to grant access. For more information, see the [trusted profiles](/docs/secure-enterprise?topic=secure-enterprise-tp-template-create&interface=cli#add-access-tp-template) and [access groups](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create&interface=cli#add-access-ag-template) documentation.

## Creating a new version of an access policy template by using the CLI
{: #new-version-policy-cli}
{: cli}

As access requirements evolve for teams in your enterprise, you might need to create another version of a policy template to meet new needs. To create a new policy template version by using the CLI, complete the following steps:

1. Edit your JSON file to include any updates to the policy template definition. For more information about the attributes that you can use in your JSON file, see the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy-template)

1. Use the `access-policy-template-version-create` command as shown in the following sample request:

   ```bash
   ibmcloud iam access-policy-template-version-create SCCEditor --file /path/to/security_editor_access_policy_template.json
   ```
   {: codeblock}

## Removing policy template references by using the CLI
{: #remove-policy-templte-cli}
{: cli}

Removing the reference to a policy template from an access group or trusted profile template revokes the permissions that the policy grants to members. You can remove a policy template reference from an IAM template in a **Draft** state. However, you can't remove a policy template that's referenced in a committed or assigned trusted profile or access group template.

If you want to assign a different policy in a committed or assigned access group or trusted profile template, you must create a new version. For more information, see [Creating a new version of access group templates](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create&interface=ui#new-version-ag-template) and [trusted profile templates](/docs/secure-enterprise?topic=secure-enterprise-tp-template-create&interface=ui#new-version-tp-template).

You can remove a policy template reference for an access group or trusted profile template by using the CLI.

1. Get the ID of the policy template by using the `ap-templates` command as shown in the following sample request:

   ```bash
   example command
   ```
   {: codeblock}

1. Update the JSON file that configures the access group or trusted profile template definition and remove the reference to the policy template. For more information, see.

## Deleting a version by using the CLI
{: #delete-policy-template-cli}
{: cli}

You can delete a policy template that's referenced in a draft or committed trusted profile or access group template. However, the policy template reference becomes invalid and you must [remove](/docs/secure-enterprise?topic=secure-enterprise-policy-template-create&interface=cli#remove-policy-templte-cli) it before you can assign the trusted profile or access group template to child accounts.

In the case that you want to delete a policy template that's referenced in a trusted profile or access group template that's assigned to child accounts, you must remove the trusted profile or access group template assignment from those child accounts. For more information, see [Removing an assignment for access group templates](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create#remove-assignment-ag) and [trusted profile templates](/docs/secure-enterprise?topic=secure-enterprise-tp-template-create#remove-assignment-tp).


To delete a policy template version by using the CLI, use the `access-policy-template-version-delete` command as shown in the following sample request:

```bash
ibmcloud iam access-policy-template-version-delete SCCEditor 2
```
{: codeblock}

This deletes version `2` of the `SCCEditor` policy template.

## Creating a policy template by using the API
{: #create-policy-template-api}
{: api}

If there's a policy that multiple access group or trusted profile templates might require, create an access policy template.

To programmatically create an access policy template by using the API, call the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy-template) as shown in the following sample request:

```bash
curl -X POST 'https://iam.test.cloud.ibm.com/v1/policy_template' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -d '{
  "name": "IKSEditor",
  "description": "Grant Editor Role on SERVICE_NAME",
  "account_id": "ACCOUNT_ID",
  "policy": {
     "type": "acces",
     "description": "Grant Editor Role on SERVICE_NAMEe",
     "control": {
        "grant": {
        "roles": [
          {
            "role_id": "crn:v1:bluemix:public:iam::::role:Editor"
          }
        ],
      },
    },
    "resource": [
      {
        "attributes": [
          {
            "key": "serviceName",
            "operator": "stringEquals",
            "value": "$SERVICE_NAME"
          }
        ]
      }
    ],
    "rule" :{
      "key": "{{environment.attributes.day_of_week}}",
      "operator": "dayOfWeekAnyOf",
      "value": ["1+00:00", "2+00:00", "3+00:00", "4+00:00", "5+00:00"]
    },
    "pattern": "time-based-conditions:weekly:all-day"
}'
```
{: curl}
{: codeblock}

```go
policyRole := &iampolicymanagementv1.Roles{
  RoleID: core.StringPtr("crn:v1:bluemix:public:iam::::role:Viewer"),
}
v2PolicyGrant := &iampolicymanagementv1.Grant{
  Roles: []iampolicymanagementv1.Roles{*policyRole},
}
v2PolicyControl := &iampolicymanagementv1.Control{
  Grant: v2PolicyGrant,
}
serviceNameResourceAttribute := &iampolicymanagementv1.V2PolicyResourceAttribute{
  Key:      core.StringPtr("serviceName"),
  Operator: core.StringPtr("stringEquals"),
  Value:    core.StringPtr("iam-access-management"),
}
policyResource := &iampolicymanagementv1.V2PolicyResource{
  Attributes: []iampolicymanagementv1.V2PolicyResourceAttribute{
    *serviceNameResourceAttribute},
}

templatePolicyModel := &iampolicymanagementv1.TemplatePolicy{
  Type:        core.StringPtr("access"),
  Description: core.StringPtr("Test Template"),
  Resource:    policyResource,
  Control:     v2PolicyControl,
}

createPolicyTemplateOptions := iamPolicyManagementService.NewCreatePolicyTemplateOptions(
  examplePolicyTemplateName,
  exampleAccountID,
  templatePolicyModel,
)

policyTemplate, response, err := iamPolicyManagementService.CreatePolicyTemplate(createPolicyTemplateOptions)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(policyTemplate, "", "  ")
fmt.Println(string(b))
```
{: go}
{: codeblock}


## Updating a policy template version by using the API
{: #update-policy-template-api}
{: api}

You can update the roles, resources, name, and description for a policy template at any time before you commit it. To programmatically update policy template by using the API, call the [IAM Policy Management API](/apidocs/iam-policy-management#replace-policy-template) as shown in the following sample request:

```bash
curl -X PUT 'https://iam.test.cloud.ibm.com/v1/policy_templates/$TEMPLATE_ID/versions/$TEMPLATE_VERSION' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -H 'If-Match: $ETAG' -d '{
  "type": "access",
  "description": "Viewer role for for all instances of SERVICE_NAME in the account.",
  "subjects": [
    {
      "attributes": [
        {
          "name": "iam_id",
          "value": "$USER_ID"
        }
      ]
    }'
  ],
  "roles":[
    {
      "role_id": "crn:v1:bluemix:public:iam::::role:Viewer"
    }
  ],
  "resources":[
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "$ACCOUNT_ID"
        },
        {
          "name": "serviceName",
          "value": "$SERVICE_NAME"
        }
      ]
    }
  ]
}'
```
{: codeblock}
{: curl}

```go
v2PolicyGrant := &iampolicymanagementv1.Grant{
  Roles: []iampolicymanagementv1.Roles{
    {core.StringPtr("crn:v1:bluemix:public:iam::::role:Viewer")},
    {core.StringPtr("crn:v1:bluemix:public:iam::::role:Administrator")},
  },
}

v2PolicyControl := &iampolicymanagementv1.Control{
  Grant: v2PolicyGrant,
}
serviceNameResourceAttribute := &iampolicymanagementv1.V2PolicyResourceAttribute{
  Key:      core.StringPtr("serviceName"),
  Operator: core.StringPtr("stringEquals"),
  Value:    core.StringPtr("watson"),
}
policyResource := &iampolicymanagementv1.V2PolicyResource{
  Attributes: []iampolicymanagementv1.V2PolicyResourceAttribute{
    *serviceNameResourceAttribute},
}

templatePolicyModel := &iampolicymanagementv1.TemplatePolicy{
  Type:        core.StringPtr("access"),
  Description: core.StringPtr("Test Template v2"),
  Resource:    policyResource,
  Control:     v2PolicyControl,
}

replacePolicyTemplateOptions := iamPolicyManagementService.NewReplacePolicyTemplateOptions(
  examplePolicyTemplateID,
  examplePolicyTemplateVersion,
  examplePolicyTemplateETag,
  templatePolicyModel,
)

policyTemplate, response, err := iamPolicyManagementService.ReplacePolicyTemplate(replacePolicyTemplateOptions)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(policyTemplate, "", "  ")
fmt.Println(string(b))
```
{: go}
{: codeblock}

If you need to make updates after you commit the template, create a new version.
{: tip}

## Committing a policy template version
{: #commit-policy-version-api}
{: api}

Review the policy template and commit it so that no further changes can be made to the version. This way, the Template Assignment Administrator can be sure that they assign the version only when you have confirmed that it's ready. To programmatically commit a policy template by using the API, call the [IAM Policy Management API](/apidocs/iam-policy-management#commit-policy-template) as shown in the following sample request:

```bash
curl -X PUT 'https://iam.test.cloud.ibm.com/v1/policy_templates/$TEMPLATE_ID/versions/$TEMLPATE_VERSION/commit' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -H 'If-Match: $ETAG' -d '{}'
```
{: codeblock}
{: curl}

```go
commitPolicyTemplateOptions := iamPolicyManagementService.NewCommitPolicyTemplateOptions(
  examplePolicyTemplateID,
  examplePolicyTemplateVersion,
  examplePolicyTemplateETag,
)

response, err := iamPolicyManagementService.CommitPolicyTemplate(commitPolicyTemplateOptions)
if err != nil {
  panic(err)
}
if response.StatusCode != 204 {
  fmt.Printf("\nUnexpected response status code received from CommitPolicyTemplate(): %d\n", response.StatusCode)
}
```
{: go}
{: codeblock}

## Granting access with a policy template by using the API
{: #grant-access-template-api}
{: api}

Unlike regular access policies, an access policy template doesn't have an explicit subject that you're granting access to when you create it. To programmatically assign a policy template to an access group or trusted profile template by using the API, call the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy-template-assignment) as shown in the following sample request:

1. List all access policy templates and note the ID of the policy template in the response:

    ```bash
    curl -X GET 'https://iam.test.cloud.ibm.com/v1/policy_templates?account_id=$ACCOUNT_ID' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json'
    ```
    {: codeblock}
    {: curl}

1. Use the policy template ID to assign access in access group templates and trusted profile templates. For more information, see the [trusted profiles](/docs/secure-enterprise?topic=secure-enterprise-tp-template-create&interface=api#create-trusted-profile-template-api) and [access groups](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create&interface=api#add-access-ag-template-api) documentation.

## Creating a new policy template version by using the API
{: #new-version-policy-api}
{: api}

As access requirements evolve for teams in your enterprise, you might need to create another version of a policy template to meet new needs. To programmatically create a new policy template version by using the API, call the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy-template-version) as shown in the following sample request:

```bash
curl -X POST 'https://iam.test.cloud.ibm.com/v1/policy_template/$TEMPLATE_ID/versions' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -d '{
  "name": "IKSEditor",
  "description": "Grant Editor Role on SERVICE_NAME",
  "account_id": "ACCOUNT_ID",
  "policy": {
     "type": "acces",
     "description": "Grant Editor Role on SERVICE_NAMEe",
     "control": {
        "grant": {
        "roles": [
          {
            "role_id": "crn:v1:bluemix:public:iam::::role:Editor"
          }
        ],
      },
    },
    "resource": [
      {
        "attributes": [
          {
            "key": "serviceName",
            "operator": "stringEquals",
            "value": "$SERVICE_NAME"
          }
        ]
      }
    ],
    "rule" :{
      "key": "{{environment.attributes.day_of_week}}",
      "operator": "dayOfWeekAnyOf",
      "value": ["1+00:00", "2+00:00", "3+00:00", "4+00:00", "5+00:00"]
    },
    "pattern": "time-based-conditions:weekly:all-day"
}'
```
{: codeblock}
{: curl}

```go
v2PolicyGrant := &iampolicymanagementv1.Grant{
  Roles: []iampolicymanagementv1.Roles{
    {core.StringPtr("crn:v1:bluemix:public:iam::::role:Viewer")},
    {core.StringPtr("crn:v1:bluemix:public:iam::::role:Administrator")},
  },
}

v2PolicyControl := &iampolicymanagementv1.Control{
  Grant: v2PolicyGrant,
}
serviceNameResourceAttribute := &iampolicymanagementv1.V2PolicyResourceAttribute{
  Key:      core.StringPtr("serviceName"),
  Operator: core.StringPtr("stringEquals"),
  Value:    core.StringPtr("watson"),
}
policyResource := &iampolicymanagementv1.V2PolicyResource{
  Attributes: []iampolicymanagementv1.V2PolicyResourceAttribute{
    *serviceNameResourceAttribute},
}

templatePolicyModel := &iampolicymanagementv1.TemplatePolicy{
  Type:        core.StringPtr("access"),
  Description: core.StringPtr("Test Template v2"),
  Resource:    policyResource,
  Control:     v2PolicyControl,
}

createPolicyTemplateVersionOptions := iamPolicyManagementService.NewCreatePolicyTemplateVersionOptions(
  examplePolicyTemplateID,
  templatePolicyModel,
)

policyTemplate, response, err := iamPolicyManagementService.CreatePolicyTemplateVersion(createPolicyTemplateVersionOptions)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(policyTemplate, "", "  ")
fmt.Println(string(b))
```
{: go}
{: codeblock}

## List policy templates
{: #list-policy-templte-api}
{: api}

You might need to list the policy templates in your enterprise account for several reasons. You can see what policies are available or retrieve details about a policy that you want to edit, remove from another template, or delete.

To list the policy templates in your enterprise account, call the [IAM Policy Management API](/apidocs/iam-policy-management#list-policy-templates) as shown in the following sample request:

```bash
curl -X GET 'https://iam.test.cloud.ibm.com/v1/policy_templates?account_id=$ACCOUNT_ID' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json'
```
{: codeblock}
{: curl}

```go
listPolicyTemplatesOptions := iamPolicyManagementService.NewListPolicyTemplatesOptions(
  exampleAccountID,
)

policyTemplateCollection, response, err := iamPolicyManagementService.ListPolicyTemplates(listPolicyTemplatesOptions)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(policyTemplateCollection, "", "  ")
fmt.Println(string(b))
```
{: go}
{: codeblock}


## Removing policy template references by using the API
{: #remove-policy-templte-api}
{: api}

Removing the reference to a policy template from an access group or trusted profile template revokes the permissions that the policy grants to members. You can remove a policy template reference from an IAM template in a **Draft** state. However, you can't remove a policy template that's referenced in a committed or assigned trusted profile or access group template.

If you want to assign a different policy in a committed or assigned access group or trusted profile template, you must create a new version. For more information, see [Creating a new version of access group templates](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create&interface=ui#new-version-ag-template) and [trusted profile templates](/docs/secure-enterprise?topic=secure-enterprise-tp-template-create&interface=ui#new-version-tp-template).

To remove a policy template from access group or trusted profile templates, call the [IAM Policy Management API](/apidocs/iam-policy-management#delete-policy-assignment) as shown in the following sample request:

```bash
curl -X DELETE 'https://iam.test.cloud.ibm.com/v1/policy_assignments/$POLICY_ASSIGNMENT_ID' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json'
```
{: codeblock}
{: curl}

## Deleting a policy template by using the API
{: #delete-policy-template-api}
{: api}

Deleting a policy template deletes all version of the template. You can delete a policy template that's referenced in a draft or committed trusted profile or access group template. However, the policy template reference becomes invalid and you must [remove](/docs/secure-enterprise?topic=secure-enterprise-policy-template-create&interface=cli#remove-policy-templte-cli) it before you can assign the trusted profile or access group template to child accounts.

In the case that you want to delete a policy template that's referenced in a trusted profile or access group template that's assigned to child accounts, you must remove the trusted profile or access group template assignment from those child accounts. For more information, see [Removing an assignment for access group templates](/docs/secure-enterprise?topic=secure-enterprise-ag-template-create#remove-assignment-ag) and [trusted profile templates](/docs/secure-enterprise?topic=secure-enterprise-tp-template-create#remove-assignment-tp).

To delete a policy template, call the [IAM Policy Management API](/apidocs/iam-policy-management#delete-policy-template) as shown in the following sample request:


```bash
curl -X DELETE 'https://iam.test.cloud.ibm.com/v1/policy_templates/$TEMPLATE_ID/versions/$TEMPLATE_VERSION' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json'
```
{: codeblock}
{: curl}

```go
deletePolicyTemplateOptions := iamPolicyManagementService.NewDeletePolicyTemplateOptions(
  examplePolicyTemplateID,
)

response, err := iamPolicyManagementService.DeletePolicyTemplate(deletePolicyTemplateOptions)
if err != nil {
  panic(err)
}
if response.StatusCode != 204 {
  fmt.Printf("\nUnexpected response status code received from DeletePolicyTemplate(): %d\n", response.StatusCode)
}
```
{: go}
{: codeblock}

### Deleting a policy template version by using the API
{: #delete-policy-version-api}
{: api}

To delete only a specific version, call the [IAM Policy Management API](/apidocs/iam-policy-management#delete-policy-template-version) as shown in the following sample request:

```bash
curl -X DELETE 'https://iam.test.cloud.ibm.com/v1/policy_templates/$TEMPLATE_ID/versions/$TEMPLATE_VERSION' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json'
```
{: codeblock}
{: curl}

```go
deletePolicyTemplateVersionOptions := iamPolicyManagementService.NewDeletePolicyTemplateVersionOptions(
  examplePolicyTemplateID,
  examplePolicyTemplateVersion,
)

response, err := iamPolicyManagementService.DeletePolicyTemplateVersion(deletePolicyTemplateVersionOptions)
if err != nil {
  panic(err)
}
if response.StatusCode != 204 {
  fmt.Printf("\nUnexpected response status code received from DeletePolicyTemplateVersion(): %d\n", response.StatusCode)
}
```
{: go}
{: codeblock}

## Policy template limitations
{: #policy-template-limits}

You can create access policy templates without an explicit subject by using the CLI or API, but using the {{site.data.keyword.cloud_notm}} console to complete this task is not supported.

When you assign access to an access group template or a trusted profile template in the console, a policy template is automatically created based on the policy that you assign. Go to **Manage > Access (IAM) > Templates > Policies** to view the policy templates.
{: note}
