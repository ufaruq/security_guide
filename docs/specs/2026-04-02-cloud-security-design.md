# Cloud Security Guide — AWS & GCP

## Overview

A single-page HTML reference covering cloud security for AWS and GCP side-by-side. Follows the established pattern of the existing security guide pages: concept → attack → defence → code examples → diagrams.

**Scope:** Core security services + compute/storage security (Phase B). Compliance frameworks, CI/CD, secrets management, incident response playbooks deferred to Phase C.

**Format:** Static HTML page matching existing design system (dark theme, fixed sidebar nav, info-boxes, attack-boxes, defence-lists, tables, pre/code blocks, diagrams).

## Sections

### 1. Shared Responsibility Model
- AWS vs GCP responsibility boundaries (side-by-side table)
- Provider responsibilities: physical, hypervisor, managed service internals
- Customer responsibilities: IAM, data, network config, encryption keys, application code
- Common misconception callout: "cloud = secure by default"
- Diagram: responsibility boundary visualization for IaaS vs PaaS vs SaaS

### 2. Organizational Structure (Multi-Account / Multi-Project)
- **AWS:** Organizations → OUs → Accounts, SCPs, Control Tower, Account Factory
- **GCP:** Organization → Folders → Projects, Organization Policies, Assured Workloads
- Landing zone patterns: security account, logging account, shared services, workload accounts
- Guardrails: preventive (SCPs / Org Policies) vs detective (Config Rules / Security Health Analytics)
- Billing isolation and blast radius containment
- Account/project vending automation (Terraform/IaC mention)
- Diagram: recommended org hierarchy for both providers side-by-side

### 3. Identity & Access Management (IAM)
- **AWS IAM:** Users, groups, roles, policies (identity vs resource), AssumeRole, permission boundaries, OIDC federation, instance profiles
- **GCP IAM:** Members (users, groups, service accounts), roles (basic/predefined/custom), IAM bindings, Workload Identity Federation, service account keys vs impersonation
- Policy evaluation logic (AWS explicit deny > explicit allow > implicit deny; GCP union of all bindings, deny policies)
- Least privilege patterns: Access Analyzer (AWS) / IAM Recommender (GCP)
- **Attacks:** Privilege escalation (iam:PassRole, iam:CreatePolicyVersion; setIamPolicy, actAs), confused deputy, cross-account role chaining, leaked service account keys
- **Defences:** SCPs, permission boundaries, org policies, condition keys (aws:SourceArn, iam.allowedPolicyMemberDomains), short-lived credentials, Workload Identity
- Code examples: least-privilege policy (AWS JSON), GCP IAM binding (gcloud), AssumeRole with external ID

### 4. Networking & Network Security
- VPC architecture: subnets (public/private), route tables, NAT gateways
- **AWS:** Security Groups (stateful) vs NACLs (stateless), VPC Endpoints (Gateway/Interface), PrivateLink
- **GCP:** VPC Firewall Rules (priority-based), Hierarchical Firewall Policies, Private Service Connect, VPC Service Controls
- **Attacks:** SSRF to IMDS (169.254.169.254), overly permissive 0.0.0.0/0 rules, public exposure of internal services, DNS rebinding
- **Defences:** IMDSv2 (AWS), metadata concealment (GCP), principle of least connectivity, VPC Service Controls perimeter, network segmentation
- Comparison table: AWS networking concepts → GCP equivalents
- Diagram: secure VPC architecture with public/private subnets and controlled egress

### 5. Encryption & Key Management
- Envelope encryption model (data key encrypted by KEK)
- **AWS KMS:** CMKs, key policies, grants, automatic rotation, CloudHSM
- **GCP Cloud KMS:** Key rings, crypto keys, IAM on keys, rotation, Cloud HSM, EKM
- Encryption at rest: default (SSE-S3 / Google-managed) vs CMEK vs CSEK
- Encryption in transit: TLS everywhere, ALB/HTTPS LB termination, mTLS in service mesh
- **Attacks:** KMS key policy too permissive, unencrypted snapshots/backups, cross-account key access
- **Defences:** Key policy least privilege, separate key per environment, envelope encryption, audit key usage via CloudTrail/Audit Logs
- Code examples: creating a CMK with restrictive policy, encrypting with envelope pattern

