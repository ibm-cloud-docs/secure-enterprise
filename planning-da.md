---

copyright:
   years: 2024, 2025
lastupdated: "2025-04-30"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# How do I decide what kind of component to create?
{: #choose-plan-process}

How do you decide whether you should create a module, a deployable architecture, or stack deployable architectures together? Let's compare the differences and evaluate the use cases in the following sections to help you decide.
{: shortdesc}


## Comparing deployable architectures and modules
{: #compare-options}

The following table provides a comparison and quick summary of the key differences between modules and types of deployable architectures.

| Method | Scope | Coupling | Deployable | Author |
|----|-------|----------|------------|--------|
| Create a module | Narrow | Tight | No | Developer |
| Create a deployable architecture | Medium to broad | Tight | Yes | Developer |
| Stack deployable architectures | Broad | Loose | Yes | Anyone |
{: row-headers}
{: caption="Comparison of concepts" caption-side="bottom"}


The following table can help you decide whether to use a module, a deployable architecture, or stack deployable architectures together depending on your use case.

| Purpose | Recommended method | Notes |
|----|-------|----------|
| Accelerate coding automation | Use modules | Modules provide reusable, curated automation to get developers of deployable architectures coding faster. Modules are for developers, not consumers. |
| Ensure that the cloud is secure and compliant | Use a deployable architecture | Deployable architectures enforce security and compliance for an architecture. If a deployable architecture is too small in scope, it cannot enforce compliance. For example, a deployable architecture that deploys only a virtual server instance can't ensure network security. |
| Give users a choice with guardrails | Stack deployable architectures together | By stacking deployable architectures, you can swap deployable architectures or add on deployable architectures and give users more choices. Since deployable architectures enforce security and compliance, stacking helps ensure the overall solution remains compliant. Stacking deployable architectures is an excellent approach for things like selecting which database to use. |
| User-created solutions or architectures | Stack deployable architectures together | Stacking deployable architectures allow users to create and publish their own repeatable patterns that are still safe, as they are composed of secure and compliant deployable architectures. |
| Decoupled architecture components | Stack deployable architectures together | Deployable architectures can be independently developed and versioned, but then stacked together for deployment. |
| Simplified user experience | Use a deployable architecture | Deployable architectures can provide a small or simple list of inputs to the user even for large or complex architectures. Deployable architectures are simple to understand and deploy. In comparison, stacking deployable architectures is a little more complex as the deployable architectures are exposed. |
{: caption="Help me choose the component to use" caption-side="bottom"}

[{{site.data.keyword.cloud_notm}} projects](/docs/secure-enterprise?topic=secure-enterprise-understanding-projects) ensure that resources are deployed through deployable architectures from the catalog and operate within the security and compliance guardrails of the organization. They also ensure that these resources are kept up to date and do not drift.

## Dependencies for deployable architectures 
{: #dependencies-das}

Dependencies arise when resources that are provisioned by one deployable architecture are required by another. That is, the resources that one deployable architecture provisions are used during the deployment of another architecture, as illustrated in the following image.

![A visual representation of a deployable architecture with a dependency. Deployable architecture A outputs resources that are then used as inputs in deployable architecture B.](images/dependency-concept.svg "Deployable architecture dependencies."){: caption="Deployable architecture dependencies" caption-side="bottom"}

One way to work with dependencies is to [stack deployable architectures and add references between them in a project](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui). For example, the [VSI on VPC landing zone](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-vsi-ef663980-4c71-4fac-af4f-4a510a9bcf68-global?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2c%2Fc2VhcmNoPXZwYyUyNTIwbGFuZGluZyUyNTIwem9uZSUyNTIwd2l0aCUyNTIwcm9rcyUyNTIwbGFiZWwlMjUzQWRlcGxveWFibGVfYXJjaGl0ZWN0dXJlI3NlYXJjaF9yZXN1bHRz&kind=terraform&format=terraform&version=db410822-02be-41fe-ab3e-fa5a6a02f3da-global#prerequisites){: external} deployable architecture includes a variation that extends the [{{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone](https://cloud.ibm.com/catalog/architecture/deploy-arch-ibm-slz-ocp-95fccffc-ae3b-42df-b6d9-80be5914d852-global){: external} deployable architecture. Consider stacking these architectures together in a project. This approach works well if the prerequisite architecture isn't deployed yet. You also don't need to edit code to stack deployable architectures.

Many deployable architectures are stand-alone and aren't extensions of other architectures, but you can choose to extend some deployable architectures in the **Architecture** section of the catalog details page. Select an option from the **How do you want to build this architecture?** menu. 
{: tip}

However, if you already deployed {{site.data.keyword.redhat_openshift_notm}} Container Platform and you need to deploy VSI, the resources that are required for VSI are already provisioned. You don't need to deploy the {{site.data.keyword.redhat_openshift_notm}} Container Platform architecture again. Since the VSI architecture is an extension of the {{site.data.keyword.redhat_openshift_notm}} Container Platform, you can deploy VSI and the architecture uses the resources from the {{site.data.keyword.redhat_openshift_notm}} Container Platform as needed. 

### Optional and swappable deployable architectures
{: #optional-swappable}

This is an experimental feature that is available for evaluation and testing purposes and might change without notice.
{: experimental}

When you onboard a deployable architecture to a private catalog, you can extend it by stacking it with other architectures. By doing so, you can create a more customizable solution for your users. 

Why stack during onboarding? 
:   Stacking architectures during onboarding is similar to stacking architectures in a project. As you onboard an architecture, you can include dependencies by stacking required architectures along with it. However, unlike stacking architectures in a project, stacking architectures during onboarding includes the following capabilities: 
    - You can add optional architectures that are tailored for different use cases. 
    - You can add swappable architectures that users can choose between. 

Optional architectures
:   Maybe your architecture works well with another deployable architecture, but isn't needed to meet a dependency or satisfy compliance. You can add the deployable architecture as optional, and users can choose to include it when they add your deployable architecture to a project. For example, a monitoring architecture might be useful but not required. 

Swappable architectures 
:   Any architecture that you stack with your own during onboarding can be swappable with other architectures. Swappable architectures give users a choice between multiple options that provide the same functionality. For example, you can include two deployable architectures that create different databases, and the user can decide which database option they want to use with your architecture.

For more information, see [Extending a deployable architecture during onboarding](/docs/secure-enterprise?topic=secure-enterprise-extend-da).

Optional and swappable architectures can be added as you onboard a deployable architecture to a private catalog. Currently, stacking deployable architectures in a project does not support optional or swappable architectures.
{: important}

## Terraform versus Ansible
{: #terraform-ansible}

In {{site.data.keyword.cloud_notm}}, a deployable architecture must use Terraform to declare the inputs and outputs of the deployable architecture (the interface) as Ansible does not have a machine-readable interface definition. Otherwise, the author of a deployable architecture might use any combination of Ansible pre- or post-scripts and Terraform to run the work of the deployable architecture. So, how does a developer decide which technology to use and for what?

|    | Terraform | Ansible |
|----|-------|----------|
| Language | Declarative | Procedural |
| Syntax | HCL (similar to JSON) | YAML (and callouts to other scripts) |
| Default approach | Mutable infrastructure | Immutable infrastructure |
| Focus | Infrastructure | Configuration |
| Drift | Comparison with wanted state | Idempotent tasks |
{: row-headers}
{: caption="Comparison of configuration languages" caption-side="bottom"}
{: summary="The first column are categories used to compare Terraform and Ansible. The second column provides information about how Terraform relates to the set category in column one and the third column provides information about how Ansible relates to the set category in column one."}

Terraform is excellent at creating and managing infrastructure, whereas Ansible is excellent at configuring the software and operating systems that are running on that infrastructure. Because Ansible is procedural, you can also script one-time operations. Maintenance tasks, like restoring from backup, are easy in Ansible.

| Purpose| Recommended configuration language | Notes |
|----|-------|----------|
| Deploy cloud infrastructure or services | Terraform | Terraform targets this use case and is better at handling changing infrastructure. {{site.data.keyword.cloud_notm}} provides Terraform modules and supported deployable architectures to accelerate secure and compliant infrastructure patterns. The Terraform state model allows developers to preview the changes. Those changes can be scanned for compliance. |
| Install or configure software | Ansible | If a pre-built container or virtual machine image is not suitable for the use case, Ansible is better at handling the software installation and configuration. Ansible has extensive support for automated operations such as configuration edits, package management, and process restarts. A large library of Ansible modules and playbooks are available to ease the configuration of thousands of commonly used software packages. |
| CCDB integration | Ansible | Making a dynamic call to an on-premises service like a CCDB can be done in Terraform or Ansible but is easier to accomplish in a procedural or scripting language. |
| Input validation | Terraform or Ansible | Terraform has limited ability to do input validation, but it is declarative and can be used by user interfaces. Ansible adds the ability to do dynamic input validation where inputs to a deployable architecture are checked against a remote service. |
| Day 2 maintenance actions | Ansible | Day 2 maintenance is generally procedural in nature and best completed in Ansible. Examples include manual backup, restore, or key rotations. |
| Drift management | * Terraform \n * Ansible | * Terraform can be used to determine whether drift occurred and what that drift is, which can help you decide how to manage the drift. The drift information is powerful because it might indicate that you can add a change to automation. \n * Ansible can't detect drift. But by regularly reapplying an idempotent ansible script, you can prevent drift. |
{: caption="Help me choose the configuration language to use" caption-side="bottom"}

For more information about including pre- or post-scripts with your deployable architectures, see [Creating scripts for deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-understand-scripts).

## Next steps: Deciding where to publish
{: #decide-next-steps}

After you've planned out your architecture and decided what type of component to create, you should [consider where you plan to share or publish](/docs/secure-enterprise?topic=secure-enterprise-publish-da-options&interface=ui) your solution, so that other users can take advantage of the solution that you create. Depending on where you plan to share or publish, you might have different levels of requirements or approvals to complete.
