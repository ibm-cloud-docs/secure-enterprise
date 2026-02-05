---

copyright:
  years: 2023, 2026
lastupdated: "2026-02-05"

keywords:

subcollection: secure-enterprise

---

{{site.data.keyword.attribute-definition-list}}

# Enabling continuous compliance in your application software development cycle
{: #compliant-software-development}

Taking advantage of DevSecOps to automate the integration of security at every phase of your enterprise application development lifecycle addresses security issues as they emerge, making them easier, faster, and less expensive to fix. By using the [DevSecOps Application Lifecycle Management](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-overview) deployable architecture, you can take advantage of the two main benefits of using DevSecOps, which are speed and security.
{: shortdesc}

Throughout the development cycle, the code is reviewed, scanned, and tested for security issues. This integrated security allows your team to catch issues early, and reduces duplicative reviews and unnecessary rebuilds, resulting in more secure code that is delivered faster. As DevSecOps integrates vulnerability scanning and patching into the development and release cycle, the ability to identify and patch common vulnerabilities and exposures (CVE) is increased. This limits the window a threat actor has to take advantage of vulnerabilities in public-facing production systems. Additionally, continuous compliance with periodic scanning of applications in production is included.

Implementing automated scanning and testing can ensure that incorporated software dependencies are at appropriate patch levels, and confirm that software passes security unit testing. Plus, it can test and validate code for security with static and dynamic analysis. Evidence of scan results is securely stored, collected, and summarized as part of an automated change management process that gates deployments, before changes are promoted to production.

Review the following sections to get an overview of the essential steps for ensuring that your application development lifecycle is secure by using the {{site.data.keyword.cloud_notm}} DevSecOps tools.

## Setting up your secure software development architecture
{: #setup-devsecops}

The DevSecOps Application Lifecycle Management deployable architecture streamlines the process to set up your secure software supply chain by using a set of continuous integration (CI), continuous deployment (CD), and continuous compliance (CC) toolchain templates. These templates use a collection of tool integrations and customizable reference Tekton pipelines for build, scan, test, change management, and deploy applications.

The Tekton pipelines provide a framework of custom scripts that you can use to ensure the compliant and automated orchestration of code and configuration changes. The pipelines maintain a GitOps release inventory, while they collect and store evidence that can be used to generate auditable change requests. Additionally, the continuous compliance pipeline periodically scans the deployed artifacts and associated source code repositories for vulnerabilities.

For more information about configuring the deployable architecture to fit your needs, see the [DevSecOps deployment guide](/docs/devsecops-alm?topic=devsecops-alm-devsecops-alm-overview). You can use a [project](/docs/secure-enterprise?topic=secure-enterprise-setup-project) to deploy this architecture in all of your application development environments, so that your development team can take a shift-left approach by identifying security risks and exposures early, so that they are addressed before code ever reaches production.


## Ensuring continuous compliance with the CC pipeline
{: #cc-pipeline}

The [CI](/docs/devsecops?topic=devsecops-cd-devsecops-ci-pipeline)/[CD](/docs/devsecops?topic=devsecops-cd-devsecops-cd-pipeline-overview) pipelines ensure that the application code that your team develops is secure and free of vulnerabilities before code is pushed to production. Once your code reaches production, you can use the CC pipeline to continuously scan your production code for new vulnerabilities. The [CC pipeline](/docs/devsecops?topic=devsecops-devsecops-cc-pipeline) can be triggered manually or periodically by using triggers.

The CC pipeline scans existing deployed artifacts and their source repositories independent of your deployment schedule. It runs the static scans and dynamic scans on the Application Source Code, detect secrets in Git repos, Bill Of Materials (BOM) check, CIS check, and Vulnerability Advisor scan.


## Managing issues and collecting evidence to be audit-ready
{: #tracking-compliance-issues}

After scanning and running checks on artifacts and source repositories, the pipeline creates a new incident issue or updates the existing incident issues in the incident repository. Finally, using these issues and the results, the pipeline collects evidence and summarizes the evidence.

Two types of issues are reported from your CI and CC pipelines: incident issues and nonincident issues. Incident issues can arise due to vulnerabilities or CVEs found inside the code or the deployed artifacts, and nonincident issues do not arise from vulnerabilities, but rather represent a deviation from the compliance posture, for example, unit test failures and branch protection check failures. For more information about managing issues, see [Processing incident and nonincident issues](/docs/devsecops?topic=devsecops-issue-processing) and [Managing incident issues](/docs/devsecops?topic=devsecops-incident-issues)

Compliance evidence creates the trail that auditors look for during a compliance audit. One of the goals of DevSecOps is automated evidence generation and storage in auditable change requests and durable evidence lockers. For more information, see [Evidence](/docs/devsecops?topic=devsecops-devsecops-evidence).