### 6. Compute Security
- **EC2/GCE hardening:** IMDSv2 enforcement, OS patching (SSM Patch Manager / OS Patch Management), CIS benchmarks, instance profiles vs service accounts
- **Serverless:** Lambda/Cloud Functions execution role scope, event injection risks, cold start and shared tenancy, environment variable secrets
- **Containers:** EKS/GKE pod security (Pod Security Standards, GKE Autopilot), Workload Identity (avoid node-level SA), image scanning (ECR/Artifact Registry), admission controllers, supply chain (Cosign/Binary Authorization)
- **Attacks:** IMDS credential theft, over-privileged Lambda role leading to lateral movement, container escape, privileged pods
- **Defences:** IMDSv2 + hop limit, minimal execution roles, read-only root filesystem, non-root containers, Binary Authorization / image signing
- Comparison table: EC2 vs GCE security features

### 7. Storage Security
- **S3/GCS** bucket misconfiguration landscape
- **AWS S3:** Bucket policies, ACLs (deprecated for new buckets), Block Public Access (account + bucket level), pre-signed URLs, S3 Object Lock
- **GCP GCS:** Uniform bucket-level access, IAM-only, signed URLs, retention policies, public access prevention
- **Attacks:** Public bucket enumeration, ACL misconfigurations, overly broad bucket policies (Principal: *), data exfiltration via pre-signed URLs
- Real-world breaches: Capital One (SSRF → S3), misconfigured GCS buckets
- **Defences:** Block Public Access (enforce at org level via SCP/Org Policy), uniform bucket-level access, least-privilege bucket policies, VPC-restricted access, server-side encryption, access logging
- Code examples: SCP to enforce S3 Block Public Access, Org Policy for GCS public access prevention

### 8. Logging, Monitoring & Detection
- **AWS:** CloudTrail (management + data events), VPC Flow Logs, GuardDuty, Security Hub, Config Rules
- **GCP:** Cloud Audit Logs (Admin Activity + Data Access), VPC Flow Logs, Security Command Center (SCC), Chronicle, Organization Policy Constraints
- What to alert on: root/super-admin usage, IAM policy changes, security group modifications, unusual API calls, impossible travel
- **Attacks:** CloudTrail tampering/disabling, log evasion (read-only API enumeration), GuardDuty suppression
- **Defences:** Immutable logging (S3 Object Lock / locked log buckets), org-level log sink, SIEM integration, automated remediation
- Comparison table: AWS detection services → GCP equivalents

## Design System

Reuse exact CSS from existing pages:
- Dark theme with CSS custom properties (--bg, --surface, --accent, etc.)
- Fixed left sidebar navigation (260px)
- Main content area (max-width 900px)
- Component classes: `.info-box.note`, `.info-box.warning`, `.info-box.key`, `.attack-box`, `.defence-list`, `pre`, `table`, `.refs`
- Responsive: sidebar hidden on mobile
- Diagrams as styled HTML `<pre>` blocks with monospace font

## Estimated Size

~1400-1800 lines (comparable to web-app-security.html at 1370 lines).

## Out of Scope (Phase C)

- Database security (RDS/Cloud SQL, DynamoDB/Firestore)
- CI/CD pipeline security (CodeBuild/Cloud Build)
- Secrets management (Secrets Manager/Secret Manager)
- Incident response playbooks
- Compliance frameworks (SOC 2, ISO 27001, FedRAMP)
- Cost/billing security (budget alerts, anomaly detection)
- WAF / DDoS (Shield/Cloud Armor)
