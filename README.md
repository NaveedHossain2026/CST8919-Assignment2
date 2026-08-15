# Cloud Service Alternatives Report
### CST8919 – DevOps Security and Compliance | Assignment 2
**Author:** _[Your Name]_
**Date:** August 15, 2026

---

## Introduction

Throughout this course, we used Microsoft Azure to implement identity, monitoring, policy, and threat-detection controls as part of a broader DevSecOps workflow. Although cloud security concepts are transferable across cloud providers, the specific services, product names, features, and pricing models vary between platforms. This report maps five core Azure services to their closest equivalents in Amazon Web Services (AWS) and Google Cloud Platform (GCP). It compares these services based on their features, security and compliance capabilities, pricing models, and integration with DevSecOps practices.



---

## Quick-Reference Comparison Table

| # | Category | Azure | AWS | GCP |
|---|----------|-------|-----|-----|
| 1 | Identity & Access Management (SSO/IAM) | Microsoft Entra ID (formerly Azure AD) | AWS IAM + AWS IAM Identity Center (formerly AWS SSO) | Google Cloud IAM + Cloud Identity |
| 2 | Monitoring & Log Analytics | Azure Monitor & Log Analytics | Amazon CloudWatch + CloudTrail | Google Cloud Operations Suite (Cloud Logging & Cloud Monitoring) |
| 3 | Policy & Governance | Azure Policy | AWS Config + AWS Organizations (SCPs) | Organization Policy Service |
| 4 | Cloud Security Posture / Workload Protection | Microsoft Defender for Cloud | AWS Security Hub + Amazon GuardDuty + Amazon Inspector | Security Command Center (SCC) |
| 5 | SIEM / SOAR | Microsoft Sentinel (formerly Azure Sentinel) | Amazon Security Lake (+ GuardDuty/Detective, or 3rd-party SIEM) | Google Security Operations (SecOps), formerly Chronicle |

---

## 1. Identity & Access Management

## Overview

| **Aspect** | **Microsoft Entra ID (formerly Azure AD)** | **AWS IAM / IAM Identity Center** | **Google Cloud IAM / Cloud Identity** |
|---|---|---|---|
| **Primary role** | Full Identity Provider (IdP) that authenticates users and manages identities and access to applications and cloud resources. | Primarily an access-management and authorization system for AWS resources and APIs. IAM Identity Center provides centralized workforce authentication and access. | IAM provides authorization for GCP resources, while Cloud Identity provides identity, user, and directory management. |

Microsoft Entra ID (formerly Azure AD) centralizes identity at the tenant level and serves as a single directory for an entire organization. AWS IAM Identity Center instead sits above individual AWS accounts to unify workforce access across a multi-account AWS Organization, while raw AWS IAM manages permissions inside a single account. Google Cloud IAM applies permissions at the project level with inheritance through folders and organizations, and Cloud Identity supplies the directory/SSO layer comparable to Entra ID.

### Core Features
- **Microsoft Entra ID (formerly Azure AD):** Provides Conditional Access, Privileged Identity Management (PIM) for just-in-time access, Identity Protection for risk-based sign-in analysis, B2B/B2C guest access, and Application Proxy for accessing on-premises applications.
- **AWS IAM / Identity Center:** Offers detailed JSON policies that can control individual API actions, roles and service-linked roles, permission sets for managing access across multiple AWS accounts, and federation with external identity providers using SAML/SCIM.
- **Google Cloud IAM / Cloud Identity:** Provides predefined and custom IAM roles, context-aware access, organization policy inheritance, Google Workspace integration, and passwordless sign-in.

### Security & Compliance
All three platforms support major compliance standards such as SOC 2, ISO 27001, FedRAMP, PCI DSS, and HIPAA. Entra ID provides built-in Conditional Access and risk-based Identity Protection. AWS IAM offers detailed and flexible policies but can be more complex and may require additional PAM tools for just-in-time access. Google Cloud focuses on clear audit logging and straightforward per-user licensing.

### Pricing Model
- **Entra ID:** The free tier is included with Azure and Microsoft 365. Premium P1/P2 add features such as Conditional Access, PIM, and Identity Protection, with pricing typically around $6–$9 USD per user/month.
- **AWS IAM:** IAM and IAM Identity Center are free to use. Costs mainly come from the AWS services and resources being managed.
- **GCP:** Cloud IAM is free. Cloud Identity has a free basic tier, while the Premium tier charges per user/month for advanced device and application management.

