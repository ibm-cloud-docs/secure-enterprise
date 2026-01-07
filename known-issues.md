---

copyright:

  years: 2022, 2026

lastupdated: "2026-01-07"

keywords: known issues, known limitations

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Known issues and limitations
{: #known-issues}

Known issues and limitations include configuration management, user access to projects, and identity and access management (IAM) limits.
{: shortdesc}

To review the default IAM limits for your enterprise, see [Enterprise limits](/docs/enterprise-management?topic=enterprise-management-what-is-enterprise#enterprise-limit). To review the default limits for an account, see [{{site.data.keyword.cloud_notm}} IAM limits](/docs/account?topic=account-cloudaccess#iam_limits).
{: note}

## Authorization
{: #auth-known-issue}

To work in a project, users must have access to the {{site.data.keyword.cloud_notm}} Projects service, the resource group for the project, and {{site.data.keyword.bplong}}. For more information about access, see [Assigning users access to projects](/docs/secure-enterprise?topic=secure-enterprise-access-project).

Authorizing projects to deploy to a target account is managed by passing an API key into the deployable architecture. [Projects can be directly authorized by using a trusted profile](/docs/secure-enterprise?topic=secure-enterprise-tp-project), but some services such as {{site.data.keyword.containerlong_notm}} don't support trusted profiles. [API keys continue to be supported](/docs/secure-enterprise?topic=secure-enterprise-authorize-project), but a trusted profile is the preferred method for authorizing deployment to target accounts.

## Configuration management
{: #configuration-known-issue}

Configurations can be added, deleted, and renamed. Moving a configuration between projects must be done manually by editing `project.json` documents on both projects. If there are multiple configurations in a project, they can be organized only by naming conventions.

## Cost estimate
{: #cost-estimate-known-issue}

Cost estimation is available for deployable architectures in the {{site.data.keyword.cloud_notm}} catalog. Depending on the deployable architecture, a starting cost is estimated based on the available data. This estimated amount is subject to change as the architecture is customized within a project, and it does not include all resources, usage, licenses, fees, discounts, or taxes. For more information, see [Estimating architecture costs in a project](/docs/secure-enterprise?topic=secure-enterprise-cost-estimate-project).

{{../account/known-issues.md#policy-version-limit}}

## Drift detection limits
{: #drift-detection-known-issue}

Schematics and Terraform can detect a drift only between a changed resource and the specific configuration that created that resource during deployment. The service can’t detect drift in reused or referenced resources.

For example, in a specific scenario, configuration `config-1` created a Cloud Object Storage instance during deployment. Later, you deployed configurations `config-2` and `config-3`, and they reused the same Cloud Object Storage instance. When you renamed the resource, drift was detected only between `config-1` and the renamed Cloud Object Storage instance. `config-2` and `config-3` failed the drift detection job because drift can be detected only between a configuration and the resource that it created, not reused.

For more information, see [Managing drift](/docs/secure-enterprise?topic=secure-enterprise-manage-drift).
