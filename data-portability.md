---

copyright:
  years: 2024
lastupdated: "2024-11-15"

keywords: projects, data portability

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}


# Understanding data portability for projects
{: #data-portability}



Data portability involves a set of tools and procedures that enable you to export the digital artifacts that are needed to implement similar workload and data processing on different service providers or on-premises software. It includes procedures for copying and storing the service customer content, including the related configuration that is used by the service to store and process the data, in your location.
{: shortdesc}

## Responsibilities
{: #data-portability-responsibilities}

{{site.data.keyword.cloud_notm}} services provide interfaces and instructions to guide you through the process of copying and storing service customer content, including the related configuration, in your selected location.

You're responsible for the use of the exported data and configuration for data portability to other infrastructures, which includes:

- The planning and execution for setting up alternative infrastructure on different cloud providers or on-premises software that provide similar capabilities to the {{site.data.keyword.IBM_notm}} services.
- The planning and execution for the porting of the required application code on the alternative infrastructure, including the adaptation of your application code, deployment automation, and so on.
- The conversion of the exported data and configuration to the format that's required by the alternative infrastructure and adapted applications.

For more information about your responsibilities for projects, see [Understanding your responsibilities when using projects](/docs/secure-enterprise?topic=secure-enterprise-responsibilities-projects).

## Data export procedures
{: #data-portability-procedures}

The {{site.data.keyword.cloud_notm}} Projects service provides the mechanisms to export your content that's uploaded, stored, and processed when you use the service. In addition, the {{site.data.keyword.cloud_notm}} Projects service provides mechanisms to export settings and configurations that are used to process your content. You can do so by [exporting the project JSON](/docs/secure-enterprise?topic=secure-enterprise-json-project&interface=ui#json-export). 

## Exported data formats
{: #data-portability-data-formats}



The exported data for a project is stored as a JSON file called `project.json`. For more information about this file and its contents, go to [Project JSON](/docs/secure-enterprise?topic=secure-enterprise-json-project&interface=ui).



## Data ownership
{: #data-portability-ownership}

All exported data is classified as customer content. Apply the full customer ownership and licensing rights, as stated in the [IBM Cloud Service Agreement](https://www.ibm.com/terms/?id=Z126-6304_WS){: external}.