### Integration for DevSecOps
All three platforms support CI/CD integration using service principals, roles, or service accounts. They also support Infrastructure as Code (IaC) tools such as Terraform, ARM, CloudFormation, and Deployment Manager for managing access. In multi-cloud environments, these identity systems can also be connected through federation, allowing users to access multiple cloud platforms with a single identity.

---

## 2. Monitoring & Log Analytics

### Overview

| **Aspect** | **Azure Monitor & Log Analytics** | **Amazon CloudWatch (+ CloudTrail)** | **Google Cloud Operations Suite** |
|---|---|---|---|
| **Purpose** | Unified metrics, logs, and traces for Azure and hybrid resources, with Kusto Query Language (KQL) for log analysis. | Metrics, logs, dashboards, and alarms through CloudWatch, combined with CloudTrail for API activity and governance auditing. | Unified Cloud Logging and Cloud Monitoring for logs, metrics, dashboards, and application performance monitoring. |

### Core Features

- **Azure Monitor / Log Analytics:** Centralized Log Analytics workspaces, KQL queries, Application Insights for application performance monitoring, Workbooks for dashboards, and Alerts/Action Groups.
- **CloudWatch / CloudTrail:** Custom metrics and dashboards, Logs Insights for log queries, alarms that can trigger automated actions, and CloudTrail for API activity and governance auditing.
- **GCP Operations Suite:** Cloud Logging with structured log exports to services such as BigQuery and Pub/Sub, Cloud Monitoring dashboards and uptime checks, and Cloud Trace and Cloud Profiler for application performance monitoring.

### Security & Compliance

All three platforms provide monitoring services that support workloads operating under major compliance frameworks such as SOC 1/2/3, ISO 27001, FedRAMP, and HIPAA, although the exact compliance scope depends on the specific service and configuration. Log retention, access controls, integrity, and the ability to export logs to a SIEM are important security controls across all three platforms. Azure Monitor can integrate with Microsoft Sentinel, AWS CloudTrail provides log-file integrity validation, and Google Cloud provides centralized logging and export capabilities for security analysis and long-term retention.

### Pricing Model

All three platforms primarily use **consumption-based pricing**. Costs can depend on factors such as the volume of logs and metrics collected, data retention, queries, and alerts. Azure Monitor and Log Analytics charge primarily based on data ingestion and retention, while CloudWatch charges for logs, custom metrics, alarms, and other monitoring features. Google Cloud similarly uses usage-based pricing for logging, monitoring, and related observability services. Each provider also offers free usage allowances for selected monitoring features.

### Integration for DevSecOps

All three platforms integrate with Infrastructure as Code (IaC) and DevSecOps tools. Azure supports Bicep, ARM templates, and Terraform; AWS supports CloudFormation, CDK, and Terraform; and GCP supports Terraform and Google Cloud deployment tools. They can also integrate with Prometheus, Grafana, CI/CD pipelines, ticketing systems, and ChatOps platforms such as Slack and Microsoft Teams. Alerts can be used to automatically trigger incident response, remediation, or deployment rollback processes.

---

## 3. Policy & Governance

### Overview

| **Aspect** | **Azure Policy** | **AWS Config + Organizations (SCPs)** | **GCP Organization Policy Service** |
|---|---|---|---|
| **Purpose** | Enforces and audits organizational standards and compliance across Azure resources. | AWS Config continuously assesses resource configuration compliance, while Service Control Policies (SCPs) in AWS Organizations establish guardrails at the account and organizational-unit (OU) level. | Centrally restricts which GCP resource configurations are allowed through the resource hierarchy. |

### Core Features

- **Azure Policy:** Built-in and custom policy definitions, Initiatives (policy sets), `deny`, `audit`, and `deployIfNotExists` effects, compliance dashboards, and remediation tasks.
- **AWS Config / SCPs:** Config Rules, including managed and custom rules using Lambda, Conformance Packs for grouping multiple rules, SCPs that define maximum permissions for OUs and accounts, and configuration drift detection.
- **GCP Organization Policy:** Boolean and list constraints applied at the organization, folder, or project level, policy inheritance, dry-run mode, and custom constraints using Common Expression Language (CEL).

### Security & Compliance

All three platforms can support common security and compliance frameworks such as CIS Benchmarks, NIST 800-53, and PCI DSS. Azure Policy provides a Regulatory Compliance dashboard, while AWS Config Conformance Packs can group rules for continuous compliance monitoring. GCP Organization Policy focuses primarily on preventive guardrails and is commonly combined with Security Command Center (SCC) for broader security and compliance monitoring.

### Pricing Model

