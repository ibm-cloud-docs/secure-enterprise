---

copyright:
  years: 2023, 2025
lastupdated: "2025-05-27"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability for projects
{: #ha-dr}

{{site.data.keyword.Bluemix}} projects is a general availability (GA) service that is offered in multiple regions: Dallas, Washington, Frankfurt, and London. Each location has three different data centers for redundancy. The data for each location is kept in the three data centers near that location. If all of the data centers in a location fail, the {{site.data.keyword.cloud_notm}} Projects service for that location becomes unavailable.

See [ensure zero downtime](/docs/resiliency?topic=resiliency-ha-redundancy#zero-downtime) to learn more about the disaster recovery standards. You can also find information about {{site.data.keyword.cloud_notm}} [Service Level Objectives](/docs/resiliency?topic=resiliency-slo).

## Responsibilities
{: #ha-responsibilities}

For more information about high availability and disaster recovery and your responsibilities when you use projects, see [Understanding your responsibilities when using projects](/docs/secure-enterprise?topic=secure-enterprise-responsibilities-projects).

## What level of availability do I need?
{: #ha-level}

You can achieve high availability on different levels in your IT infrastructure and within different components of your cluster. The level of availability that is correct for you depends on several factors, such as your business requirements, the service level agreements (SLAs) that you have with your customers, and the resources that you want to expend.

## What level of availability does {{site.data.keyword.cloud_notm}} offer?
{: #ha-service}

The level of availability that you set up for your cluster impacts your coverage under the {{site.data.keyword.cloud_notm}} high availability service level agreement terms.

Service level objectives (SLOs) describe the design points that the {{site.data.keyword.cloud_notm}} services are engineered to meet. Projects are designed to achieve the following availability target.

| Availability target | Target Value   |
|---|---|
|  Availability % | 99.999% |
{: caption="SLO for projects " caption-side="bottom"}

The SLO is not a warranty and {{site.data.keyword.IBM_notm}} will not issue credits for failure to meet an objective. Refer to the SLAs for commitments and credits that are issued for failure to meet any committed SLAs. For a summary of all SLOs, see [{{site.data.keyword.cloud_notm}} service level objectives](/docs/resiliency?topic=resiliency-slo).


## Locations
{: #ha-locations}

For more information about service availability within regions and data centers, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).
