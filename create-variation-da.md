---

copyright:
   years: 2024, 2025
lastupdated: "2025-04-30"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Creating deployable architecture variations
{: #create-variation-da}

A deployable architecture can include variations of capability or complexity. For example, you might create a quick start variation with basic capabilities for simple, low-cost deployment, and then you might have a standard variation with a more complex architecture that would be used in production. Each of these variations is itself a deployable architecture, which is onboarded and configured to appear as options for the same tile in the catalog.
{: shortdesc}

If you're working in the `ibm_catalog.json` catalog manifest file, `variations` are referred to as `flavors`. However, in the catalog details page, they are referred to as variations.
{: tip}

The variations of the core architecture often vary in the following key areas:

* Cost
* Compliance
* Complexity in time and use
* It deploys something different for a specific use case, but solves the same overall business problem.

Here's an example of a deployable architecture with two variations in the catalog. These are two variations that use the same source URL, product name, and version during onboarding to the catalog to ensure that they show up side by side after a user selects the catalog tile:

![A deployable architecture with two variations](images/variation-example.png "Deployable architecture with two variations"){: caption="Deployable architecture with two variations" caption-side="bottom"}

## Adding a variation to an existing deployable architecture
{: #add-variation}

To create another variation of the deployable architecture that you already created, complete the following steps:

1. In your source code repo, create a working directory and add the [required Terraform files for a deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-create-da#required-files).
1. In the manifest file, add the following code snippet into the `flavors` section. This array defines the offering as part of the same deployable architecture, but allows it to be listed as a variation within the catalog. If you [downloaded your manifest from a previously onboarded](/docs/secure-enterprise?topic=secure-enterprise-onboard-da#download-manifest) version in the catalog, the file will already have a minimal definition in place for a new variation.

   For example, if your deployable architecture is called `Dinner` and you want to create a variation of that, you might call it `steak` as shown in the following example.

   ```json
   "flavors": [
    {
        "label": "Steak",
        "name": "steak-variation",
        "working_directory": "./steak",
        "install_type": "fullstack"
    }
   ]
   ```
   {: codeblock}

   The updated catalog manifest file that you upload as part of your release auto-fills most of the configurations for your new variation. However, it is a best practice to validate the configuration before you share the product with your organization.
   {: tip}

You can repeat these steps if you have more variations to add. When you're done, you can create your Git release and start [onboarding to a private catalog](/docs/secure-enterprise?topic=secure-enterprise-onboard-da) to create a catalog tile that you can share with others.

## Adding a variation made of multiple deployable architectures
{: #add-stack-variation}

If you [stacked architectures together in your project](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui), you can add that solution as a variation in the catalog. If a quick start variation of a deployable architecture is simple and low-cost, a solution that was created by stacking is typically more complex and creates more resources than that quick start variation. 

Complete the following steps:

1. Stack deployable architectures in a project. For more information, see [Stacking deployable architectures](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui). 
   
   Configure and deploy the architectures to verify that the overall solution works as designed before you add it as a variation. 
   {: important}

1. Define the variables that users need to configure to deploy successfully. For more information, see [Defining stack variables](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui#stack-define-variables).
1. Create a [stack definition file](/docs/secure-enterprise?topic=secure-enterprise-stack-definition). The stack definition specifies how the stacked deployable architectures relate to each other. The stack variables that you defined in the previous step are included in your stack definition file as well. 
1. In your source code repo, create a working directory and add the `stack_definition.json` file to that directory. For more information, see [Creating your source repo](/docs/secure-enterprise?topic=secure-enterprise-create-da#source-repo-da).
1. In your `ibm_catalog.json` manifest file, add the following code snippet into the `flavors` section:

   For example, if your deployable architecture is called `Dinner` and you want to create a variation of that architecture, you might call it `Steak and potatoes with broccoli` as shown in the following example.

   ```json
   "flavors": [
    {
        "label": "Steak and potatoes with broccoli",
        "name": "steak-potatoes-broccoli-variation",
        "working_directory": "./steak-potatoes-broccoli",
        "install_type": "fullstack"
    }
   ]
   ```
   {: codeblock}

If you haven't done so already, create your Git release and start [onboarding to a private catalog](/docs/secure-enterprise?topic=secure-enterprise-onboard-da) to create a catalog tile that you can share with others.
