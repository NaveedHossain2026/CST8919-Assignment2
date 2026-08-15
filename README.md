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

| **Aspect** | **Microsoft Entra ID** | **AWS IAM / IAM Identity Center** | **Google Cloud IAM / Cloud Identity** |
|---|---|---|---|
| **Scope model** | Tenant-wide directory that spans Azure, Microsoft 365, and third-party SaaS applications. | IAM is scoped to individual AWS accounts, while IAM Identity Center provides centralized workforce access across an AWS Organization. | IAM permissions are applied at the organization, folder, project, or resource level, with permissions inherited through the hierarchy. |
| **Primary role** | Full Identity Provider (IdP) that authenticates users and manages identities and access to applications and cloud resources. | Primarily an access-management and authorization system for AWS resources and APIs. IAM Identity Center provides centralized workforce authentication and access. | IAM provides authorization for GCP resources, while Cloud Identity provides identity, user, and directory management. |

Microsoft Entra ID centralizes identity at the tenant level and serves as a single directory for an entire organization. AWS IAM Identity Center instead sits above individual AWS accounts to unify workforce access across a multi-account AWS Organization, while raw AWS IAM manages permissions inside a single account. Google Cloud IAM applies permissions at the project level with inheritance through folders and organizations, and Cloud Identity supplies the directory/SSO layer comparable to Entra ID.

### Core Features
- **Microsoft Entra ID:** Conditional Access, Privileged Identity Management (PIM/JIT access), Identity Protection (risk-based sign-in analytics), B2B/B2C guest access, Application Proxy for on-prem apps.
- **AWS IAM / Identity Center:** Fine-grained JSON policies down to individual API actions, roles/service-linked roles, permission sets applied across accounts, external IdP federation (SAML/SCIM).
- **Google Cloud IAM / Cloud Identity:** Predefined and custom IAM roles, context-aware access, org-policy inheritance, integration with Google Workspace, passwordless sign-in.

### Security & Compliance
All three support SOC 2, ISO 27001, FedRAMP, and industry frameworks such as PCI DSS and HIPAA at the platform level. Entra ID adds built-in Conditional Access and Identity Protection risk scoring; AWS IAM relies more heavily on precise (but complex) policy authoring and is often paired with third-party PAM tooling for JIT access; Google Cloud emphasizes transparent audit logging and simpler per-user licensing.

### Pricing Model
- **Entra ID:** Free tier included with Azure/M365; Premium P1/P2 tiers add Conditional Access, PIM, and Identity Protection (roughly $6–$9 USD/user/month for P1/P2 tiers).
- **AWS IAM:** IAM itself is free; IAM Identity Center is free — costs come from the accounts/resources it governs.
- **GCP:** Cloud IAM is free; Cloud Identity is free for basic use, with the paid Cloud Identity Premium tier priced per user/month for advanced device and app management.

### Integration for DevSecOps
All three integrate with CI/CD via service principals / roles / service accounts, support Terraform/ARM/CloudFormation/Deployment Manager for IaC-driven access provisioning, and can be federated together (e.g., Entra ID federated into AWS IAM Identity Center via SAML/SCIM) in real-world multi-cloud environments.

---

## 2. Monitoring & Log Analytics

### Overview
| | Azure Monitor & Log Analytics | Amazon CloudWatch (+ CloudTrail) | Google Cloud Operations Suite |
|---|---|---|---|
| **Purpose** | Unified metrics, logs, and traces for Azure and hybrid resources, queried with Kusto Query Language (KQL) | Metrics, logs, alarms (CloudWatch) plus API-activity auditing (CloudTrail) | Unified Cloud Logging + Cloud Monitoring, formerly Stackdriver |

### Core Features
- **Azure Monitor/Log Analytics:** Centralized Log Analytics workspaces, KQL queries, Application Insights (APM), Workbooks for dashboards, Alerts/Action Groups.
- **CloudWatch/CloudTrail:** Custom metrics/dashboards, Logs Insights query language, Alarms with auto-scaling triggers, CloudTrail for full API call history and governance auditing.
- **GCP Operations Suite:** Cloud Logging (structured, exportable to BigQuery/Pub/Sub), Cloud Monitoring dashboards and uptime checks, Cloud Trace and Cloud Profiler for APM.

### Security & Compliance
Each platform's monitoring layer inherits the parent platform's compliance certifications (SOC 1/2/3, ISO 27001, FedRAMP High, HIPAA-eligible). Log immutability/retention and export-to-SIEM capabilities are key controls auditors look for in all three; CloudTrail's log-file integrity validation and Azure Monitor's diagnostic settings pipeline into Sentinel/Security Hub/SecOps for centralized retention.

### Pricing Model
All three follow **consumption-based pricing**: pay per GB ingested/retained (Log Analytics, CloudWatch Logs, Cloud Logging) plus per-metric and per-alarm charges. Azure and GCP typically include a modest free tier of log ingestion/retention (e.g., first several GB/month); AWS charges per metric beyond the free-tier basic metrics and per GB for Logs Insights queries.

### Integration for DevSecOps
Native integration with each provider's IaC tooling (Bicep/ARM, CloudFormation/CDK, Terraform/Deployment Manager), Grafana/Prometheus exporters, and webhook/Action-Group-style alerting into ticketing or ChatOps tools (Slack, Teams, PagerDuty) for CI/CD pipeline observability and automated rollback triggers.

---

