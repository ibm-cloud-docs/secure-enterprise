---

copyright:
  years: 2024, 2025
lastupdated: "2025-04-16"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Best practices for creating deployable architectures
{: #best-practice-deployable-arch}

A [deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-understand-module-da#what-is-da) is a self-contained, modular unit of cloud automation that combines one or more cloud resources to provide a common architectural pattern. It enables simplified deployment, scalability, and modularity, allowing users to easily provision and manage infrastructure resources.
{: shortdesc}

This guide outlines best practices for building well-designed and maintainable deployable architectures that are written in Terraform. Focusing on key attributes such as scope, composability, consumability, and quality checks help ensure robust and reliable solutions. The final section of this guide provides references to tools and templates that help implement these practices.

These best practices apply to creating deployable architectures with Terraform. For more information, see [Creating a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-create-da).
{: note}

## Design principles
{: #design-principles}

Scope, composability, and consumability are the three main design principles that you need to consider when you are creating a deployable architecture.

In the [planning and researching phase](/docs/secure-enterprise?topic=secure-enterprise-starting-da-process), you should evaluate the current ecosystem of offerings and evaluate the business use case and requirements. Use the [Well-Architected Framework](https://www.ibm.com/architectures/well-architected){: external} and the [Architecture Design Framework](/docs/architecture-framework?topic=architecture-framework-intro) to plan and design the required components for the architecture.

### Scope
{: #scope}

A well-defined scope for the deployable architecture is crucial, as it should be comprehensive enough to include all necessary resources, yet focused enough to avoid unnecessary complexity.

A best practice is to include infrastructure resources that are typically deployed together as a unit, require similar access rights and permissions, and have the same lifecycle. For example, let's consider the [VPC landing zone deployable architecture](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-overview#overview-vpc). This deployable architecture has a well-defined scope that includes the following infrastructure resources:

| Resource      | Description     |
|---------------|-----------------|
| VPCs          | Creates a secure VPC topology |
| Network infrastructure   | Includes subnets, public gateways, ACLs, transit gateways, and security groups |
| Edge networking  | Isolates traffic to the public internet |
| Monitoring and logging  | Integrates Flow Logs for observability and auditing of the VPC traffic |
{: caption="Resources in the VPC landingn zone DA" caption-side="top"}

These resources are typically deployed together as a unit, require similar network administrative rights and permissions, and have the same lifecycle, meaning that they:

* Are created together, such as when a new VPC is provisioned with its associated subnets, public gateways, and security groups.
* Are updated together, such as when a change is made to the VPC's network configuration, which requires updates to the subnets, public gateways, and security groups.
* Are deleted together, such as when a VPC is decommissioned and all its associated resources, including subnets, public gateways, and security groups, are removed.

### Composability
{: #composability}

A fundamental principle of a deployable architecture is composability, which enables the creation of a broader deployable architecture by stacking multiple deployable architectures together. This modular approach allows for maximum flexibility and reusability of automation resources.

To achieve composability, a deployable architecture should be designed to:

* Maximize the amount of information surfaced through the output values, yet keep the output types simple. This practice enables the deployable architecture automation to be reused in a wide range of scenarios, and makes it a versatile building block for various automated solutions.

* Allow optional references to existing deployed resources, such as resource groups, {{site.data.keyword.keymanagementservicefull}} or {{site.data.keyword.hscrypto}} instances, and {{site.data.keyword.secrets-manager_full_notm}} instances, among others. Users can then configure existing instances or deploy into existing resource groups, increasing the versatility of your automation. The [{{site.data.keyword.secrets-manager_short}} deployable architecture](https://cloud.ibm.com/catalog/7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3/architecture/deploy-arch-ibm-secrets-manager-6d6ebc76-7bbd-42f5-8bc7-78f4fabd5944-global?kind=terraform&format=terraform&version=ba032ffb-286c-4a00-ad79-ae9e09cea687-global) is a great example of this principle in action. By allowing users to reuse existing {{site.data.keyword.secrets-manager_short}} instances, resource groups, and KMS encryption key, this automation provides a high degree of flexibility and adaptability. For example, you can:
    * Configure an existing {{site.data.keyword.secrets-manager_short}} instance by passing its ID, and create secret groups in the existing instance.
    * Integrate with an existing key management system like {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}.
    * Deploy in an existing resource group or create a new one with customizable naming conventions.

Alternatively, this deployable architecture also allows creating a new {{site.data.keyword.secrets-manager_short}} instance, a new resource group, and other resources from scratch, providing a stand-alone solution.

By embracing composability, a deployable architecture can be easily integrated into a more complex solution architecture by stacking it with other deployable architectures. For example, the [Retrieval Augmented Generation Pattern](https://cloud.ibm.com/catalog/7a4d68b4-cf8b-40cd-a3d1-f49aff526eb3/architecture/Retrieval_Augmented_Generation_Pattern-5fdd0045-30fc-4013-a8bc-6db9d5447a52-global) demonstrates how multiple deployable architectures, including the {{site.data.keyword.secrets-manager_short}} deployable architecture, can be combined to build a complex solution. Deployable architectures that are designed with composability in mind provide the foundation for these complex solutions.

When deployable architectures are stacked together, each member deployable architecture maintains its independent configuration state, allowing for individual deployment, update, or undeployment. This modular approach enables cost, compliance, support, and quality assurances to be derived from the included deployable architectures, while the overall solution remains uniquely versioned with its own descriptions and reference architecture. 

### Consumability
{: #consumability}

A deployable architecture should be designed with consumability in mind, making it easy for users to understand and deploy. To achieve this, the deployable architecture should provide comprehensive documentation that includes the following:

Prerequisites
:   Software dependencies and infrastructure requirements necessary for deployment.

Detailed input variables and output values descriptions
:   Including purpose, data type, and default values.

Necessary minimal permissions
:   Required permissions to run the deployable architecture automation.

Diagrams and architecture maps
:   Visual representations of the deployable architecture's components and relationships.

Simplified configuration
:   Easy to deploy and manage.

Reduced resource requirements
:   Optimize the deployable architecture to minimize hardware and resource requirements, such as lower CPU and memory requirements, making it more affordable and efficient.

Streamlined deployment
:   Faster and easier to get started.

Another facet of ensuring the deployable architecture is easily consumable is by providing multiple variations, including a quick start variation. A quick start version of the deployable architecture should be provided, which is cheaper and faster to run with. For example, the [QuickStart variation of the {{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-roks-ra-qs){: external} deployable architecture creates a fully customizable Virtual Private Cloud (VPC) environment in a single region, providing a single {{site.data.keyword.redhat_openshift_notm}} cluster in a secure VPC for workloads. This quick start variation is designed for demonstration and development purposes, and costs less than $400 per month to run.

In contrast, the standard version of the [{{site.data.keyword.redhat_openshift_notm}} Container Platform on VPC landing zone](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-ocp-ra) deployable architecture, based on the {{site.data.keyword.framework-fs_full}} reference architecture, creates secure and compliant {{site.data.keyword.redhat_openshift_notm}} Container Platform workload clusters on a Virtual Private Cloud (VPC) network, but costs over $4,000 per month to run. And, it includes advanced features such as management VPC service, workload VPC service, isolation of management VPC and workload VPC, and advanced network security architecture decisions.

#### Input variables
{: #input-vars-best-practice}

Make it easier for users to configure the deployable architecture's input variables by using the following best practices:

Expose only commonly modified arguments
:   Expose only the variables that most users will need to change, avoiding deployable architectures with a large number of input variables that would overwhelm users. For advanced users, consider providing a single JSON input field for further customization. As an example, the VPC Landing zone deployable architecture surfaces a single field that is named `override_json_string` that gives full control to advanced users on the deployed topology. For more information, see [the VPC Landing zone deployment guide](/docs/secure-infrastructure-vpc?topic=secure-infrastructure-vpc-override-kms).

Use clear and descriptive naming for existing resources
:   When referring to existing resources, use names that clearly indicate what they refer to, such as `existing_cluster_name` instead of `cluster_name`, to avoid ambiguity.

Prefer names over IDs
:   When referring to existing resources, use names instead of IDs for better user consumability.

Avoid acronyms
:   Instead of using acronyms, use full product names to make it easier for people not familiar with the products or services to understand what they refer to. For example, `secrets_manager` instead of `sm`, or `key_management` instead of `kms`.

Use advanced typing capabilities
:   To enable the {{site.data.keyword.cloud_notm}} Projects service to render appropriate input widgets for variables, which makes it easier for users to configure values. For example:

   * VPC Region: a dropdown list of all available VPC regions on {{site.data.keyword.cloud_notm}}.
   * VPC SSH Key: a secure input field for SSH key management.
   * Cluster: a dropdown list of available clusters on {{site.data.keyword.cloud_notm}}.

   For more information, see [Locally editing the catalog manifest values](/docs/secure-enterprise?topic=secure-enterprise-manifest-values&interface=ui).

By following these guidelines, the deployable architecture can be made more consumable, allowing users to quickly understand and deploy it. 

## Quality
{: #deploy-arch-quality}

To ensure the deployable architecture is reliable and consistent, it is essential to implement and automate quality checks. These checks should cover various aspects of the deployable architecture, including code quality, configuration validation, testing, and continuous integration.

### Code quality
{: #code-quality}

Leverage linting and code formatting. Enforce consistent coding styles and formatting to make the code easy to read and maintain. Detect errors and warnings in the code to prevent issues during deployment. Consider going beyond just the Terraform code, but incorporate any tools relevant to all resources in your deployable architecture, such as Bash scripts, Python scripts, YAML and JSON files, and Golang.

Examples:
* [`terraform_fmt`](https://www.terraform.io/docs/cli/commands/fmt.html){: external} to format Terraform code.
* [`go-fmt`](https://golang.org/cmd/go/#hdr-Run_'go_fmt'_on_package_sources){: external} to format Go code.
* [`black`](https://black.readthedocs.io/en/stable/){: external} to format Python code.
* [`isort`](https://pycqa.github.io/isort/){: external} to sort Python imports.
* [`flake8`](https://flake8.pycqa.org/en/latest/){: external} to check Python code for errors and warnings.
* [`shellcheck`](https://www.shellcheck.net/){: external} to check shell scripts for errors and warnings.
* [`golangci-lint`](https://golangci-lint.run/){: external} to check Go code for errors and warnings.

### Configuration validation
{: #config-validation}

Use static validation to check the deployable architecture's syntax and configuration to ensure it is correct and consistent. Validate the deployable architecture's configuration to prevent errors during deployment. Again, consider going beyond just the Terraform code, and incorporate any tools relevant to all resources in your deployable architecture, such as Bash scripts, Python scripts, YAML and JSON files, and Golang.

Examples:
* [`terraform_validate`](https://www.terraform.io/docs/cli/commands/validate.html){: external} to validate Terraform configuration.
* [`checkov`](https://www.checkov.io/){: external} to check for security and compliance issues in Terraform code.
* [`tflint`](https://github.com/terraform-linters/tflint){: external} to check for errors and warnings.
* [`detect-secrets`](https://github.com/ibm/detect-secrets){: external} to detect secrets in code.
* [`hadolint`](https://github.com/hadolint/hadolint){: external} to check Docker files for errors and warnings.
* [`helmlint`](https://helm.sh/docs/helm/helm_lint/){: external} to check Helm charts for errors and warnings.

### Testing
{: #testing-deploy-arch}

When it comes to testing infrastructure code, there is no pure unit testing in the way that you might think of it for application code. Instead, the test strategy involves deploying the infrastructure to a real environment, validating that it works, and then undeploying it.

#### Automated validation test suite
{: #automated-validation-testing}

The recommendation is to have a basic automated test suite that covers the following fundamentals:

Deployment tests
:   Verify that the infrastructure code can be successfully deployed to a real environment. Create all necessary resources, such as virtual machines, databases, and networks. These tests help ensure that the infrastructure code is correct and can be successfully applied to a real environment. It is recommended to vary the input parameters of the deployable architecture in these tests to ensure broad coverage aligned with common usage.

Destruction tests
:   Verify that the infrastructure code can be successfully undeployed or destroyed, removing all created resources. These tests ensure that the infrastructure code can be safely removed from a real environment, without leaving behind orphaned resources or causing unintended consequences.

Idempotency tests
:   Verify that the infrastructure code can be reapplied multiple times without causing unintended changes or errors. In other words, the code should produce the same result regardless of how many times it is applied. These tests are critical in environments like {{site.data.keyword.cloud_notm}}, where the platform periodically checks for changes to detect drift between the deployed infrastructure and the source of truth, which is the automation code. Idempotency tests help ensure that the infrastructure code can handle repeated deployments or updates without causing issues. These tests also help ensure that drift detection capabilities can accurately identify and remediate any discrepancies between the intended state and the actual state of the infrastructure. For more information, see [Managing drift](/docs/secure-enterprise?topic=secure-enterprise-manage-drift).

Version upgrade tests
:   Verify that the infrastructure code can be successfully upgraded from one version to another, without causing errors or unintended changes. These tests ensure that the infrastructure code can be safely upgraded, without disrupting or destroying existing resources or causing unintended consequences.

#### Advanced test cases
{: #advanced-test-cases}

The advanced test cases include scenarios such as:

Deploying the deployable architecture multiple times in the same account
:   Verify that the infrastructure code can handle multiple deployments in the same account, without causing resource name clashes or other issues.

Deploying the deployable architecture with a trusted profile
:   Verify that the infrastructure code can be deployed with an {{site.data.keyword.iamshort}} trusted profile. For more information, see [Defining an authentication method](/docs/secure-enterprise?topic=secure-enterprise-best-practices-projects#best-practice-auth).

By including these tests in your automated testing suite, you can ensure that your infrastructure code is reliable, robust, and safe to deploy to production.

### Continuous integration
{: #continuous-intergration}

To ensure the reliability, consistency, and maintainability of the deployable architecture, a shift-left approach is recommended, where quality checks and testing are integrated early in the development cycle. This approach helps catch errors and defects early, reducing the likelihood of downstream problems and improving overall quality.

As part of this approach, the following quality checks are recommended:

Client-side quality control
:   Client-side [Git commit hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks){: external} should be used to run checks on the developer machine before committing code. This includes checks for coding standards, syntax errors, and sensitive data. Tools like [Pre-commit](https://pre-commit.com/){: external} can be used to automate this process.

CI practices
:   Best practices for continuous integration (CI) should be followed. The general practices for any software engineering product applies to deployable architecture development, including:

   * Working with small, focused pull requests (PRs) to facilitate timely reviews and reduce merge conflicts.
   * Integrating code changes into the main branch on a regular basis to prevent long-lived feature branches and reduce merge complexity.
   * Implementing automated testing and code reviews to ensure code quality and consistency.
   * Continuously validating the deployable architecture's configuration and syntax to ensure correctness and consistency.

CI Pipeline
:   A CI pipeline should be set up to automate these practices, ensuring that every code change in the deployable architecture is systematically tested and validated. This pipeline ensures that the deployable architecture works correctly and consistently, and that any errors or defects are caught early.

## Tools and resources
{: #tools-resources}

A comprehensive set of tools and resources is provided to facilitate the creation of high-quality deployable architectures. Curated Terraform modules are a key piece, with over 60+ reusable, secure, and validated modules that cover a wide range of infrastructure needs. These modules are available on [GitHub](https://github.com/terraform-ibm-modules){: external} and are supported and kept current through an open source contribution model, and backed by contributions from the {{site.data.keyword.cloud_notm}} development organization.

In addition to the curated Terraform modules, best practices and templates are also provided to help with deployable architecture and module authoring. This includes documentation, a [GitHub deployable architecture repository template](https://github.com/terraform-ibm-modules/terraform-ibm-module-template){: external} that is aligned with authoring best practices, and [module authoring guidelines](https://terraform-ibm-modules.github.io/documentation/#/implementation-guidelines){: external} that apply to both Terraform modules and Terraform-based deployable architectures. These resources can be used to quickly get started on a new deployable architecture.

An automated testing framework is also provided, and based on the Terratest library, with tests written in Go. The framework covers idempotency tests, upgrade tests, and GitHub, and uses test helper functions from the [https://github.com/terraform-ibm-modules/ibmcloud-terratest-wrapper](https://github.com/terraform-ibm-modules/ibmcloud-terratest-wrapper){: external} library. For more information, see our [testing documentation](https://terraform-ibm-modules.github.io/documentation/#/tests){: external}.

To support CI pipeline development, a range of tools and resources are available, including:

* Reusable [GitHub Actions](https://terraform-ibm-modules.github.io/documentation/#/gh-actions){: external}.
* Automated generation of documentation.
* Automated onboarding to {{site.data.keyword.cloud_notm}}.
* Automated dependency updates by using custom renovate.
* Local development setup tooling and pre-commit hooks configuration, see our [local development setup documentation](https://terraform-ibm-modules.github.io/documentation/#/local-dev-setup){: external} for more information.

These tools and resources are designed to accelerate and facilitate the creation of high-quality deployable architectures.

## Next steps
{: #bp-da-next-steps}

Now that you understand the best practices for building a deployable architecture, you can use the tools and resources and review the following {{site.data.keyword.cloud_notm}} documentation before developing your automation code. This helps ensure that you thoroughly planned and designed your solution to share in {{site.data.keyword.cloud_notm}}:

* [Planning and researching for designing an architecture](/docs/secure-enterprise?topic=secure-enterprise-starting-da-process) to ensure that you're designing an architecture that is a viable reusable pattern that meets your business requirements.
* [How do I decide what type of component to create?](/docs/secure-enterprise?topic=secure-enterprise-choose-plan-process) to make sure you understand the differences between creating a module, creating a deployable architecture, and stacking deployable architectures together.
* [How do I decide where to share my solution?](/docs/secure-enterprise?topic=secure-enterprise-publish-da-options) to ensure you're meeting the requirements depending on where you plan to share or publish your solution.
