# Cloud Security Guide Implementation Plan

> **For agentic workers:** This is a single HTML file following established patterns. Build sections sequentially, verify accuracy.

**Goal:** Create `cloud-security.html` — a comprehensive AWS/GCP cloud security reference page.

**Architecture:** Single static HTML file matching existing design system (dark theme, fixed sidebar, info-boxes, attack-boxes, defence-lists, tables, diagrams). Side-by-side AWS/GCP coverage throughout.

**Tech Stack:** HTML, CSS (matching existing custom properties), vanilla JS (smooth scroll + nav highlighting)

---

### Task 1: Create page scaffold with nav + CSS + sections 1-2

**Files:**
- Create: `cloud-security.html`

- [ ] Create HTML boilerplate with identical CSS from existing pages
- [ ] Add fixed sidebar nav with all 8 section links + cross-links to other site pages
- [ ] Write Section 1: Shared Responsibility Model (table, diagram, misconception callout)
- [ ] Write Section 2: Organizational Structure (AWS Orgs vs GCP hierarchy, landing zones, guardrails, diagrams)

### Task 2: Write sections 3-4 (IAM + Networking)

- [ ] Write Section 3: IAM (AWS IAM vs GCP IAM, policy eval, attacks, defences, code examples)
- [ ] Write Section 4: Networking (VPC, firewalls, attacks, defences, comparison table, diagram)

### Task 3: Write sections 5-6 (Encryption + Compute)

- [ ] Write Section 5: Encryption & Key Management (envelope encryption, KMS, CMEK, attacks, defences)
- [ ] Write Section 6: Compute Security (EC2/GCE, serverless, containers, attacks, defences)

### Task 4: Write sections 7-8 (Storage + Logging)

- [ ] Write Section 7: Storage Security (S3/GCS, misconfigs, breaches, defences, code)
- [ ] Write Section 8: Logging & Detection (CloudTrail/Audit Logs, GuardDuty/SCC, alerting)

### Task 5: Add smooth-scroll JS + verify

- [ ] Add vanilla JS for smooth scroll and nav active-state highlighting (copy from existing pages)
- [ ] Cross-link the new page from all existing pages' nav sections
