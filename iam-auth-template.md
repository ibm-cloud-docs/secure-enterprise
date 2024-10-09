---
copyright:
  years: 2024
lastupdated: "2024-09-23"

keywords: enterprise, enterprise account, multiple accounts, enterprise access, policy templates, enterprise managed, policies, enterprise policy, template

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Creating enterprise-managed authorization policy templates
{: #authorization-policy-template-create}

Create predefined authorization policies for your enterprise that enable a source service to access a target service based on a set of assigned roles. In an authorization, the source service is the service that is granted access to the target service. A source service can be in the same account where the authorization is created or in another account. The target service is the service that you are granting permission to be accessed by the source service based on the roles that you assign. The target service is always in the account where the authorization is created.
{: shortdesc}

| Source              | Target          | Description |
|---------------------|-----------------|---------------------|
| This account        | Child account   | Service in the enterprise account gets access to a service in the child account where the template is assigned. |
| Assigned account(s) | Child account   | Service in the assigned child account gets access to another service in the same child account. |
| Specific account    | Child account   | Service in the specified child account gets access to a service in the child account where the template is assigned. |
{: caption="Example authorization policy templates." caption-side="top"}

Review the following scenarios to understand how authorization policy templates simplify the creation and management of many authorization policies:

1. Replicating authorization relationships in various child accounts in an enterprise.

   - For example, an enterprise that uses the service "{{site.data.keyword.filestorage_vpc_short}}" might want to establish a disaster recovery pattern by enabling cross-region replication of critical data. To enable data replication, you must create an authorization policy template and assign it in each child account. The authorization policy grants the file service in one VPC access to interact with the file service of another VPC located in a different geographical region in the same account. You can create an authorization policy template and specify "{{site.data.keyword.filestorage_vpc_short}}" for both the source and target. For a detailed example of this authorization policy template, review the examples in this topic. For more information, see [Establishing service-to-service authorizations for {{site.data.keyword.filestorage_vpc_short}}](/docs/vpc?topic=vpc-file-s2s-auth&interface=ui).

2. Allowing a service instance in an enterprise account to access resources in many child accounts.

   - For example, an enterprise might want a centralized data store, where the volumes are encrypted by using encryption keys that reside in child accounts. You can create an authorization policy template to define a service-to-service authorization between the source service, "Block Storage" service in an enterprise account, and the target service, "Key Management Service" in a child account. When you assign this template to child accounts, authorization policies are automatically created in each child accounts that grant the "Block Storage" service access to the "Key Management Service".

## Creating an authorization policy template in the console
{: ui}
{: #create-authorization-policy-template-ui}

To create an authorization policy template, complete the following steps:

1. Go to **Manage > Access (IAM) > Templates** in the console.
1. Select **Authorizations** and click **Create**.
1. Enter a name and description for the authorization template that describes its purpose for enterprise users.
1. Enter a description for the enterprise-managed authorization policy that describes its purpose for child account users.
1. Click **Create**.

Next complete the following steps to build the authorization rules:

1. Go to **Authorization** to specify the details of the authorization policy.
1. Select the account from which the source service requests access to another service. For this example select Assigned account(s), but there are a few options to consider:
    1. **This account**: The single, root enterprise account
    1. **Assigned account(s)**: When you assign the authorization template to a child account, the source account is populated to the same account as the target child account, which holds the resource being accessed.
    1. **Specific account**: A single specific account within the enterprise.

    The target is always in the child account where the template is assigned. The source can be the same child account as the target (Assigned account), the root enterprise account (This account) or a specific account in the enterprise (Specific account).
    {: note}

1. Next you select the source service and resources. For this example, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. From the list, select **Resource type**.
   1. In the next field, select **{{site.data.keyword.filestorage_vpc_short}}**.
   1. Click **Next**.
1. For the target service, select **VPC Infrastructure Services** from the list. Click **Next**.
   1. Select the scope by clicking **Specific resources**.
   1. Click **Select an attribute**.
   1. From the list, select **Resource type**.
   1. In the next field, select **{{site.data.keyword.filestorage_vpc_short}}**.
   1. Click **Next**.
1. Select the role `Editor`.
1. Click **Review** and **Save**.
1. The template is now ready to commit and assign to child accounts.

This example creates authorization policies that help enable data replication in various child accounts where the template is assigned.

Templates are valuable only if they are generic enough to apply to multiple accounts. The previous example specifies {{site.data.keyword.filestorage_vpc_short}} for both the source and target, but does not include specific instance IDs because those are specific to the resources in each child account.
{: note}

## Deleting an authorization policy template in the console
{: #delete-authorization-policy-template-ui}
{: ui}

Deleting an authorization policy template deletes all version of the template. You can delete an authorization policy template that's referenced in a **Draft** or **Committed** state. You cannot delete authorization policy templates that are in **Assigned** state, you must unassign it first.

To delete a policy template in the console, complete the following steps:

1. Go to **Manage > Access (IAM) > Templates** in the {{site.data.keyword.cloud_notm}}.
1. Click **Authorizations**.
1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Delete template**.
1. Confirm that you want to delete the template.

## Deleting an authorization policy template version in the console
{: #delete-authorization-policy-template-version-ui}
{: ui}

To delete only a specific version, complete the following steps:

1. Go to **Manage > Access (IAM) > Templates** in the {{site.data.keyword.cloud_notm}}.
1. Click **Authorizations**.
1. Click the **Actions** icon ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Delete version**.
1. Confirm that you want to delete the version.


## Create an authorization policy template by using the CLI
{: #create-authorization-policy-template-cli}
{: cli}

To create an authorization policy template, complete the following steps in the CLI:

1. Create a JSON file that configures the definition of the authorization policy template. For more information about the attributes that you can use in your JSON file, see the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy-template).

   The following example JSON file specifies the `name` and `description` of the template and the `account_id` of the enterprise account. Then, the `authorization policy` definition is defined. This authorization template grants an Editor role between all "{{site.data.keyword.filestorage_vpc_short}}" in the assigned accounts.

    ```json
        {
          "name": "VPC_REGIONAL_REPLICATOR",
          "description": "Grant Editor Role between VPC File Storage to enable regional replication",
          "account_id": "ENTERPRISE_ROOT_ACCOUNT_ID",
          "policy": {
            "type": "authorization",
            "description": "Grant Editor on VPC File Storage",
            "control": {
                "grant": {
                "roles": [
                  {
                    "role_id": "crn:v1:bluemix:public:iam::::role:Editor"
                  }
                ]
              }
            },
            "subject":
              {
                "attributes": [
                  {
                    "key": "serviceName",
                    "operator": "stringEquals",
                    "value": "is"
                  },
                  {
                    "key": "resourceType",
                    "operator": "stringEquals",
                    "value": "share"
                  }
                ]
              }
            ,
            "resource":
              {
                "attributes": [
                  {
                    "key": "serviceName",
                    "operator": "stringEquals",
                    "value": "is"
                  },
                  {
                    "key": "resourceType",
                    "operator": "stringEquals",
                    "value": "share"
                  }
                ]
              }
          }
    }
    ```
    {: codeblock}

1. Use the `authorization-policy-template-create` command as shown in the following sample request:

```bash
 ibmcloud iam authorization-policy-template-create --file /path/to/vpc-share-authorization-template.json
```
{: codeblock}

## Updating an authorization policy template by using the CLI
{: #update-authorization-policy-template-cli}
{: cli}

You can update the roles, target, source, name, and description for an authorization template at any time before you commit it.

To update an authorization template by using the CLI, complete the following steps:

1. Update your JSON file with the new authorization template definition. For more information about the attributes that you can use in your JSON file, see the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy-template).

   When you update the template name, you update the name for every version.
   {: note}

1. Use the `authorization-policy-template-version-update` command as shown in the following sample request:

   ```bash
    iam  authorization-policy-template-version-update VPC_REGIONAL_REPLICATOR 1 --file /path/to/vpc-share-authorization-template-updated.json
   ```
   {: codeblock}

   This updates version `1` of the `VPC_REGIONAL_REPLICATOR` authorization template by referring to the updated JSON file.

     When you update an existing template, remove the "account_id"
   {: note}

If you need to make updates after you commit the template, create a new version.
{: tip}

## Committing an authorization template version by using the CLI
{: #commit-authorization-policy-version-cli}
{: cli}

Review the authorization template and commit it so that no further changes can be made to the version. This way, the Template Assignment Administrator can be sure that they assign the version only when you confirm that it's ready.

1. Review the authorization policy template before you commit it by using the `authorization-policy-template-version` method to view the specific version.

   ```bash
   ibmcloud iam authorization-policy-template-version VPC_REGIONAL_REPLICATOR 1
   ```
   {: codeblock}

1. To commit an authorization template by using the CLI, use the `authorization-policy-template-version-commit` method as shown in the following sample request:

   ```bash
   ibmcloud iam authorization-policy-template-version-commit  VPC_REGIONAL_REPLICATOR 1
   ```
   {: codeblock}


## Assigning access with an authorization template by using the CLI
{: #assign-authorization-policy-template-cli}
{: cli}

Assign an authorization template by using the CLI to an account within the enterprise to create the underlying authorization policies in that account.

1. Create an authorization template by using the `authorization-policy-assignment-create` command as shown in the following sample request:

   ```bash
    iam authorization-policy-assignment-create  VPC_REGIONAL_REPLICATOR 1  --target-type Account --target <<ACCOUNT_ID>>
   ```
   {: codeblock}

1. Use the `authorization-policy-assignment` command to see or refresh the status of the assignment as shown:

    ```bash

     iam authorization-policy-assignment <<POLICY_ASSIGNMENT_GUID>>
    ```
   {: codeblock}


It might take a few seconds for the assignment to create the underlying resources and display the appropriate status.
{: note}


## Unassigning an authorization template by using the CLI
{: #unassign-authorization-policy-template-cli}
{: cli}

Unassigning an authorization policy assignment removes all the underlying resources that assignment created within the child accounts. To unassign an authorization template by using the CLI, complete the following command:

1. Use the `authorization-policy-assignment-delete` command to run an unassignment against a particular authorization policy assignment.
    ```bash

     iam authorization-policy-assignment-delete <<POLICY_ASSIGNMENT_GUID>>
    ```
   {: codeblock}

## Creating a version of an access policy template by using the CLI
{: #new-version-authorization-policy-cli}
{: cli}

As access requirements evolve for teams in your enterprise, you might need to create another version of an authorization template to meet new needs. To create a new authorization template version by using the CLI, complete the following steps:

1. Edit your JSON file to include any updates to the authorization template definition. For more information about the attributes that you can use in your JSON file, see the [IAM Policy Management API](/apidocs/iam-policy-management#create-policy-template)

1. Use the `authorization-policy-template-version-create` command as shown in the following sample request:

   ```bash
   ibmcloud iam authorization-policy-template-version-create VPC_REGIONAL_REPLICATOR --file /path/to/vpc-share-authorization-template-updated_v2.json
   ```
   {: codeblock}


## Deleting a version of an authorization policy template by using the CLI
{: #delete-authorization-policy-template-cli}
{: cli}

You can delete a particular version of an authorization policy template that's referenced in a **Draft** or **Committed** state. You cannot delete authorization policy templates that are in **Assigned** state, you must unassign it first.

To delete a version of an authorization policy template in the CLI, complete the following steps:

1. Get the ID and version of the authorization policy template by using the `authorization-policy-template` command as shown in the following sample request:

   ```bash
   ibmcloud iam authorization-policy-templates
   ```
   {: codeblock}


1. Delete the particular version by using the `authorization-policy-template-version-delete` command as shown in the following sample request:

   ```bash
   ibmcloud iam authorization-policy-template-version-delete VPC_REGIONAL_REPLICATOR 2
   ```
   {: codeblock}

This example deletes version `2` of the `VPC_REGIONAL_REPLICATOR` policy template.
