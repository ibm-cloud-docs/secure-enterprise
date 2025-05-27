---

copyright:
  years: 2023, 2025
lastupdated: "2025-05-27"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}


# Achieving continuous compliance as an enterprise
{: #continuous-compliance}

With continuous security and compliance at the core of {{site.data.keyword.cloud}}'s platform, you can find compliant-by-default infrastructure for hosting your regulated workloads in the cloud. From deployable architectures for secure infrastructure and DevSecOps pipelines to continuous validation through {{site.data.keyword.compliance_short}}, you can be sure that your organization is secure and compliant through every stage of development.
{: shortdesc}

![Security and compliance for regulated workloads on {{site.data.keyword.cloud_notm}}](https://cdnapisec.kaltura.com/html5/html5lib/v2.101/mwEmbedFrame.php/p/1773841/uiconf_id/27941801/entry_id/1_oh70qmd5?wid=_1773841&iframeembed=true&entry_id=1_oh70qmd5){: video output="iframe" data-script="#regulatedworkload-video-transcript" id="mediacenter-player" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen}

## Video transcript
{: #regulatedworkload-video-transcript}
{: notoc}

Hi, I’m Chris Mitchell, an STSM and Architect for IBM Cloud.

Today’s enterprises are challenged to manage risk and compliance, contain costs, and unlock innovation.

Here at IBM Cloud, we understand the complexity and challenges in optimizing for security and compliance, reducing lead time for deploying new systems, and successfully running secure and compliant infrastructure.

IBM Cloud is the first secure and compliant-by-default cloud for regulated industries. It is specifically designed to reduce risk and has been architected to be the hub for enterprise IT security and compliance.

With the integration of security and compliance through our catalog of deployable architectures and other tools, you can streamline the process of defining compliance profiles, implementing secure infrastructure, and assessing the compliance of your enterprise workloads.

To define your goals for assessing your workloads, you can use one of IBM Cloud’s predefined compliance profiles, like IBM Cloud Framework for Financial Services, or you can create your own custom profile in the IBM Cloud Security and Compliance Center.

Then, depending on your defined compliance standards and infrastructure needs, you can take advantage of automated well-documented architecture patterns available in the catalog that are based on the IBM Cloud Framework for Financial Services.

The IBM Cloud Framework for Financial Services reference architectures are based on a set of best practices for cloud infrastructure and software as a service.

When you provision infrastructure using these deployable architectures, you meet the defined controls and stay within the constraints of the IBM Cloud Framework for Financial Services profile. This can help take the guesswork out of creating secure and compliant patterns for infrastructure in your enterprise.

Each deployable architecture can be deployed and configured by using an IBM Cloud project. Projects support a simplified process for deploying repeatable infrastructure patterns with integrated compliance checks.

With projects, you can review changes to infrastructure costs and review compliance checks based on your configuration of the architecture prior to deployment, which helps build in transparency and trust along your journey.

Finally, when you’re ready to assess the compliance of your deployed cloud resources against your defined standards, you can use the Security and Compliance Center to evaluate resources in your accounts.

Whether it’s a one-time evaluation or recurring scans, you can prepare for audits by having a clear view of the security and compliance posture of your enterprise.

And, to ensure continuous compliance of your cloud resources, you can set up notifications to get alerted when an issue is found so your team can stay on top of remediating any issues.

With the architectures and security tools available through IBM Cloud, you can define your compliance goals from the start, deploy secure solutions with automation, and stay compliant, all while managing your resources at scale.

To learn more about achieving continuous compliance as an enterprise, visit the documentation. https://cloud.ibm.com/docs/secure-enterprise

## Reviewing available controls
{: #review-controls}

As a regulated business, there are specific standards that apply to your industry that you need to prove compliance to. In {{site.data.keyword.compliance_short}}, you can view the [profiles](/docs/security-compliance?topic=security-compliance-predefined-profiles) that are offered by {{site.data.keyword.IBM}} that can meet your requirements. For example, if you are a financial institution, you might want to use the {{site.data.keyword.cloud_notm}} for Financial Services library. If you don't see the control set that you are looking for, you can always create a custom one. After you have reviewed the libraries, you can pull specific compliance controls into a profile that can be evaluated.

During your investigation phase, you might also want to review the available infrastructure [deployable architectures](/catalog#referencearchitecture) in the catalog. {{site.data.keyword.cloud_notm}} has created automation for the deployment of common architectural patterns that combine one or more cloud resources and designed for easy scalability and modularity. You can review the components of the architecture and the level of compliance each deployable architecture meets by reviewing the details directly in the catalog detail pages, and you can customize these architectures to meet your exact needs.

![IBM Cloud catalog showing deployable architecture tiles](images/catalog.svg){: caption="IBM Cloud catalog showing deployable architecture tiles" caption-side="bottom"}

## Deploying your infrastructure and applications
{: #deploy}

Now that you've evaluated what is available to you on {{site.data.keyword.cloud_notm}} and you know what needs to be customized or what can be used as is, it's time to start working! The engineers on your teams can start by getting your infrastructure and application workloads ready to deploy.

Your team can use [projects](/docs/secure-enterprise?topic=secure-enterprise-config-project) to help organize your enterprise deployments and ensure that commit checks, vulnerability scans, and cost estimations are completed as deployable architectures are configured. Within the context of a project, you can easily deploy infrastructure resources from approved, compliant {{site.data.keyword.cloud_notm}} or private catalog offerings by using a deployable architecture. By using a predefined deployable architecture, you can be sure that you are meeting the compliance standards that the architecture is associated with. Or, you can onboard your own and specify the controls within {{site.data.keyword.compliance_short}} that your architecture is compliant with.

Before you deploy an architecture, a validation check is run on your configuration for both compliance and risk so that you can address any issues that are found. You can view the logs through the {{site.data.keyword.bplong}} service to determine which resources are affected and consider whether to fix or override the flagged issue and move forward.

After your infrastructure is deployed and your [DevSecOps toolchains](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-overview) are configured, you're ready to deploy your app by using the DevSecOps continuous integration and continuous deployment pipelines. These pipelines can help your enterprise to shift left and reduce the possibility of human error or introduction of new vulnerabilities before code ever reaches production to help mitigate any major security or financial risks.

![A visual representation of a process that includes continuous integration, deployment, and compliance.](images/cd.svg){: caption="Continuous integration, deployment, and compliance" caption-side="bottom"}


## Staying compliant
{: #stay-compliant}

After you deploy resources that you know are compliant, you can ensure that you remain compliant in two ways. First, by validating your resource configurations with {{site.data.keyword.compliance_short}}. {{site.data.keyword.compliance_short}} regularly scans your configurations of the resources in your account to ensure that there hasn't been a drift in compliance.

![A visual representation of the Security and Compliance Center dashboard](images/dashboard.svg){: caption="Example Security and Compliance Center dashboard" caption-side="bottom"}

Second, you can ensure that you're deploying your code by using DevSecOps pipelines. When you use the continuous compliance toolchain, scans are reexecuted against your current production code artifacts. This continuous scanning helps to ensure that any code that is deployed in to production is checked for the latest known vulnerabilities allowing for regular revalidation of deployed code and remediation of any new issues that are discovered since the last scan.

Staying compliant and audit-ready is of the utmost importance. {{site.data.keyword.compliance_short}} allows you to define the controls you need to meet by using pre-defined or custom profiles, attach the profiles to a group of resources, or scope, and [perform regularly scheduled evaluations](/docs/security-compliance?topic=security-compliance-scopes&interface=ui). As evaluations are completed, the results are displayed in a dashboard so you can get an overarching view of your current compliance posture against the controls that are important for your use case and download compliance reports. Your security and compliance managers can also choose to set up notifications to get alerted when an issue is found so that it can be remediated quickly. In addition, {{site.data.keyword.compliance_short}} can collect evidence from your DevSecOps pipeline runs on your application code so that it can show a complete view of your compliance on {{site.data.keyword.cloud_notm}}.