## 3. Policy & Governance

### Overview
| | Azure Policy | AWS Config + Organizations (SCPs) | GCP Organization Policy Service |
|---|---|---|---|
| **Purpose** | Enforces and audits organizational standards/compliance across Azure resources | AWS Config continuously assesses resource configuration compliance; Service Control Policies (SCPs) in AWS Organizations set guardrails at the account/OU level | Centrally constrains which GCP resource configurations are allowed, enforced through the resource hierarchy |

### Core Features
- **Azure Policy:** Built-in and custom policy definitions, Initiatives (policy sets), deny/audit/deployIfNotExists effects, compliance dashboard, remediation tasks.
- **AWS Config/SCPs:** Config Rules (managed and custom via Lambda), Conformance Packs bundling multiple rules, SCPs that set maximum permission boundaries per OU/account, drift detection.
- **GCP Organization Policy:** Boolean/list constraints applied at org/folder/project level, policy inheritance, dry-run mode, custom constraints via Common Expression Language (CEL).

### Security & Compliance
All three map to common compliance frameworks (CIS Benchmarks, NIST 800-53, PCI DSS) via built-in policy/rule packs. Azure Policy's Regulatory Compliance dashboard and AWS Config Conformance Packs both provide continuous compliance scoring; GCP's Organization Policy is intentionally narrower (preventive guardrails) and is usually paired with SCC for detective/compliance reporting.

### Pricing Model
- **Azure Policy:** Free for standard policy evaluation; DeployIfNotExists remediation and resource changes may incur costs from the resources created.
- **AWS Config:** Charged per configuration item recorded and per rule evaluation; Conformance Packs add evaluation costs.
- **GCP Organization Policy:** Free to use; cost is indirect (e.g., BigQuery/SCC costs if paired with reporting tools).

### Integration for DevSecOps
Policies-as-code is the common pattern: Azure Policy definitions and Initiatives can be version-controlled and deployed via Bicep/ARM/Terraform in a pipeline; AWS Config Rules and SCPs are deployable via CloudFormation/Terraform and validated in CI before merge; GCP Organization Policies integrate with Terraform and Cloud Build for policy-gated deployments. All three support "shift-left" policy checks (e.g., pre-deployment `what-if`/plan validation) before resources reach production.

---

## 4. Cloud Security Posture Management & Workload Protection

### Overview
| | Microsoft Defender for Cloud | AWS Security Hub + GuardDuty + Inspector | Google Security Command Center (SCC) |
|---|---|---|---|
| **Purpose** | Combines Cloud Security Posture Management (CSPM) with Cloud Workload Protection (CWPP) across Azure, AWS, GCP, and on-prem | Security Hub aggregates findings (CSPM); GuardDuty provides threat detection; Inspector performs vulnerability scanning | Centralized asset inventory, vulnerability/misconfiguration detection, and threat detection for GCP resources |

Microsoft Defender for Cloud is explicitly multi-cloud, continuously assessing security posture across Azure, AWS, and GCP resources and providing prioritized, benchmark-aligned recommendations plus automated remediation. AWS Security Hub is primarily a findings-aggregation and CSPM service that depends on other AWS services (GuardDuty for threat detection, Inspector for vulnerability scanning, IAM Access Analyzer) to supply its data, and it is AWS-account-centric by design. Google SCC combines asset inventory, misconfiguration/vulnerability detection, and can integrate a built-in SIEM/SOAR layer, with strong integrations into BigQuery and third-party tools.

### Core Features
- **Defender for Cloud:** Secure Score, attack path analysis, Just-in-Time VM access, container/Kubernetes protection, regulatory compliance dashboard, multi-cloud connectors.
- **AWS Security Hub/GuardDuty/Inspector:** AWS Security Finding Format normalization, CIS/PCI DSS/NIST standards mapping, automated response via EventBridge, GuardDuty's ML-based anomaly detection on VPC Flow Logs/CloudTrail/DNS logs.
- **Google SCC:** Continuous asset discovery, Security Health Analytics, Web Security Scanner, Event Threat Detection, integration with Forseti and third-party SIEM/SOAR tooling.

### Security & Compliance
All three map findings to CIS Benchmarks, PCI DSS, and NIST frameworks and support attestation reporting for auditors. Defender for Cloud differentiates with native cross-cloud coverage (protecting AWS/GCP resources directly from the Azure control plane); Security Hub is deepest for pure-AWS environments; SCC's Premium/Enterprise tiers add built-in SIEM/SOAR functionality that the other two require external services (Sentinel/SecOps) to achieve.

### Pricing Model
- **Defender for Cloud:** Free foundational CSPM tier; paid "Defender plans" priced per protected resource (VM, SQL, Storage, Kubernetes node, etc.).
- **AWS Security Hub:** Priced per security check and per finding ingested, plus underlying GuardDuty (per GB of log data analyzed) and Inspector (per resource scanned) costs.
- **Google SCC:** Free Standard tier for basic asset inventory; Premium/Enterprise tiers priced by asset count/usage and include the added threat-detection and SIEM/SOAR capability.

### Integration for DevSecOps
All three expose APIs/webhooks for pipeline gating (e.g., failing a build on critical findings), support Infrastructure-as-Code scanning integration, and can trigger automated remediation (Azure Logic Apps, AWS EventBridge + Lambda, GCP Cloud Functions/Workflows) as part of a shift-left DevSecOps loop.

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
