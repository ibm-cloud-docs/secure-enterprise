---

copyright:
  years: 2025
lastupdated: "2025-08-08"

keywords: onboard, catalog management, private catalog, catalog manifest, software, automation, metadata, deployable architecture, stacking deployable architecture, extending, stack

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Extending a deployable architecture during onboarding
{: #extend-da}

As you onboard a deployable architecture to a private catalog, you can choose to extend it by stacking it with other architectures. When you add those other architectures, you specify whether they are required to deploy your architecture as intended, or if they can be used optionally to extend the use case. When you finish onboarding, the result is a robust end-to-end solution that is made of multiple deployable architectures that work together.  
{: shortdesc}

Are the deployable architectures that you want to stack together already available in a catalog? If so, you can [stack those architectures in a project](/docs/secure-enterprise?topic=secure-enterprise-config-stack&interface=ui) and onboard that solution as a separate catalog entry. Currently, when you stack deployable architectures in a project, including optional or interchangeable architectures is not supported. So, that approach works best if all of the architectures you want to stack together are required to deploy the end-to-end solution. 

## Why extend a deployable architecture? 
{: #extend-why}

You can extend a deployable architecture for different reasons. For example, if the architecture that you're onboarding requires a dependency, you can stack it along with the architecture it depends on. If the dependency is listed as required, then it's included along with your deployable architecture when a user configures and deploys it. 

Similarly, to encourage security by default for a deployable architecture that you're onboarding, you can stack other architectures with it that satisfy security needs. If the other deployable architectures are included as recommended, it helps ensure that users in your organization deploy the architecture in a specific way with those security guardrails in place. Recommended architectures are included by default, but users can remove them. 

You can also extend a deployable architecture to provide more customization options for users so they can take advantage of an extended use case. Maybe your architecture works well with another deployable architecture that isn't needed to meet a dependency or satisfy compliance. You can add the deployable architecture as optional, and users can choose to include it when they add your deployable architecture to a project. 

## Before you begin
{: #prereq}

As you complete the steps to [onboard your deployable architecture to a private catalog](/docs/secure-enterprise?topic=secure-enterprise-onboard-da#add-catalog), you're prompted to configure the version details. Go to **Extend your architecture** to stack other deployable architectures along with the one that you're onboarding. 

If you already onboarded your deployable architecture, edit the version in your private catalog, then go to **Extend your architecture**. 

Prefer to work with code? You can specify details about other deployable architectures you want to stack with the one you're onboarding in the catalog manifest file. Set the `dependency_version_2` property to `true` and use the `dependencies` array to specify which architectures you want to include with the one you're onboarding. For more information about how to structure the catalog manifest, see [Locally editing your manifest file](/docs/secure-enterprise?topic=secure-enterprise-manifest-values).
{: tip}

## Adding deployable architectures
{: #add-arch}

As you onboard a deployable architecture, you can extend it by stacking it with other architectures. Specify a relationship when you add other architectures, then define variables for users to configure across the architectures that you're stacking together. Complete the following steps: 

1. As you onboard a deployable architecture to a catalog, in the **Extend your architecture** section, click **Add**.
1. Use the **Product** menu to select the deployable architecture you want to add. You can select any deployable architecture that your account has access to across your catalogs. 
1. Select a **Relationship** option for the deployable architecture that you're adding. The architecture is either required and can't be removed. It's optional and users can select to include it or not. Or, it's recommended and included by default, though users can choose to remove it. For more information, go to [Specifying a relationship](#relationship). 
1. Optional: If you're adding two or more deployable architectures that are interchangeable depending on the user's preferences, select them in the **Extend your architecture** table, and click **Group as swappable**. Users can select which architecture they want to use based on their needs. 
1. When you're done adding deployable architectures, click **Next** > **Define variables** to define variables for your users. From there, you can connect the deployable architectures together so users can configure inputs in a single interface, instead of configuring architectures one by one. For more information, go to [Defining variables for your users](#define-variables).

### Specifying a relationship 
{: #relationship}

When you stack deployable architectures along with the one that you're onboarding, you're asked to specify a **Relationship**. What's the relationship between the deployable architecture that you're adding and the overall solution that you're onboarding to your private catalog? You can choose from three options: **Required**, **Optional**, and **Recommended**. The following table describes and explains how each option impacts customizing the end-to-end solution when a user adds the architecture to a project from the private catalog: 

| Option | Description |
|----------|---------|
| Required | Included when a user adds your architecture to a project. The user is not able to remove required architectures because they are necessary for your overall solution to function as intended. |
| Optional | Not included by default. Users can select to include optional architectures if they want to take advantage of an extended use case.|
| Recommended | Included by default, but users can remove it if they want to.  \n  \n For example, perhaps you want to encourage secure deployments by adding {{site.data.keyword.secrets-manager_full_notm}} as recommended to your deployable architecture. {{site.data.keyword.secrets-manager_short}} is then included by default when a user adds your deployable architecture to a project. However, if they have another tool for managing secrets, or otherwise don't require {{site.data.keyword.secrets-manager_short}}, the user can choose to exclude it. |
{: caption="Relationship options explained" caption-side="bottom"}

### Defining variables for your users
{: #define-variables}

After you extend your deployable architecture by stacking other architectures with it, you need to define variables for your users. The end goal is to link the deployable architectures together so users can configure inputs in a single interface, instead of configuring architectures one by one. Consider the following questions: 

* Which inputs within the deployable architectures do users need to configure? These inputs need to be added to your product so users can configure them without editing the individual architectures one by one. Select **Add to product** to do so.
* Which deployable architectures need to be linked together with references to inputs or outputs? For example, if an input in one architecture depends on the output from another, add a reference in the input to connect it to the output. Select **Reference a variable** to do so. 
   
   Only inputs that you add to your product can reference an output. If you added an architecture and one of its inputs needs to reference an output from another architecture, select **Add to product** for that input. Then, select **Reference a variable** for the input that you added. From there, you can select the architecture that contains the output. Select **Output** as the variable type, and then select the name of the output you want the input to reference.
   {: note}

* Are all required inputs resolved for users? For required inputs that aren't added to the product, provide a fixed value or a reference to resolve those inputs. Doing so connects the architectures within this end-to-end solution and makes it easier for users to configure and deploy it. 

   Default values can't be provided in the console. Use the `default_value` property within the `configuration` array in the catalog manifest file to specify default values. For more information, see [Locally editing your manifest file](/docs/secure-enterprise?topic=secure-enterprise-manifest-values).
   {: important}

* Do users need any extra information about a variable to help them configure it? When you add an input variable to your product, you can change the way that the input appears to users. Often, it can be useful to provide a new name for a variable or a description, especially if two similar variables are included. For example, if the architecture that you're onboarding contains a `region` input, and you add a `region` input from an infrastructure architecture as well, consider naming the input `region-infrastructure` when you add it to your product. A meaningful name helps identify where the input is used and makes it easier to configure.

Any inputs that you added to your product are included in the input variables table. If you need to remove a variable that you added, select it and click **Delete**. 

## Next steps
{: #next-steps}

After the variables are defined for your users, [continue onboarding your deployable architecture](/docs/secure-enterprise?topic=secure-enterprise-onboard-da). When you're done, users who have access to your private catalog are able to add your deployable architecture to a project. When they do so, they can customize it by selecting which architectures they want to include based on what you specified during the onboarding process.
