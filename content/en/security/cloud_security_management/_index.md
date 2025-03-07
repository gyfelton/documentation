---
title: Cloud Security Management
kind: documentation
aliases:
  - /security_platform/cloud_security_management/
further_reading:
  - link: "https://app.datadoghq.com/release-notes?category=Security%20%26%20Compliance"
    tag: "Release Notes"
    text: "See What's New in Datadog Security Compliance"
  - link: "/security/misconfigurations/setup"
    tag: "Documentation"
    text: "Start tracking misconfigurations with CSM Misconfigurations"
  - link: "/security/threats/setup"
    tag: "Documentation"
    text: "Uncover kernel-level threats with CSM Threats"
  - link: "https://www.datadoghq.com/blog/cyber-attack-simulation-with-stratus-red-team/"
    tag: "Blog"
    text: "Elevate AWS threat detection with Stratus Red Team"
  - link: "https://www.datadoghq.com/blog/kubernetes-security-best-practices/"
    tag: "Blog"
    text: "Best practices for securing Kubernetes applications"
  - link: "https://www.datadoghq.com/blog/workload-security-evaluator/"
    tag: "Blog"
    text: "Run Atomic Red Team detection tests in container environments with Datadog’s Workload Security Evaluator"
  - link: "https://www.datadoghq.com/blog/security-context-with-datadog-cloud-security-management/"
    tag: "Blog"
    text: "Add security context to observability data with Datadog Cloud Security Management"
  - link: "https://www.datadoghq.com/blog/security-labs-ruleset-launch/"
    tag: "Blog"
    text: "Fix common cloud security risks with the Datadog Security Labs Ruleset"
  - link: "https://www.datadoghq.com/blog/securing-cloud-native-applications/"
    tag: "Blog"
    text: "Best practices for application security in cloud-native environments"
  - link: "https://www.datadoghq.com/blog/custom-detection-rules-with-datadog-cloud-security-management/"
    tag: "Blog"
    text: "Customize rules for detecting cloud misconfigurations with Datadog Cloud Security Management"
algolia:
  tags: ['inbox']
---

Datadog Cloud Security Management (CSM) delivers real-time threat detection and continuous configuration audits across your entire cloud infrastructure, all in a unified view for seamless collaboration and faster remediation. Powered by observability data, security teams can determine the impact of a threat by tracing the full attack flow and identify the resource owner where a vulnerability was triggered.

CSM leverages the Datadog Agent and platform-wide cloud integrations and includes:

- [**Threats**][1]: Monitors file, network, and process activity across your environment to detect real-time threats to your infrastructure.
- [**Misconfigurations**][2]: Tracks the security hygiene and compliance posture of your production environment, automates audit evidence collection, and enables you to remediate misconfigurations that leave your organization vulnerable to attacks.
- [**Identity Risks**][8]: Provides in-depth visibility into your organization's AWS IAM risks and enables you to detect and resolve identity risks on an ongoing basis.
- [**Vulnerabilities**][9]: Leverages infrastructure observability to detect, prioritize, and manage vulnerabilities in your organization's containers and hosts.

{{< img src="security/csm/csm_overview.png" alt="Cloud Security Management in Datadog" width="100%">}}

## Prioritize and remediate important security issues

The **Security Inbox** on the [CSM Overview][4] shows a list of prioritized security issues that require immediate attention. Security issues are a consolidation of other security detections and resource attributes such as being publicly accessible, attached to privileged roles, and residing in production environments.

The order in which security issues are prioritized is based on the following criteria:

- Higher severity issues are listed first
- Whether an issue has a threat attached to it
- Number of related risks (publicly accessible, production environment, misconfiguration, vulnerability)
- Number of resources impacted
- Discovered date (newer issues are listed first)

Remediating security issues can meaningfully improve your organization's security. Use the **Security Inbox** to prioritize which security issues to resolve, either by fixing the underlying issues or by muting the issue.

{{< img src="security/csm/security_inbox.png" alt="The Security Inbox on the CSM overview shows prioritized issues for remediation" width="100%">}}

## Track your organization's health

Available for [CSM Misconfigurations][2], the [security posture score][5] helps you track your organization's overall health. The score represents the percentage of your environment that satisfies all of your active out-of-the-box cloud and infrastructure compliance rules.

Improve your organization's score by remediating findings, either by resolving the underlying issue or by muting the finding.

{{< img src="security/csm/health_scores.png" alt="The posture score on the CSM overview page tracks your organization's overall health" width="100%">}}

## Explore and remediate issues

Use the [Issues page][7] to review and remediate your organization's detections and findings. View detailed information about a detection, including guidelines and remediation steps. [Send real-time notifications][6] when a threat is detected in your environment, and use tags to identify the owner of an impacted resource.

{{< img src="security/cws/threats_page.png" alt="CSM Threats page" width="100%">}}

## Next steps

To get started with CSM, navigate to the [**Security** > **Setup**][3] section in Datadog, which has detailed information on how to set up and configure CSM.

## Further reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /security/threats/
[2]: /security/misconfigurations/
[3]: https://app.datadoghq.com/security/configuration
[4]: https://app.datadoghq.com/security/csm
[5]: /glossary/#posture-score
[6]: /security/notifications/
[7]: https://app.datadoghq.com/security?product=cws
[8]: /security/identity_risks/
[9]: /security/infrastructure_vulnerabilities/
