---

copyright:
   years: 2024, 2025
lastupdated: "2025-06-11"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Creating a module
{: #create-module}

A module is a stand-alone unit of automation code that can be reused by developers and shared in a larger system. Modules are not independently deployable and usually manage a smaller number of related resources and are used to build deployable architectures. Typically coded in Terraform or Ansible, modules are a convenience for developers similar to Node.js or Python packages. Think of modules as building blocks that can be used in combination with other modules to build a complete deployable architecture that solves a complex infrastructure need.
{: shortdesc}

Modules that are created by {{site.data.keyword.IBM}} are created and sourced in [`terraform-ibm-modules`](https://github.com/terraform-ibm-modules/){: external}, a public GitHub org. Modules include usage information and one or more runnable examples. This enables users who you share your modules with to copy the usage code to get started with any module, but they are not deployable from a private catalog.

## Guidelines and requirements for creating a module
{: #guidelines-modules}

The guidelines and requirements for creating a module are available in the {{site.data.keyword.cloud_notm}} Terraform modules documentation. You can refer to the following resources for creating your own modules:

* [Authoring guidelines](https://terraform-ibm-modules.github.io/documentation/#/implementation-guidelines){: external}
* [Module structure](https://terraform-ibm-modules.github.io/documentation/#/module-structure){: external}


## Next steps: Sharing modules
{: #next-steps-modules}

Modules can be used to [create a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-create-da). You have a couple of options for sharing your module with others in your organization or external users.

* You can share with users in your account or other accounts by using a [private catalog](/docs/secure-enterprise?topic=secure-enterprise-catalog-share&interface=ui), if you have a Git release.
* You might also choose to contribute your modules to the `terraform-ibm-modules` public GitHub org. For more information, see [Contributing to the {{site.data.keyword.cloud_notm}} Terraform modules project](https://terraform-ibm-modules.github.io/documentation/#/contribute-module){: external}.
