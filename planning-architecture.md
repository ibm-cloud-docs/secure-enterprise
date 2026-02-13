---
copyright:
  years: 2024, 2026
lastupdated: "2026-02-13"

keywords: creating an architecture, planning for a deployable architecture

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Planning and researching for designing an architecture
{: #starting-da-process}

When you're planning to create a module, deployable architecture, or deployable architecture stack, you need to research and come up with a plan for designing the architecture before creating the automation. Evaluating what exists and how your proposed solution fits into the current ecosystem of offerings, thoroughly planning the architecture around your business requirements, and determining what you plan to build and where you will share or publish the solution are all key parts of the planning phase.
{: shortdesc}

## Researching existing offerings and user needs
{: #research-da}

As part of the research phase you need to understand what's currently available and what business use cases and requirements you plan to solve.

### Researching current offerings
{: #existing-offerings}

Evaluate the currently available modules and deployable architectures from {{site.data.keyword.cloud}} to determine whether other similar solutions exist or how much automation your team needs to develop. For example, if the existing architectures in the catalog can help build the solution you're planning, you might have to build only a single deployable architecture that you group into a stack with other existing deployable architectures to build the full solution. So, instead of building all of the pieces yourself, you can use existing deployable architectures. Similarly, if you're building a deployable architecture that is composed of modules, you can review the existing modules to identify any that you might want to use in your architecture.

You can explore existing modules and deployable architectures in the following locations:

- [`terraform-ibm-modules` public GitHub org](https://github.com/terraform-ibm-modules/){: external} is the {{site.data.keyword.cloud_notm}} Terraform modules project. This collection of Terraform modules follows best practices and simplifies the provisioning of {{site.data.keyword.cloud_notm}} services and instances.
- [{{site.data.keyword.cloud_notm}} module registry](https://cloud.ibm.com/catalog?catalog=2){: external} is a collection of assets that is separate from the {{site.data.keyword.cloud_notm}} catalog and is governed and maintained by the process in the `terraform-ibm-modules` repository. Discover public modules that have been confirmed to work with deployable architectures for your customization and building needs.
- [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog#reference_architecture){: external} includes generally available deployable architectures that are covered by the {{site.data.keyword.cloud_notm}} terms and conditions.
- [Community registry](https://cloud.ibm.com/catalog?catalog=community-registry){: external} includes a collection of real world examples of coded industry solutions to jumpstart your building needs. This collection is maintained based on specifications in the originating Github repository that might change frequently or be discontinued at short notice.

### Determining a viable reusable architecture pattern
{: #choosing-a-pattern}

The second part of the research phase is to understand the business requirements and use cases that the solution will be built to address and how you can build a viable, reusable pattern. Many of the existing architectures are based on customer use cases for digital transformation, application modernization, and more. Knowing which use cases you're meeting will help determine which technologies you choose to build your architecture.

Use the [Well-Architected Framework](https://www.ibm.com/think/architectures/well-architected){: external} and the Architecture Design Framework to plan and design the required components for the architecture. Follow the steps for [Designing a cloud solution by using the Architecture Design Framework](/docs/architecture-framework?topic=architecture-framework-create-solution). The decisions that you make during this process for the requirements and components that will be used to build your architecture pattern will be the design for your development team to build the pattern.

## Designing the architecture
{: #design-architecture}

Based on the decisions reached during the research phase, many teams start documenting the pattern by using a Word document where they can track changes and provide comments to collaborate on what the team plans to build.

As the plan for your architecture gets solidified, you can refer to the [reference architecture template](https://github.com/terraform-ibm-modules/documentation/blob/main/docs/templates/reference-architecture-template.md){: external} as a guide for documenting your pattern. This includes creating an architecture diagram, design requirements heatmap, and documenting requirements and the architecture decisions for the chosen components.


## Next steps: Choosing what to create
{: #choose-what-to-build}

It's important to understand the key differences between modules and types of deployable architectures to determine which you plan to create. The scope, coupling, whether it's deployable, and the purpose of your solution should all be taken into account. For guidance and use cases to help you decide what you plan to build, see [How do I decide what kind of component to create](/docs/secure-enterprise?topic=secure-enterprise-choose-plan-process).
