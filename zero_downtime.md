---

copyright:
  years: 2023
lastupdated: "2025-05-27"

keywords: HA for IBM projects, DR for IBM projects, high availability for IBM projects, disaster recovery for IBM projects, failover for IBM projects, BC for IBM projects, DR for IBM projects, business continuity for IBM projects, disaster recovery for IBM projects

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Understanding business continuity and disaster recovery for projects
{: #bc-dr}

Disaster recovery involves a set of policies, tools, and procedures for returning a system, an application, or an entire data center to full operation after a catastrophic interruption. It includes procedures for copying and storing an installed system's essential data in a secure location, and for recovering that data to restore normalcy of operation.
{: shortdesc}

## Responsibilities
{: #bc-dr-responsibilities}

For more information about your responsibilities when using projects, see [Shared responsibilities for projects](/docs/secure-enterprise?topic=secure-enterprise-responsibilities-projects).

### Disaster recovery strategy
{: #bc-dr-strategy}

{{site.data.keyword.cloud_notm}} has business continuity plans in place to provide for the recovery of services within hours if a disaster occurs. You are responsible for your data backup and associated recovery of your content.

{{site.data.keyword.cloud_notm}} performs regular electronic backups of project data with Recovery Time Objective (RTO) and Recovery Point Objective (RPO) of hours as documented in the [{{site.data.keyword.cloud_notm}} Disaster Recovery Plan](/docs/resiliency?topic=resiliency-ha-redundancy#disaster-recovery). Projects don't replicate data outside of a region, except for backup data. When possible, backup data is kept within the data centers of a country but data is always kept within a geography. European data does not leave the EU.

| Disaster recovery objective | Target Value   |
|---|---|
|  RPO | 1 hour  |
|  RTO | 4 hours   |
{: caption="RPO and RTO for projects" caption-side="top"}

## Locations
{: #ha-locations-bc-dr}

For more information about service availability within regions and data centers, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region){: external}.