- **Azure Policy:** Standard policy evaluation is generally available without a separate policy charge. However, remediation actions and resources created through policies may incur normal Azure resource costs.
- **AWS Config:** Charges are based on configuration items recorded and rule evaluations. Additional costs may apply when using Conformance Packs and related services.
- **GCP Organization Policy:** The Organization Policy Service itself does not generally have a separate usage charge. Costs may occur indirectly when it is combined with services such as Security Command Center or BigQuery for reporting and analysis.

### Integration for DevSecOps

**Policy-as-code** is a common DevSecOps pattern across all three providers. Azure Policy definitions and Initiatives can be version-controlled and deployed through Bicep, ARM templates, or Terraform pipelines. AWS Config Rules and SCPs can be managed through CloudFormation or Terraform and validated as part of CI/CD workflows. GCP Organization Policies can be managed with Terraform and integrated into Cloud Build workflows. These approaches support **shift-left security**, allowing policies and infrastructure configurations to be validated before resources are deployed to production.

---

## 4. Cloud Security Posture Management & Workload Protection

### Overview

| **Aspect** | **Microsoft Defender for Cloud** | **AWS Security Hub + GuardDuty + Inspector** | **Google Security Command Center (SCC)** |
|---|---|---|---|
| **Purpose** | Combines Cloud Security Posture Management (CSPM) and Cloud Workload Protection (CWPP) across Azure, AWS, GCP, and on-premises environments. | Security Hub aggregates security findings and provides CSPM capabilities, while GuardDuty provides threat detection and Inspector performs vulnerability scanning. | Provides centralized asset inventory, vulnerability and misconfiguration detection, and threat detection for GCP resources. |

Microsoft Defender for Cloud provides multi-cloud security management, continuously assessing security posture across Azure, AWS, and GCP resources. It provides prioritized, benchmark-aligned recommendations and supports automated remediation. AWS Security Hub primarily aggregates and correlates findings from AWS security services such as GuardDuty and Inspector, making it particularly effective in AWS environments. Google Security Command Center combines asset inventory, security analytics, vulnerability detection, and threat detection, with integrations for external security and data-analysis tools.

### Core Features

- **Microsoft Defender for Cloud:** Secure Score, attack path analysis, Just-in-Time (JIT) VM access, container and Kubernetes protection, regulatory compliance dashboards, and multi-cloud connectors.
- **AWS Security Hub / GuardDuty / Inspector:** Security Finding Format (ASFF) for standardized findings, CIS/PCI DSS/NIST standards mapping, automated response through EventBridge, GuardDuty threat detection using sources such as VPC Flow Logs, CloudTrail, and DNS activity, and Inspector vulnerability scanning.
- **Google SCC:** Continuous asset discovery, Security Health Analytics for misconfigurations, vulnerability detection, Event Threat Detection, and integrations with SIEM/SOAR and third-party security tools.

### Security & Compliance

All three platforms support security and compliance monitoring aligned with frameworks such as CIS Benchmarks, PCI DSS, and NIST. Microsoft Defender for Cloud differentiates itself through its native multi-cloud capabilities, allowing organizations to monitor AWS and GCP resources from the Azure security platform. AWS Security Hub provides particularly strong integration within AWS environments, while Google SCC provides broader security capabilities for GCP workloads and can integrate with additional SIEM/SOAR services for advanced security operations.

### Pricing Model

- **Microsoft Defender for Cloud:** Provides a free foundational CSPM tier, while paid Defender plans are generally priced according to the protected resource, such as virtual machines, databases, storage, or Kubernetes workloads.
- **AWS Security Hub:** Pricing is based on factors such as security checks and findings ingested. Additional charges apply for services such as GuardDuty and Inspector based on their respective usage.
- **Google Security Command Center:** Provides a Standard tier with basic security capabilities, while higher-tier offerings provide additional vulnerability detection, threat detection, and security operations features based on usage and protected resources.

### Integration for DevSecOps

All three platforms provide APIs and integrations that can be incorporated into DevSecOps pipelines. Security findings can be used to **gate deployments**, such as failing a build when critical vulnerabilities are detected. Infrastructure-as-Code scanning can identify security issues before deployment, while automated remediation can be triggered through services such as Azure Logic Apps, AWS EventBridge with Lambda, or Google Cloud Workflows and functions. This supports a **shift-left security model**, where security checks are integrated throughout the development and deployment lifecycle.
---

## 5. SIEM / SOAR

