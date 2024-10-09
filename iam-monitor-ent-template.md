---

copyright:
  years: 2023, 2024
lastupdated: "2024-10-09"

keywords: monitoring, iam templates, monitoring iam templates, activity tracker

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring enterprise IAM templates
{: #monitor-enterprise-iam-templates}

Enterprise IAM templates are used to centrally administer IAM access and account settings throughout an enterprise. You can use {{site.data.keyword.at_full}} to monitor IAM template assignments to child accounts, versioning, and changes that child account administrators make to enterprise-managed IAM resources in their account.
{: shortdesc}

## Before you begin
{: #before-monitor-iam-template}

You must create an instance of the {{site.data.keyword.cloudaccesstrailshort}} service in the Frankfurt (eu-de) region to monitor events for enterprise IAM templates. For more information, see [Provisioning an instance](/docs/activity-tracker?topic=activity-tracker-provision).

## Reviewing IAM template events by using {{site.data.keyword.cloudaccesstrailshort}}
{: #monitor-iam-template-assignment}

When you assign an enterprise-managed IAM template to target child accounts, multiple events generate. The enterprise account and each target child account have their own corresponding events.

### Analyzing template create events in the enterprise account
{: #ent-create-assignments}

Complete the following steps to review the creation of IAM templates in an enterprise account:

1. In the {{site.data.keyword.cloud_notm}} console, go to the **Navigation Menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **Observability** > **{{site.data.keyword.cloudaccesstrailshort}}**.
1. Click **Open dashboard** on the dashboard that you use to monitor enterprise IAM templates.
1. Click **Sources** and select the IAM service name to filter the results and view only events for the type of template that you're looking for.
   - Access group templates: `iam-groups`
   - Trusted profile templates: `iam-identity`
   - Settings templates: `iam-identity`
   - Policy templates: `iam-access-management`
1. Search  for the `template.create` action.
   1. For access group templates, search for `action:iam-groups.group-template.create`
   1. For trusted profile templates, search for `action:iam-identity.profile-template.create`
   1. For settingstemplates, search for `action:iam-identity.account-settings-template.create`
   1. For access group templates, search for  `action:iam-access-management.policy-template.create`

Use the search field to view new version events by adding `responseData.version=={{version number}}` to your search. For example, `action:iam-identity.profile-template.create responseData.version=={{version number}}`.
{: tip}

### Analyzing assignment events in the enterprise account
{: #ent-account-assignments}

Complete the following steps to review IAM template assignments from the enterprise account to child accounts:

1. In the {{site.data.keyword.cloud_notm}} console, go to the **Navigation Menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **Observability** > **{{site.data.keyword.cloudaccesstrailshort}}**.
1. Click **Open dashboard** on the dashboard that you use to monitor enterprise IAM templates.
1. Click **Sources** and select the IAM service name to filter the results and view only events for the type of template that you're looking for.
   - Access group templates: `iam-groups`
   - Trusted profile templates: `iam-identity`
   - Settings templates: `iam-identity`
   - Policy templates: `iam-access-management`
1. Use the search field to view IAM templates events.
   1. For access group templates, search for `action:iam-groups.groups-assignment.create`
   1. For trusted profile templates, search for `action:iam-identity.profile-assignment.create`
   1. For settings templates, search for `action:iam-identity.account-settings-assignment.create`
   1. For access policy templates, search for `action:iam-access-management.policy-assignment.create`
1. Select an event to view the fields that have identifying information.

To search for failed assignments, add `outcome==failure` to your search. For example, `action:iam-groups.groups-assignment.create outcome==failure`.
{: tip}

#### Analyzing changes to enterprise-managed IAM resources in child accounts
{: #changes-ent-iam}

You can set action controls in access group template to enable administrators in chid accounts to change certain attributes of an enterprise-managed group in their account. For example, you can set the action control for adding policies to true. This allows access group administrators in child accounts to add policies the group assigned to their account.

You can stream child account events to the {{site.data.keyword.cloudaccesstrailshort}} instance in your enterprise account to monitor how child account administrators modify the enterprise-managed IAM resources in their account. For more information, see [Configuring streaming to an Activity Tracker instance through the UI](/docs/activity-tracker?topic=activity-tracker-streaming-configure-l2l).

Complete the following steps to review how child account administrators are modifying the enterprise-managed IAM resources in their account:

1. In the {{site.data.keyword.cloud_notm}} console, go to the **Navigation Menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **Observability** > **{{site.data.keyword.cloudaccesstrailshort}}**.
1. Click **Open dashboard** on the dashboard that you use to monitor enterprise IAM templates.
1. Click **Sources** and select the IAM service name to filter the results and view only events for the type of template that you're looking for.
   - Access group templates: `iam-groups`
   - Trusted profile templates: `iam-identity`
   - Settings templates: `iam-identity`
   - Policy templates: `iam-access-management`
1. Use the search field to view IAM templates events by searching for the `action` and `template_id`. For example,  `action:iam-groups.update template_id=={template_id}`.

### Analyzing events in child accounts
{: #account-review}

All enterprise-managed IAM resources that are created in child accounts from an enterprise assignment contain request data that maps the resource to an enterprise IAM template.

The request data includes the following attributes and example values:

```bash
"template_version": "1",
"template_id": "706f6c69637954656d706c6174652" ,
"assignment_id": "76450570-1ef3-41d2-ab24-935966032f93"
```
{: codeblock}

Complete the following steps to review IAM template assignments in your child account:

1. In the {{site.data.keyword.cloud_notm}} console, go to the **Navigation Menu** icon ![Navigation Menu icon](../icons/icon_hamburger.svg "Menu") > **Observability** > **{{site.data.keyword.cloudaccesstrailshort}}**.
1. Click **Open dashboard** on the dashboard that you use to monitor IAM events.
1. Click **Sources** and  select the IAM service name to filter the results and view only events for the type of template that you're looking for.
   - Access group templates: `iam-groups`
   - Trusted profile templates: `iam-identity`
   - Settings templates: `iam-identity`
   - Policy templates: `iam-access-management`
1. Use the search field to view enterprise-managed IAM events by searching for `template`.

## Examples
{: #monitor-ent-iam-example}

### Policy template creation
{: #ex-policy-assign}

An access policy template that is named `SecretsEditor` is created and has the following identifying attributes:

| Attribute    | Value      | Description |
|--------------|-----------|------------|
| `outcome` | `success` | The outcome of the request. |
| `action` | `iam-access-management.policy-template.create` | The action that triggers the event. |
| `initiator ID` | `IBMid-779000XKG2` | The unique ID of the requester.|
| `initiator name` | `user@example.com` | The email of the requester.|
| `tempalate_id` | `policyTemplate-b4fcbb84-9761-4b21-8aea-f78692c34641` | The policy template ID. |
| `name` | `SecretsEditor` | The name of the policy. |
| `account_id` |   `b04d9d6240ac4071nnf9152bf46bd94e` | The enterprise account ID. |
| `policy` | See [Example `policy`](/docs/secure-enterprise?topic=secure-enterprise-monitor-enterprise-iam-templates) | The structure of the policy. The type `access` gratns access to users, and `authorization` grants access between services. |
| `version` | `1` | The version of the policy template. |
| `description` | `Grant Editor Role on Secrets Manager service.` | The description of the policy. |
| `transaction_id` | `99450571-2df3-51c2-tn24-565813689t91` | Used for debug interactions and tracing the request with support. |
| `errors` | `{  "code": "insufficent_permissions",  "message": "You are not allowed to create the requested policy template."}` | If the `outcome` is `failure`, an error message is shown. |
{: caption="Sample policy template attributes and values in Activity Tracker" caption-side="top"}

#### Example `policy`
{: #example-policy-structure}

```json
"policy": "{  \"type\": \"access\",  \"control\": {    \"grant\": {      \"roles\": [        {          \"role_id\": \"crn:v1:bluemix:public:iam::::role:Editor\"        }      ]    }  },  \"resource\": {    \"attributes\": [      {        \"value\": \"hyperp-dbaas-mongodb\",        \"operator\": \"stringEquals\",        \"key\": \"serviceName\"      }    ]  }}"
```

### Access group template creation
{: #ex-ag-create}

An access group template that is named `dev-group-1` hasn't been committed yet:

| Attribute    | Value      | Description |
|--------------|-----------|------------|
| `action` | `iam-groups.group-template.create` | The action that triggers the event. |
| `outcome` | `success` | The outcome of the request. |
| `correlationId` | `76450570-1ef3-41d2-ab24-935966032f9` | Used for debug interactions and tracing the request with support. |
| `initiator ID` | `IBMid-779000XKG2` | The unique ID of the requester.|
| `initiator name` | `user@example.com` | The email of the requester.|
| `account_id` | `b04d9d6240ac4071nnf9152bf46bd94e` | The enterprise account ID. |
| `committed` | `false` | Committed flag determines if the template is ready for assignment. |
| `description` | `Dev group description` | The description of the policy. |
| `id` | `AccessGroupTemplateId-2324f8a0-3f87-42c2-a678-6595b0a59b55` | The access group template ID. |
| `name` | `dev-group-1` | The name of the access group template shown in the enterprise account. |
| `version` | `1` | The version of the policy template. |
| `group` | `action_controls`, `assertions`, `description`, `members`, `name`| The structure of the access group. |
| `action_controls` | See [Example `group`](/docs/secure-enterprise?topic=secure-enterprise-monitor-enterprise-iam-templates) | Determines whether access group administrators in child accounts can add policies to the enterprise-managed group in their account. |
| `assertions` | See [Example `group`](/docs/secure-enterprise?topic=secure-enterprise-monitor-enterprise-iam-templates) | Includes dynamic rules and the action controls that determines whether access group administrators in child accounts can add, remove, and update dynamic rules to the enterprise-managed group in their account. |
| `description` | `Access group description` | The access group description that is shown in child accounts. |
| `members` | See [Example `group`](/docs/secure-enterprise?topic=secure-enterprise-monitor-enterprise-iam-templates) | The enterprise members and service IDs to add to the access groups in child accounts. This also includes the action controls that determine whether access group administrators in child accounts can add and remove members in the enterprise-managed group in their account. |
| `name` | `groupName 1690274020835 - VPC-Staging` | The acces group name shown in child accounts. |
| `policy_template_references` | See Example `policy_template_references` | Existing policy templates that you can reference to assign access in the trusted profile template. |
| `errors` | `"code": "template_conflict_error", "message": "An access group template with the name "AccessGroup" already exists. Enter a different name."` | If the `outcome` is `failure`, an error message is shown. |
{: caption="Sample access group template creattion attributes and values in Activity Tracker" caption-side="top"}

#### Example `group`
{: #example-groups-structure}

```json
    "group": {
      "action_controls": "{  "access": {    "add": true  }}",
      "assertions": "{  "action_controls": {    "add": true,    "remove": false,    "update": false  },  "rules": [    {      "action_controls": {        "remove": false,        "update": false      },      "conditions": [        {          "claim": "blueGroup",          "operator": "CONTAINS",          "value": "\\"test-bluegroup-saml\\""        }      ],      "expiration": 12,      "name": "Manager group rule",      "realm_name": "https://idp.example.org/SAML2"    }  ]}",
      "description": "Access group description",
      "members": "{  "action_controls": {    "add": true,    "remove": false  },  "services": [    "ServiceId-ad7d5c39-da76-4145-aa55-ffb87aa81715",    "ServiceId-d9ddc9ca-9c4c-4405-839f-43a25fdefd0e"  ],  "users": [    "IBMid-50PJGPKYJJ",    "IBMid-665000T8WY"  ]}",
      "name": "groupName 1690274020835 - VPC-Staging"
    }
```
{: codeblock}

#### Example `policy_template_references`
{: #example-policy-ref}

```json
"policy_template_references": [
      {
        "id": "policyTemplate-2c8998aa-a706-4e07-bc35-6f23c9753c51",
        "version": "1"
      },
      {
        "id": "policyTemplate-998f1122-aca4-4094-a6dd-b14010d487bf",
        "version": "1"
      }
    ]
```
{: codeblock}

### Access group template assignment
{: #ex-ag-assign}

An access group template named `dev-group-1` that's assigned to child accouts has the following identifying attributes:

| Attribute    | Value      | Description |
|--------------|-----------|------------|
| `action` | `iam-groups.group-assignment.create` | The action that triggers the event. |
| `outcome` | `success` | The outcome of the request. |
| `correlationId` | `6dd6adda-42c2-45ce-9718-373b5cdf1923` | Used for debug interactions and tracing the request with support. The assignment event in child accounts have the same assignmentId. |
| `initiator ID` | `IBMid-999200LPO7` | The unique ID of the requester.|
| `initiator name` | `user@example.com` | The email of the requester.|
| `AssignmentId` | `AccessGroupAssignmentId-AccessGroupTemplateId-274f883e-fe00-4445-8e75-1698a7c76918-Target-a5d1c4130c62404dbc857a37237c49bf` | The assignment ID. Once groups are created in the target accounts, individual events in each account referencing the assignment are created.|
| `TemplateName` | `dev-group-1` |
| `TemplateVersion` | `2` | Version number of the template assigned. |
| `account_id` | `8f27b893b8ce49e19e748bf654ab999e` | Enterprise account ID. |
| `operation` | `assign` | The operation that maps to the action. |
| `status` | `succeeded` | Status of the assignment. |
| `target` | `a5d1c4130c62404dbc857a37237c49bf` | The ID of the target account or account group. |
| `target_type` | `AccountGroup` | The target can be either an `Account` or an `AccountGroup`. |
| `template_id` | `AccessGroupTemplateId-274f883e-fe00-4445-8e75-1698a7c76918` | ID of the access group template assigned. |
{: caption="Sample access group template assignment attributes and values in Activity Tracker" caption-side="top"}

### Trusted profile template creation
{: #tp-template-event}

| Attribute    | Value      | Description |
|--------------|-----------|------------|
| `action` | `iam-identity.profile-template.create` | The action that triggers the event. |
| `outcome` | `failure` | The outcome of the request. |
| `correlationId` | `bGxxcmw-06b47d9c53564bb1984dded8b3fdb455`| Used for debug interactions and tracing the request with support. |
| `initiator ID` | `IBMid-999200LPO7` | The unique ID of the requester.|
| `initiator name` | `user@example.com` | The email of the requester.|
| `realm_id` | `IBMid` | The identity provider. |
| `template_committed` | `false` | Committed flag determines if the template is ready for assignment. |
{: caption="Sample trusted profile template attributes and values in Activity Tracker" caption-side="top"}

### Trusted profile template assignment
{: #tp-template-assignment-event}

| Attribute    | Value      | Description |
|--------------|-----------|------------|
| `action` | `iam-identity.profile-assignment.create` | The action that triggers the event. |
| `outcome` | `success` | The outcome of the request. |
| `correlationId` | `dHptOTc-574744cfcb474f67b9299082fcd2b1ff`| Used for debug interactions and tracing the request with support. The assignment event in child accounts have the same assignmentId. |
| `initiator ID` | `IBMid-999200LPO7` | The unique ID of the requester.|
| `initiator name` | `user@example.com` | The email of the requester.|
| `realm_id` | `IBMid` | The identity provider. |
| `bss_account` | `86a1004d3f1848a291de32874cb48120` | The enterprise account ID. |
| `url` | `https://iam.cloud.ibm.com/v1/profile_assignments/TemplateAssignment-691487d2-a249-4475-b976-a7bf655ef3be` | URL to look up the assigned child accounts. See [Example `url` curl request](/docs/secure-enterprise?topic=secure-enterprise-monitor-enterprise-iam-templates) |
| `template_id` | `ProfileTemplate-73e6bad8-0815-44f7-b086-0cac24b0d3a7` | ID of the trusted profile template assigned. |
| `template_type` | `profile` | The type of IAM template. |
| `template_version` | `1` | The version number of the trusted profile template. |
| `instance_id` | `TemplateAssignment-691487d2-a249-4475-b976-a7bf655ef3be` | The assignment ID. Once profiles are created in the target accounts, individual events in each account referencing the assignment are created. |
{: caption="Sample trusted profile assignment attributes and values in Activity Tracker" caption-side="top"}

#### Example `url` curl request
{: #url-example}

The following example request uses the URL to get the assigned child accounts.

```bash
curl -H "Authorization: Bearer $TOKEN" "https://iam.test.cloud.ibm.com/v1/profile_assignments/TemplateAssignment-691487d2-a249-4475-b976-a7bf655ef3be"
```
{: codeblock}


## State of an assignment
{: #assignment-state}

| Attribute     | Description |
|---------------|-----------|
| `accepted`    | The assignment record is accepted and the assignment will be processed. |
| `in progress` | The assignment records were successfully created and the assignment process is in progress. |
| `success`     | The assignment process has completed and the template has been assigned to all target accounts and account groups successfully. |
| `fail`        | The assignment process is complete but there is one or more target accounts where the template failed to be assigned. |
| `superseded`  | The assignment has been superseded by another assignment record with the same template at a target account group higher in the enterprise accounts hierarchy. For more information, see [Superseeding a version](/docs/secure-enterprise?topic=secure-enterprise-working-with-versions#template-superseded). |
{: caption="The possible states for an IAM template assignment in Activity Tracker" caption-side="top"}
