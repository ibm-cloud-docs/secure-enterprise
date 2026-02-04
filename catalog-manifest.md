---

copyright:
  years: 2022, 2026
lastupdated: "2026-02-04"

keywords: onboard, catalog management, private catalog, catalog manifest, software, automation, metadata

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Editing the catalog manifest locally 
{: #manifest-values}

The catalog manifest file specifies the information about your onboarded solution that you want to share with users through a catalog. You can provide licensing and compliance information, make specific settings, and provide descriptions about the intended purpose of your product.
{: shortdesc}

Prefer to use the console to edit your catalog details? You can make the selections by following the [provided wizard and then export the manifest file](/docs/secure-enterprise?topic=secure-enterprise-onboard-da) to add to your source repo. If you are [stacking deployable architectures in a project](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui), the catalog manifest is created for you when you add your architectures to a private catalog from your project.
{: tip}

## Mapping catalog details to the manifest file
{: #mapping-manifest}

To help visualize how the content added to the manifest file is displayed to users, see the following examples that show the relationship between the `ibm_catalog.json` and the catalog details page.

Let's look at how the deployable architecture name, description, features, and variations are defined in the catalog manifest file and how the user sees the information on the catalog details page.

![Deployable architecture title, description, features text mapping to the source file](images/da-basic-catalog-details-mapping.png "Deployable architecture title, description, features text mapping to the source file"){: caption="Deployable architecture title, description, features text mapping to the source file" caption-side="bottom"}

Let's look at how the variation features list is used to help users compare variations based on how it's defined in the catalog manifest file.

![Deployable architecture variation feature comparison](images/da-feature-highlights-mapping.png "Deployable architecture variation feature comparison"){: caption="Deployable architecture variation feature comparison" caption-side="bottom"}

Let's look at where the permissions and architecture diagram details are specified in the catalog manifest file and how it's displayed on the catalog details page.

![Deployable architecture permissions and architecture text mapping to the source file](images/da-arch-iam-details-mapping.png "Deployable architecture permissions and architecture text mapping to the source file"){: caption="Deployable architecture permissions and architecture text mapping to the source file" caption-side="bottom"}

And, if your architecture meets a specific level of compliance that is verified with inventory results by using {{site.data.keyword.sysdigsecure_short}}, you can claim that compliance per variation. You define how your architecture meets a certain level of compliance in the `ibm_catalog.json` file by specifying the {{site.data.keyword.sysdigsecure_short}} policy. You must also deploy the resources that your architecture creates, as {{site.data.keyword.sysdigsecure_short}} uses those deployed resources to verify compliance. For more information, see [Managing compliance information for your deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-format-controls).

See in the following example how the compliance information that is defined in the manifest file is displayed to users.

![Deployable architecture compliance](images/da-compliance-mapping.png "Deployable architecture compliance"){: caption="Deployable architecture compliance" caption-side="bottom"}

## Editing your manifest
{: #edit-manifest}

To edit your manifest locally, you can use the following steps.

1. Copy the following example manifest file into a local editor.
2. Name the file `ibm_catalog.json`.
3. Add your preferred configurations into the file by using the example manifest as a guide. To learn more about each value, view the [available values](#available-values).
4. Add the file into the root folder of your source code repository.
5. [Add your deployable architecture to your catalog](/docs/secure-enterprise?topic=secure-enterprise-onboard-da).

If your deployable architecture is already onboarded to a private catalog, you can [download the manifest](/docs/secure-enterprise?topic=secure-enterprise-onboard-da#download-manifest) from the console.
{: tip}

## Example manifest file
{: #example-manifest}

The following code snippet can be used as a template.

```json
{
   "products": [
      {
         "name": "",
         "label": "",
         "product_kind": "",
         "tags": [
            "tag 1",
            "tag 2"
         ],
         "keywords": [
            "keyword 1",
            "keyword 2",
            "keyword 3"
         ],
         "short_description": "Short description of your product.",
         "long_description": "A longer description of your product.",
         "offering_docs_url": "URL",
         "offering_icon_url": "URL or emebbed image",
         "provider_name": "Community",
         "module_info": {
            "works_with": [
               {
                  "catalog_id": "",
                  "name": "module name",
                  "kind": "terraform",
                  "version": "0.1.0",
                  "flavor": "Variation name"
               }
            ]
         },
         "support_details": "Explanation of support.",
         "features": [
            {
               "title": "Feature 1 title"
               "description": "Feature 1 description"
            },
            {
               "title": "Feature 2 title"
               "description": "Feature 2 description"
            }
         ],
         "flavors": [
            {
               "label": "Display name",
               "name": "Programatic name",
               "index": 1,
               "install_type": "Install type",
               "working_directory": "Directory path",
               "usage_template": "template",
               "scripts": [
                  {
                     "type": "ansible",
                     "short_description": "Short description of what your script is intended to do.",
                     "path": "Path to script location.",
                     "stage": "The stage. For example, pre.",
                     "action": "The action. For example, validate."
                  }
               ],
               "change_notices": {
                  "breaking": [
                     {
                        "title": "Title of breaking change",
                        "description": "Description of the change."
                     }
                  ],
                  "new": [
                     {
                        "title": "Title of new feature",
                        "description": "Description of the new feature or capability."
                     }
                  ],
                  "update": [
                     {
                        "title": "Title of general update",
                        "description": "Description of the general update."
                     }
                  ]
               },
               "compliance": {
                  "authority": "scc-v3",
                  "controls": [
                     {
                        "profile": {
                           "name": "Security and Compliance Center profile name",
                           "version": "Profile version"
                        },
                        "names": [
                           "Control name 1 e.g. AC-2(a)",
                           "Control name 2",
                           "Control name 3"
                        ]
                     }
                  ]
               },
               "configuration": [
                  {
                     "key": "key type e.g. ssh_key",
                     "required": true
                  },
                  {
                     "key": "Key type e.g. ibmcloud_api_key",
                     "required": true,
                     "type": "The data type"
                  }
               ],
               "outputs": [
                  {
                     "description": "Output description",
                     "key": "key"
                  },
                  {
                     "description": "Output description",
                     "key": "key"
                  }
               ],
               "dependencies": [
                  {
                     "catalog_id": "ID",
                     "id": "ID",
                     "name": "Product programmatic name",
                     "kind": "Format kind",
                     "version": "Versions or range of versions",
                     "flavors": [
                        "Variation name 1",
                        "Variation name 2",
                        "Variation name 3"
                     ],
                     "install_type": "fullstack or extension",
                  }
               ],
               "iam_permissions" [
                  {
                     "role_crns": [
                        "CRN 1 e.g. crn:v1:bluemix:public:iam::::serviceRole:Manager",
                        "CRN 2 e.g. crn:v1:bluemix:public:iam::::role:Administrator"
                     ],
                     "service_name": "Programatic service name e.g. is.vpc"
                  }
               ],
               "licenses": [
                  {
                     "name": "License name",
                     "smref": "Link to the license"
                  }
               ],
               "schematics_env_values": {
                  "value": "value",
                  "smref": " "
               },
               "architecture": {
                  "descriptions": " ",
                  "features": [
                     {
                        "title": "Feature 1 title",
                        "description": "Feature 1 description"
                     },
                     {
                        "title": "Feature 1 title",
                        "description": "Feature 1 description"
                     }
                  ],
                  "diagram": {
                     "caption": "Diagram caption",
                     "url": "Link to diagram or embedded image",
                     "metadata": []
                  },
                  "description": "Description of the diagram"
               }
            }
         ]
      }
   ]
}
```
{: codeblock}

## Available values
{: #available-values}

The following sections include information about each value that can be referenced in your manifest file.

### Products
{: #value-products}

The products value indicates an array of products with size one or more. If a catalog manifest file exists at the root of your repository, only the products within the file can be imported. Products are imported one at a time. The following values can be included at the `products` level:

#### `label`
{: #label}

The product display name. This value must match the display name that you provide during onboarding.

#### `name`
{: #name}

The programmatic name of the product.

#### `version`
{: #version}

The version of the product in SemVer format, including the major version, minor version, and revision, for example, 1.0.0. This value can also be specified when the product is onboarded to a catalog.

#### `product_kind`
{: #product_kind}

The kind of product that you are onboarding. Valid values are software, module, or solution. A solution is otherwise known as a deployable architecture.

#### `tags`
{: #tags}

An array of predefined values that can help users to filter the catalog to identify and learn more about your product. To view the available options, run the following command: `ibmcloud catalog filter options --all`.

#### `keywords`
{: #keywords}

An array of specific words or phrases that a user might try to search for.

#### `short_description`
{: #short-desc}

A concise summary of what your product is and its value.

#### `long_description`
{: #long-desc}

A detailed description of your product that explains the product's value and benefits to users.

#### `provider_name`
{: #provider-name}

Users can filter the catalog by the provider of a product. When you onboard a product to a private catalog, the provider name is set to `Community` by default. However, you can customize this field to display your company or organization name. IBM is a reserved value and can be used for IBM build products only.

#### `offering_docs_url`
{: #offering-docs-url}

A link to documentation about the product that users can access.

#### `offering_icon_url`
{: #offering-icon-url}

A link to the URL where the icon that you want to appear on the product's catalog entry page is located.

#### `support_details`
{: #support-details}

Support information in markdown format that can include support contacts, support locations, and support methods.

#### `features`
{: #features}

Section header within `products` for details that highlight the processes, abilities, and results of the product. These product-level features are listed on your catalog entry page with your product description. For example, features might include CPU requirements, security features, or more. Each entry is defined as an array as shown in the example manifest in the previous section. The following values can be included in the `features` section:

`features[].title`
:   The name of the feature.

`features[].description`
:   A concise description of the feature.

### Modules
{: #value-module}

The `module_info` value indicates information about other products that the deployable architecture is compatible with. The following values can be included within the `module_info` section:

#### `works_with`
{: #works_with}

Section header for information about a singular product that is compatible with the deployable architecture. The following values can be included in the `works_with` section: 

`works_with[].catalog_id` (optional)
:   ID of the catalog that houses the product. If not specified, the {{site.data.keyword.cloud_notm}} catalog is the default.

`works_with[].id` (optional)
:   ID of the product. The ID is not required if the `name` value is set.

`works_with[].name` (optional)
:   Programmatic name of the product that works with the deployable architecture.

`works_with[].kind`
:   The format of the module that works with your deployable architecture. Most often this is `terraform`.

`works_with[].version`
:   Version or range of product versions that work with the deployable architecture in SemVer format.

`works_with[].flavors[]` (optional)
:   The programmatic names of the compatible variations. Variations are onboarded individually to a catalog and are given a version number. An example variation name might be `standard` or `advanced`.

### Flavors
{: #value-variations}

Section header for information about the deployable architecture variations. Flavors are now known as variations in the console. The following values can be included at the `flavors` level:

#### `label`
{: #flavors-label}

Variation display name.

#### `name`
{: #flavor-name}

Variation programmatic name. 

#### `short_description`
{: #flavor-shortdesc}

A short description for this version of the variation.

#### `index`
{: #flavor-index}

The order that the variations are listed in the catalog listing. 

#### `working_directory`
{: #working-dir}

For a working directory that is at the root level of your repo, you don't need to specify the working directory. If it is not at the root, then list the path from the root of your repository. For example, `./examples/`.

#### `usage`
{: #usage}

Information on how to embed the architecture or run it locally through Terraform.

#### `usage_template`
{: #usage-template}

Similar to `usage`. With a template you can use variables as a place holder where the values can be substituted. The string is stored in the `usage` property.

| Template variable | Replacement value |
|:------------------|:------------------|
| ${{version}} | The version string of this variation or flavor. |
| ${{flavor}} | The programmatic name of the variation or flavor. |
| ${{kind}} | The implementation kind. I.e. `terraform`. |
| ${{id}} | The offering or product ID. |
| ${{name}} | The programmatic name of the offering or product. |
| ${{catalogID}} | The ID of the catalog where the offering or product is located. |
| ${{workingDirectory}} | The working directory of the flavor or variation. |
{: caption="Usage template values and descriptions" caption-side="bottom"}

#### `licenses`
{: #licenses}

Section header within the `flavors` section that provides information about the end user license agreements that users are required to accept when they install the product. The license agreements are in addition to the {{site.data.keyword.cloud_notm}} Services Agreement.

```json
{
	"id": "string, license id",
	"name": "string, license display name",
	"type": "string, type of license, e.g. Apache xxx",
	"url": "string, URL for the license text",
	"description": "string, license description"
}
```
{: screen}

The following values can be included within the `licenses` section:

`licenses[].id`
:   The license ID.

`licenses[].name`
:   The name of the license.

`licenses[].type`
:   The type of license. For example, Apache.

`licenses[].url`
:   A URL to where the user can access the license agreement.

`licenses[].description`
:   A description of the license.

#### `compliance`
{: #compliance}

Section header within the `flavors` section that indicates which compliance controls the architecture satisfies with the default installation settings. The evaluation and validation of the claims made is completed by {{site.data.keyword.sysdigsecure_short}}.

The following example shows the JSON structure for the `compliance` section:

```json
"flavors": [{
  "compliance": {
    "authority": "scc-wp-v1",
    "profiles": [{
      "profile_name": "",
      "profile_version": ""
    }],
    "controls": [{
      "profile": {
        "name": "",
        "version": ""
      },
      "names": []
    }]
  }
}]
```
{: codeblock}

You can list multiple policies in your catalog manifest JSON file, but only the first policy is added to your compliance information in a private catalog.
{: important}

The following values can be included in the `compliance` section:

`compliance.authority`
:   {{site.data.keyword.sysdigsecure_short}} v1 is the only authority accepted. This is programmatically written as `scc-wp-v1`.

`compliance.profiles[]`
:   Array of policies that contain the controls being claimed. You can view [predefined policies in {{site.data.keyword.sysdigsecure_short}}](/docs/workload-protection?topic=workload-protection-about&interface=ui#about-available-policies).

`compliance.profiles[].profile_name`
:   The name of the policy. For example, `NIST`. You can find the policy name in {{site.data.keyword.sysdigsecure_short}}.

`compliance.profiles[].profile_version`
:   The version of the policy. For example, `1.0.0`. You can find the policy version in {{site.data.keyword.sysdigsecure_short}}.

`compliance.controls[]`
:   Array of controls claimed on this variation. The catalog manifest accepts an array of controls that you can claim by specifying a control's profile name, profile version, and control name.

`compliance.controls[].profile`
:   Object that indicates you are adding controls from a specific policy.

`compliance.controls[].profile.name`
:   The policy name of the claimed control. For example, `NIST`. You can find the policy name in {{site.data.keyword.sysdigsecure_short}}.

`compliance.controls[].profile.version`
:   The version of the policy. For example, `1.0.0`. You can find the policy version in {{site.data.keyword.sysdigsecure_short}}.

`compliance.controls[].names[]`
:   Array of claimed control names. For example: `["CM-7(b)", "AC-2(a)"]`.

If you have included controls in your readme and your catalog manifest file, the manifest file takes precedence. It is best practice to make sure the controls that are listed in your catalog manifest file match the controls in your readme file.
{: note}

#### `change_notices` (optional)
{: #change_notices}

A list of the three types of changes that you might want to alert your users to when releasing a new version of your deployable architecture. You can specify `breaking changes`, `new features`, and `general updates`. Breaking changes are those updates that break functionality that was available through a previous version. New features highlight any new functionality that a user might encounter with the new version. Updates encompass any changes that you want to highlight to a user such as a modified behavior that doesn't necessarily break existing functionality or changes that make the deployable architecture easier for them to use.

```json
"change_notices": {
   "breaking": [
      {
         "title": "",
         "description": ""
      }
   ],
   "new_features": [
      {
         "title": "",
         "description": ""
      }
   ],
   "updates": [
      {
         "title": "",
         "description": ""
      }
  ]
}
```
{: codeblock}

#### `iam_permissions` (optional)
{: #iam_permissions}

Section header for a list of all IAM permissions that are required for a user to work with your deployable architecture version. IAM permission information includes the programmatic name of the service that is required and a list of CRNs for roles that are needed. If you build your catalog manifest file from the UI, the CRNs are already included.

The following example shows the JSON structure for the `iam_permissions` section:

```json
"flavors": [{
  "iam_permissions": [{
    "service_name": "IAM defined service name",
    "notes": "Optional notes about this permission",
    "role_crns": ["crn:v1:..."],
    "resources": [{
      "name": "resource name",
      "description": "resource description",
      "role_crns": ["crn:v1:..."]
    }]
  }]
}]
```
{: codeblock}

The following values can be included in the `iam_permissions` section:

`iam_permissions[].service_name`
:   The programmatic name of the service that users must have access to.

`iam_permissions[].notes` (optional)
:   Provide more information for users about this role or why it's included. For example, `This role is only required if you are using {{site.data.keyword.keymanagementservicelong_notm}} for encryption.`

`iam_permissions[].role_crns[]`
:   Section header to indicate a list of access roles.

`iam_permissions[].resources[]`
:   Array of resources for a permission.

`iam_permissions[].resources[].name`
:   The name of the resource.

`iam_permissions[].resources[].description`
:   A description of the resource.

`iam_permissions[].resources[].role_crns[]`
:   Section header to incidate a list of access roles.

#### `architecture`
{: #architecture}

Section header within the `flavors` section that specifies high-level information about the deployable architecture version that includes a description, features, and a diagram. Multiple diagrams, with captions, can be provided.

The following example shows the JSON structure for the `architecture` section:

```json
"flavors": [{
  "architecture": {
    "features": [{
      "title": "",
      "description": ""
    }],
    "diagrams": [{
      "diagram": {
        "caption": "",
        "url": "",
        "type": "image/svg+xml",
        "thumbnail_url": ""
      },
      "description": ""
    }]
  }
}]
```
{: codeblock}

The following values can be included in the `architecture` section:

`architecture.features[]`
:   Array of information that highlights the processes, abilities, and results of the version, or if applicable, architecture variation. When onboarding by using the console, these details are called highlights. These details appear on the variation selection box within your catalog entry. If your product has multiple architecture variations, users can compare the variation-level features to decide which variation suits their needs.

`architecture.features[].title`
:   Name of the feature.

`architecture.features[].description`
:   A description of the feature.

`architecture.diagrams[]`
:   Array of architecture diagrams that includes the diagram caption, the URL to embed the SVG of the diagram, the diagram metadata such as element ID and element description, and the description of the reference architecture.

`architecture.diagrams[].diagram`
:   Object containing information about a singular architecture diagram.

`architecture.diagrams[].diagram.url`
:   The URL to the diagram's SVG. You can also embed an SVG.

`architecture.diagrams[].diagram.api_url`
:   The catalog management API URL to the diagram.

`architecture.diagrams[].diagram.url_proxy`
:   Object containing information about a proxied image.

`architecture.diagrams[].diagram.url_proxy.url`
:   The URL to the proxied image.

`architecture.diagrams[].diagram.url_proxy.sha`
:   The `sha` identifier of the image.

`architecture.diagrams[].diagram.caption`
:   A short label for the architecture diagram.

`architecture.diagrams[].diagram.type`
:   The type of media.

`architecture.diagrams[].diagram.thumbnail_url`
:   A link to a thumbnail for the diagram.

`architecture.diagrams[].description`
:   Information about architecture diagram as a whole, including the outline of the system and the relationships, constraints, and boundaries between components of the deployable architecture.

#### `dependencies`
{: #dependencies}

Section header within the `flavors` section for a list of products that are compatible with the deployable architecture. Dependencies can be required or optional. A dependency included here can't be added to the `swappable_dependencies` section as well. Information includes the programmatic name of the product and product versions. Optionally, you can include the catalog ID and a list of dependent variations.

The following example shows the JSON structure for the `dependencies` section:

```json
"flavors": [{
  "dependencies": [{
    "catalog_id": "catalog ID",
    "id": "offering ID",
    "name": "offering name",
    "kind": "terraform",
    "version": "SemVer version e.g. 3.1.2",
    "flavors": ["flavor name"],
    "install_type": "fullstack or extension",
    "optional": true,
    "description": "Description of optional dependency",
    "on_by_default": false,
    "input_mapping": [{
      "dependency_output": "kms_instance_crn",
      "version_input": "existing_kms_instance_crn"
    }]
  }]
}]
```
{: codeblock}

You can provide information about required architectures that meet a dependency and optional architectures that work with your own as you onboard your deployable architecture to a catalog. For more information, go to [Extending a deployable architecture during onboarding](/docs/secure-enterprise?topic=secure-enterprise-extend-da).
{: tip}

The following values can be included in the `dependencies` section:

`dependencies[].catalog_id` (optional)
:   ID of the catalog that houses the product. If not specified, the {{site.data.keyword.cloud_notm}} catalog is the default.

`dependencies[].id` (optional)
:   The product ID. The ID is not required if the `name` value is set.

`dependencies[].name` (optional)
:   Programmatic name of the product.

`dependencies[].kind`
:   The format kind of the dependency. Use `stack` for a deployable architecture made of grouped deployable architectures where a stack config file is present. Use `terraform` for deployable architectures made solely of one or more modules.

`dependencies[].version`
:   A version or range of versions to include as dependencies in SemVer format.

`dependencies[].flavors[]` (optional)
:   Array of variation names that the architecture is compatible with.

`dependencies[].default_flavor` (optional)
:   Specifies a default variation that is selected for your users when multiple variations are compatible with or required to deploy your architecture. Your users can select a different variation if it's included in the `flavors` property. The value is the `name` of the variation. To use this property, you must also set `dependency_version_2` to `true`. If not set, then a default variation is not provided for your users.

`dependencies[].optional`
:   Specifies whether the dependency is required or not required. The default value is `false`. To use this property, you must also set `dependency_version_2` to `true`.

`dependencies[].description` (optional)
:   Provide a description for an optional architecture that's compatible with your own, so users can understand how the architecture works within the broader solution and why they might want to include it. To use this property, you must also set `dependency_version_2` to `true`.

`dependencies[].on_by_default`
:   Specifies whether an optional dependency is selected for users when they add your deployable architecture to a project from a catalog. Users can deselect the architecture if they do not want it. The default value is `false`. To use this property, you must also set `dependency_version_2` and `optional` to `true`.

`dependencies[].input_mapping[]` (optional)
:   Array that specifies the values that are referenced between the compatible architecture and the architecture that you're onboarding. To use this property, you must also set `dependency_version_2` to `true`.

`dependencies[].input_mapping[].dependency_output` or `dependencies[].input_mapping[].dependency_input` (optional)
:   Specifies the variable from the dependency that the architecture that you're onboarding references. The value is the name of the variable from the dependency. Only one of these two properties should be provided. If `reference_version` is set to `true`, then this variable references the `version_input` variable from the architecture that you're onboarding.

`dependencies[].input_mapping[].version_input` (optional)
:   Specifies the name of the input variable in the architecture that you're onboarding that references the `dependency_output` or `dependency_input` value. If `reference_version` is set to `true`, then the `dependency_input` variable references the `version_input` variable from the architecture that you're onboarding.

`dependencies[].input_mapping[].value` (optional)
:   Specifies the preset value for an input from the architecture that you're onboarding (`version_input`) or its dependency (`dependency_input`). The value that is specified here is only used if a `version_input` or `dependency_input` is provided, and `dependency_output` is not provided. If `version_input` is provided, then when the architecture and its dependency are added to a project by a user, the architecture's `version_input` is preset to the value specified here. If `dependency_input` is provided, then when the architecture and its dependency are added to a project by a user, the dependency's `dependency_input` is preset to the value specified here.

`dependencies[].input_mapping[].reference_version` (optional)
:   Indicates the flow of references between the architecture that you're onboarding and its dependency. The default value is `false`. The default behavior is for the architecture input (`version_input`) to reference either an input or output from the dependency (`dependency_input` or `dependency_output`). When this flag is set to `true`, the `dependency_input` references a value from the `version_input`.

#### `dependency_version_2` (optional)
{: #dependency_version_2}

Peer to the `dependencies` section, `dependency_version_2` Specifies that the updated dependency handling is used with this deployable architecture. If you are using the `optional` property or the `input_mapping` sections within the `dependencies` section, set this value to `true`. If not, set it to `false`. If this property is set to `true`, all dependencies that have the `optional` property set to `false` are required to deploy the architecture that you're onboarding. 

#### `swappable_dependencies` (optional) 
{: #swappable}

Section header for a list of products that are compatible with the deployable architecture. Unlike the `dependencies` array, the products in this section are swappable. The user can pick which product they want to use to meet the dependency. Swappable dependencies can be required or optional. A dependency included here can't be added to the `dependencies` array as well. Information includes the programmatic name of the product and product versions. Optionally, you can include the catalog ID and a list of dependent variations. To use this property, you must also set `dependency_version_2` to `true`. 

```json
{
  "optional": "true or false",
  "name": "Name for this group of swappable dependencies",
  "default_dependency": "the name of the dependency that is selected by default", 
  "dependencies": [
    {
      	"name": "offering name",
      	"id": "offering ID",
      	"kind": "terraform",
      	"version": "SemVer version e.g. 3.1.2",
      	"flavors": [
           "flavor name"
        ],
      	"install_type": "fullstack or extension",
      	"catalog_id": "catalog ID",
      	"input_mapping": [
        {
            "dependency_output": "kms_instance_crn",
            "version_input": "existing_kms_instance_crn"
        }
        ]
    },
    {
      	"name": "offering name",
      	"id": "offering ID",
      	"kind": "terraform",
      	"version": "SemVer version e.g. 3.1.2",
      	"flavors": [
           "flavor name"
        ],
      	"install_type": "fullstack or extension",
      	"catalog_id": "catalog ID",
      	"input_mapping": [
        {
            "dependency_output": "kms_instance_crn",
            "version_input": "existing_kms_instance_crn"
        }
        ]
    }
  ]
}
```
{: codeblock}

The following values can be included within the `swappable_dependencies` section:

`swappable_dependencies[].name` (optional)
:   Used when the architecture is onboarded to a catalog to help you identify the specific group of `swappable_dependencies`.

`swappable_dependencies[].default_dependency` (optional)
:   The `name` of one of the dependencies in the group that is selected for users by default.

`swappable_dependencies[].dependencies`
:   Array of dependencies that are swappable within this group. The values within this array are the same as those values documented in the [`dependencies`](#dependencies) section.


#### `release_notes_url`
{: #release_notes_url}

URL to the architecture's release notes.

#### `configuration`
{: #configuration}

Section header within the `flavors` section that specifies the configuration of deployment variables for specific variation. Catalog data types are used to extend native types and facilitate for a better user experience when you're working in the {{site.data.keyword.cloud_notm}} console. If you are running your code on a local machine or another environment, the variables are not used. An example might be a catalog type of `password` that is used to extend the capabilities of a Terraform variable defined with a type of `string` so that it is treated as sensitive in the UI.

The following example shows the JSON structure for the configuration section:

```json
"flavors": [{
  "configuration": [{
    "key": "deployment_variable_name",
    "type": "string",
    "default_value": "default value",
    "description": "Description shown to users",
    "display_name": "Display Name",
    "required": true,
    "hidden": false,
    "options": ["option1", "option2"],
    "custom_config": {
      "type": "widget_id",
      "grouping": "Target",
      "grouping_index": 1
    },
    "value_constraints": [{
      "type": "regex",
      "value": "^.{12,30}$",
      "description": "Must be between 12 and 30 characters"
    }]
  }]
}]
```
{: codeblock}

The following values can be included in the `configuration` section:

`configuration[].key`
:   The configuration key. The value should match the name of a deployment variable.

`configuration[].type`
:   The type of input that a customer can define or select. The data type must be supported by the Catalog Management service. Native Terraform types map to some of the supported types. For example, the Terraform type `map` equates to `object`. The Terraform type `list` equates to `array`. A Terraform type `string` with a sensitive attribute equates to `password`. Customers that consume your deployable architecture must provide values for the input type that you define in the catalog manifest. 

    Supported predefined types:
    * `boolean` requires a `true` or `false` string input from users.
    * `float` requires a point decimal from users.
    * `int` requires an integer input from users.
    * `number` requires a numeric value. The `number` type can represent both whole numbers and fractional values like `4.56`.
    * `password` requires a string input from users. The string is redacted in the console and logs.
    * `string` requires a sequence of Unicode characters representing text. You can optionally include a random string to add as a suffix, which helps avoid naming collisions for strings that are used as prefixes or as base names. You can also specify the length of this random string. The generated string is lower case, a-z, no special characters and preceeded with a dash, for example, `myString-wx`. If a default value is also given, then the suffix is added. If no default value is given, then the value is the suffix without the dash character. For example,

    ```json
    "random_string": {
    	"length": 2
    }
    ```
    {: codeblock}

    * `object` requires a Terraform object input from users. For more information, see [`map`](https://developer.hashicorp.com/terraform/language/expressions/types#map){: external}.

    Predefined types require manual input from users.
    {: note}

    Supported custom types:
    * `array` requires a list of values separated by a comma.
    * `region` requires user to select a region to deploy the deployable architecture from a dropdown list. You can filter the regions that are available to end users. For example, you might specify `country_id:us,ca,jp` in the Region filter to limit the available regions to those countries. For more information, see [Filtering syntax](/docs/hybrid-workloads?topic=hybrid-workloads-managing_locations#filtering_syntax).
    * `textarea` requires users to input text that can be split into multiple lines. For example, a description.
    * `vpc` requires users to select a VPC by name from a dropdown list. The output is the VPC name or ID that your template requires.
    * `vpc ssh key` requires users to select a VPC SSH key for authentication to a virtual machine.
    * `cluster` requires users to select a {{site.data.keyword.containershort}} or {{site.data.keyword.redhat_openshift_notm}} cluster. The output is the cluster ID.
    * `power iaas` requires users to select a {{site.data.keyword.powerSys_notm}} instance.
    * `resource group` requires users to select a resource group. The output is the resource group's ID, name, or CRN.
    * `multi-line secure value` requires users to input text that can be split into multiple lines, which is redacted in the console and logs. For example, if a long key is required, the value is hidden in workspaces.
    * `schematics workspace` requires users to select a specific workspace form a dropdown list. This list is dynamically filtered based on dependencies defined in the deployable architecture. For example, if your deployable architecture, `example-da-1`, depends on another deployable architecture, `example-da-2`, the input dropdown list for `example-da-1` shows only workspaces associated with `example-da-2`. Users then select the appropriate instance of `example-da-2`'s workspace when setting up `example-da-1`.
    * `json editor` gives users a space to specify larger JSON inputs or plain text files.
    * `code editor` gives users a choice of JSON or HCL formatted inputs which is useful for Terraform-based inputs.
    * `Platform resource` requires users to select an instance resource from a list for the type of resource that you specify. The resource type can be `VPC Subnet`, `VPC Image`, `VPC Floating IPs`, `Cloud Logs`, `Sysdig`, `Cloud Object Storage`, `Key Protect`, or `Secrets Manager`. You can specify the ID, name, or CRN as the values that users can choose from and allow single or multiple selections. The output is the name or ID that your Terraform code requires.
    * `secret_group` requires users to select a secret group by name from a specific {{site.data.keyword.secrets-manager_short}} instance. The output is the secret group's ID or name. To list groups from a specific {{site.data.keyword.secrets-manager_short}} instance, this type must be associated with the `platform resource` custom type, with resource type `Secrets Manager`, and with a `crn` value type output.
    * `secret` requires users to select a secret by name from a specific {{site.data.keyword.secrets-manager_short}} instance. The output is the secret's ID, name, or CRN. To list secrets from a specific {{site.data.keyword.secrets-manager_short}} instance, this type must be associated with at least the `platform resource` custom type, with resource type `Secrets Manager`, and with a `crn` value type output. You can optionally associate it with the `secret_group` type as well with a `id` value type output to list secrets from a specific secret group in that {{site.data.keyword.secrets-manager_short}} instance.
    * `kms_key` requires users to select a key from a specific {{site.data.keyword.keymanagementserviceshort}} instance. The output is the key's ID, name, or CRN. To list keys from a specific {{site.data.keyword.keymanagementserviceshort}} instance, this type must be associated with the `platform resource` custom type, with resource type `Key Protect`, and with a `crn` value type output.

`configuration[].default_value`
:   The value that is to be set as the default.

`configuration[].virtual` (optional)
:   Flag that specifies whether an input should be passed to {{site.data.keyword.bpshort}} service. If set to `true`, the input is not passed to {{site.data.keyword.bpshort}}. Set this flag to `true` for any inputs within your deployable architecture that are referenced in compatible architectures but not used in the deployable architecture that you're onboarding. Add references in `input_mapping` within the [`dependencies`](#optional-components) or [`swappable_dependencies`](#swappable) section of the catalog manifest.

`configuration[].description`
:   A description of the variable that you want to display in the UI for users of your deployable architecture.

`configuration[].display_name`
:   The name that is displayed for the configuration type.

`configuration[].required`
:   A boolean that indicates if users are required to specify the parameter during installation.

`configuration[].hidden`
:   A boolean that indicates if the parameter should be hidden from users during installation.

`configuration[].options[]`
:   Array of options that users can choose from for a parameter.

`configuration[].custom_config`
:   Object to indicate that a custom configuration can be used.

`configuration[].custom_config.type`
:   The ID of the widget type used for configuration.

`configuration[].custom_config.grouping`
:   Where the configuration type should appear in the catalog. Valid values are `Target`, `Resource`, and `Deployment`.

`configuration[].custom_config.original_grouping`
:   Where the configuration type originally appeared. Valid values are `Target`, `Resource`, and `Deployment`.

`configuration[].custom_config.grouping_index`
:   The order of this configuration item when there are multiple.

`configuration[].custom_config.config_constraints`
:   Map of constraint parameters that are given to the custom widget.

`configuration[].custom_config.associations`
:   Object for parameters that are associated with the configuration.

`configuration[].configuration_group`
:   The name of an associated configuration group.

`configuration[].value_constraints[]`
:   Array of value constraints, where each constraint defines validation rules.

`configuration[].value_constraints[].type`
:   The type of the constraint. Only `regex` is supported at this time.

`configuration[].value_constraints[].value`
:   A JavaScript regular expression.

`configuration[].value_constraints[].description`
:   A message to display if the provided value does not match the specified regular expression.

#### `schematics_env_values`
{: #schematics_env_values}

Within the `flavors` section, `schematics_env_values` specifies a list of the values and variable names that need to be passed to the {{site.data.keyword.bpshort}} service to be used as environment variables during the execution of the Terraform. This might be a secure value, a setting of the Terraform logging or something else. You can choose to specify a string or create a reference to {{site.data.keyword.secrets-manager_short}}. If both are specified, then the {{site.data.keyword.secrets-manager_short}} reference is used.

The following example shows the JSON structure for the schematics_env_values section:

```json
"flavors": [{
  "schematics_env_values": {
    "value": "[{\"name\": \"TF_LOG\",\"value\": \"TRACE\",\"secure\": true,\"hidden\": true}]",
    "sm_ref": "cmsm_v1:{...}"
  }
}]
```
{: codeblock}

The following values can be included in the `schematics_env_values` section:

`schematics_env_values.value`
:   A JSON string that includes an array of environment variables and their values.

`schematics_env_values.value[].name`
:   Specifies the name of the environment variable.

`schematics_env_values.value[].value`
:   Specifies the value of the environment variable.

`schematics_env_values.value[].secure`
:   Specifies whether or not to display the environment variable value in clear text in the execution log. Possible values are `true` or `false`.

`schematics_env_values.value[].hidden`
:   Specifies whether or not to include this variable in the execution log. Possible values are `true` or `false`.

`schematics_env_values.sm_ref`
:   A reference to a {{site.data.keyword.secrets-manager_short}} instance that contains your environment variables that are saved as a secret. The secret must be a JSON string that includes an array of environment variables and their values.

The following example JSON string includes two variables, `TF_LOG` and `TF_IGNORE`, and their values that are added as environment variables during the Terraform execution:

```json
"schematics_env_values": {
    "value": "[{\"name\": \"TF_LOG\",\"value\": \"TRACE\",\"secure\": true,\"hidden\": true},{\"name\": \"TF_IGNORE\",\"value\": \"TRACE\",\"secure\": false,\"hidden\": false}]"
}
```
{: codeblock}

Use escape characters for the quotation marks within the list.
{: tip}

The following example uses a reference to a secret in {{site.data.keyword.secrets-manager_short}}:

```json
"schematics_env_values": {
    "sm_ref": "cmsm_v1:{\"name\": \"envVarSecret\",\"id\":\"1234567890\",\"service_id\":\"crn:v1:bluemix:public:secrets-manager:eu-gb:a/1234567890:1234567890::\",\"service_name\":\"My SM instance\",\"group_id\":\"1234567890\",\"group_name\":\"My SM group\",\"resource_group_id\":\"1234567890\",\"region\":\"eu-gb\",\"type\":\"arbitrary\"}"
}
```
{: codeblock}

#### `minimum_compatible_version` (optional)
{: #minimum_compatible_version}

A semver value that indicates the earliest version that is compatible with the current version. If no earlier versions are compatible with the current version, specify the current version value in this field.  By default, the current version is compatible with all earlier versions.

#### `ignore_readme`
{: #ignore_readme}

If set to `true`, the readme file is not used when you onboard this version, and the `long_description` field is empty. If the `long_description` field is empty, a link to the readme file does not appear in the **Related Links** menu in the catalog listing for the version.

#### `terraform_version`
{: #terraform_version}

The Hashicorp Terraform runtime version that is needed to validate and install the version. Setting this value in the manifest overrides what is specified in the source code. 

#### `outputs`
{: #outputs-cm}

Section header for information about Terraform output values.

```json
{
   "key": "name of the output value as defined in the Terraform",
   "description": "The description of the key"
}
```
{: codeblock}

The following values can be included in the `outputs` section: 

`outputs[].key`
:   Specifies the output value.

`outputs[].description`
:   A short summary of the output value.

#### `install_type`
{: #install_type}

Specifies whether a deployable architecture is `fullstack` or `extension`. Architectures listed as extensions require prerequisites. The `dependencies` array must also completed if you set this value to `extension`. This property is ignored if `dependency_version_2` is set to `true`. 

#### `scripts`
{: #scripts}

A list of scripts contained within the same repository that can be run by a project during a particular stage of a specified action. Each key in the map must match the format `action` and `stage` in the entry. `Stage` must be either `pre` or `post`. `Action` must be `validate`, `deploy`, or `undeploy`.

```json
{
   "short_description": "description for the script",
   "type": "type of script. i.e. ansible",
   "path": "the path to the script in the repo. Must begin with scripts/...",
   "stage": "pre or post",
   "action": "The action that executes the script. Options include validate, deploy, or undeploy."
}
```
{: codeblock}