### Overview
| | Microsoft Sentinel | AWS Security Lake (+ GuardDuty/Detective) | Google Security Operations (SecOps, formerly Chronicle) |
|---|---|---|---|
| **Purpose** | Cloud-native SIEM + SOAR built on Azure, ingesting logs across Microsoft and third-party sources | Centralizes security data in OCSF format in a customer-owned data lake; typically paired with GuardDuty/Detective or a third-party SIEM for analytics | Cloud-native SIEM+SOAR built on Google's infrastructure, combining large-scale log analytics with Mandiant threat intelligence |

Microsoft Sentinel emphasizes automation and identity-centric analytics and integrates tightly with Defender products and Entra ID signals. AWS does not ship a single native full SIEM equivalent; Security Lake normalizes and centralizes security telemetry (in Open Cybersecurity Schema Framework format) so it can be analyzed by GuardDuty/Detective or exported to a partner SIEM — meaning AWS's "equivalent" is really a composition of services. Google SecOps (rebranded from Chronicle) combines SIEM, SOAR, and threat intelligence in one platform, using the YARA-L detection language and Gemini-powered AI for natural-language investigation, with Mandiant and VirusTotal intelligence built in.

### Core Features
- **Sentinel:** KQL-based analytics rules, built-in and custom playbooks (Logic Apps) for SOAR, UEBA (User and Entity Behavior Analytics), MITRE ATT&CK mapping, dual-tier pricing separating an Analytics tier from a lower-cost Data Lake tier.
- **AWS Security Lake:** OCSF-normalized centralized log storage across accounts/regions, native integration with GuardDuty/Detective/Security Hub, subscriber model for third-party SIEM tools (Splunk, Sumo Logic, etc.).
- **Google SecOps:** YARA-L 2.0 correlation rules, Retrohunt (re-running new detection rules against up to 12 months of historical hot data), curated detections, Gemini AI-assisted investigation, built-in SOAR case management.

### Security & Compliance
All are backed by the parent platform's compliance certifications. A key differentiator is default hot-data retention: Google SecOps includes roughly 12 months of hot-state retention by default, while Sentinel's default hot retention is around 90 days with additional cost to extend (mitigated by its 2026 Data Lake tier). AWS's approach shifts retention/compliance control largely to the customer since Security Lake is a data lake the customer owns and configures.

### Pricing Model
- **Sentinel:** Consumption-based, billed per GB ingested into the Analytics tier (with a cheaper Data Lake tier for long-term/infrequently queried data).
- **AWS Security Lake:** Billed for data ingestion/normalization and S3 storage; downstream analytics tools (GuardDuty, Detective, or third-party SIEM) are billed separately.
- **Google SecOps:** Often licensed via fixed/subscription pricing based on ingestion volume tier rather than strict per-GB billing, which can reduce cost unpredictability during incident-driven log spikes.

### Integration for DevSecOps
All three support automated playbooks/response (Sentinel Logic Apps playbooks, AWS EventBridge + Lambda/Step Functions, Google SOAR playbooks), API-driven detection-as-code workflows that can be version-controlled and CI/CD-deployed, and integration with ticketing/ChatOps tools to close the loop between detection and remediation in a DevSecOps pipeline.

---

## Summary & Recommendations

- **Single-cloud, Microsoft-centric organizations** benefit most from the tightly integrated Azure stack (Entra ID → Monitor → Policy → Defender for Cloud → Sentinel), since each service shares identity context and feeds the next.
- **AWS-native organizations** get comparable coverage but must compose several services (IAM Identity Center, CloudWatch/CloudTrail, Config/SCPs, Security Hub/GuardDuty/Inspector, Security Lake) rather than a single unified product — offering more granular control at the cost of more integration work.
- **GCP-native organizations** benefit from simpler, more centralized pricing (e.g., SecOps's generous default retention) and strong inheritance-based governance (Organization Policy), with Security Command Center and SecOps closing the gap on CSPM/SIEM capability.
- In **multi-cloud environments**, it is common to see hybrid architectures — e.g., Microsoft Entra ID as the master identity provider federated into AWS IAM Identity Center and Google Cloud Identity, or Microsoft Defender for Cloud used to monitor posture across all three clouds from a single pane of glass.

---

## Sources
- Microsoft Learn — Microsoft Entra ID, Azure Monitor, Azure Policy, Defender for Cloud, Microsoft Sentinel documentation
- AWS Documentation — IAM, IAM Identity Center, CloudWatch, CloudTrail, Config, Security Hub, GuardDuty, Inspector, Security Lake
- Google Cloud Documentation — Cloud IAM, Cloud Identity, Cloud Operations Suite, Organization Policy Service, Security Command Center, Google Security Operations (SecOps)
- Industry comparison analyses (Gartner Peer Insights, G2, Sysdig, Scybers, TechLeague), accessed August 2026

---
