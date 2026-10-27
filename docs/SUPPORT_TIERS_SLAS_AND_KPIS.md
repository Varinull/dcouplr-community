# <dcouplr/> Support Tiers, SLAs & Operational KPIs Specification

**Document ID:** VA-SPEC-SUPPORT-SLA-001  
**Version:** 1.0.0 (GA Enterprise Specification)  
**Date:** 2026-10-27  
**Classification:** Public Specification & Enterprise Master Service Agreement (MSA) Appendix  
**Target Audience:** SREs, Platform Engineers, DevOps Leads, Procurement & Legal Teams  
**Published Reference:** [https://dcouplr.com/docs#support-slas](https://dcouplr.com/docs#support-slas)  

---

## 1. Executive Summary & The Zero-Egress Support Paradigm

`<dcouplr/>` is an open-core, in-perimeter observability and infrastructure-as-code transpilation engine designed to operate 100% inside private VPCs, Kubernetes clusters, and air-gapped on-premises datacenters. Because `<dcouplr/>` enforces a mathematical zero-egress guarantee ($Egress_{WAN} \equiv 0.00\text{ KB}$), our support and incident management model is deliberately architected for **zero-trust security**:

1. **No External Credentials or Data Ingestion:** `<dcouplr/>` support engineers never request, store, or receive customer production credentials, private keys, database connection strings, or unredacted metric feeds.
2. **In-Perimeter Diagnostic Pairing:** Technical troubleshooting takes place inside the customer's perimeter via live in-VPC screen sharing or private, ephemeral WebRTC video war-rooms.
3. **Deterministic AST Linters:** Diagnostic data shared during support engagements is processed through local HMAC-SHA256 anonymization filters (`LocalTokenAnonymizer`), ensuring zero IP or hostname disclosure.
4. **Radical Transparency:** Every tier's entitlements, guaranteed response times, resolution targets, operational KPIs, and contractual financial remedies are codified publicly and objectively measured.

---

## 2. Tier-by-Tier Support Entitlements Matrix

The table below outlines the service boundaries, coverage windows, support channels, and engineering allocation across all six commercial licensing tiers:

| Attribute | Tier 0: Community Core | Tier 1: Developer Pro | Tier 2: Cluster Small | Tier 3: Cluster Medium | Tier 4: Cluster Large | Tier 5: Custom Enterprise |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Pricing Model** | **$0 / month** (Free) | **$300 / month** | **$1,500 / month** | **$3,500 / month** | **$45 / node / month** | **Custom Annual Contract** |
| **Monitored Capacity** | 1 Local CLI / 10 Nodes | 25 Monitored Nodes | 50 Monitored Nodes | 150 Monitored Nodes | 91 to 500+ Nodes | Unlimited Enterprise Nodes |
| **License Type** | Apache 2.0 Open-Core | Commercial In-VPC | Commercial In-VPC | Commercial Multi-VPC | Commercial Fleet Mesh | Air-Gapped Perpetual |
| **Coverage Hours** | Asynchronous (Volunteer) | 9×5 Local Business | 12×5 Extended (UTC) | 24×7×365 for P1 / 12×5 for P2–P4 | 24×7×365 Continuous Priority | 24×7×365 Follow-the-Sun |
| **P1 First Response SLA** | **Best Effort** | **24 Hours** (Bus. Days) | **4 Hours** | **2 Hours** (24/7 Guaranteed) | **1 Hour** (24/7 Guaranteed) | **15 Minutes** (Ultra-Priority) |
| **Support Intake Channels** | GitHub Discussions & Issues | Priority Email Desk (`support@dcouplr.com`) | In-VPC Support Desk (`/support`) | Shared Slack / Teams Channel + Web Desk | Direct PagerDuty / Opsgenie + Slack Connect | 24/7 Emergency Hotline + Private Slack Grid |
| **Incident War-Room Bridge** | — | — | Ad-hoc Screen-Share (1/qtr) | Dedicated On-Demand Bridge (< 30m) | Live Video War-Room (< 15m) + PagerDuty | Instant War-Room Cell (< 5m) + Dedicated On-Call |
| **Account Management** | Community Forums | Shared Support Queue | Pooled Technical Support | Designated Associate TAM | Dedicated Named Senior TAM | Dedicated Principal TAM & Solutions Architect |
| **Custom Parser SLAs** | Community PRs | Roadmap Requests | Roadmap Priority | 30-Day Parser Commit | 14-Day Bespoke Grammar Delivery | 5-Day Bespoke Parser Sprint |
| **SLA Credit Remedy** | — | — | — | Included (10%–50%) | Included (10%–50%) | Custom MSA Penalties |

---

## 3. Incident Severity Classification & Response SLA Matrix

Incoming incident tickets are categorized by operational severity. Response times represent the maximum guaranteed duration between ticket submission and active hands-on triage by a certified `<dcouplr/>` Site Reliability Engineer:

### 3.1 Severity Classifications

- **Severity 0 (P0) — Catastrophic Disaster / Cluster Failure (Tier 5 Exclusive):**
  - *Criteria:* Total collapse of observability or migration orchestrator across production cluster; unrecoverable split-brain in Raft consensus; catastrophic data corruption risk.
  - *Mitigation Target:* **< 4 Hours**
  - *Status Update Cadence:* Every 30 minutes on active War-Room bridge.

- **Severity 1 (P1) — Critical Outage / System Inoperable:**
  - *Criteria:* Primary `<dcouplr/>` in-VPC daemon halted; telemetry or alerting pipeline completely stalled; production migration wave blocked during cutover window.
  - *Mitigation Target:* **< 8 Hours** (Tiers 3–5)
  - *Status Update Cadence:* Every 60 minutes via ticket/channel until mitigation.

- **Severity 2 (P2) — Major Degradation / Impaired Core Feature:**
  - *Criteria:* High AST transpilation error rates on specific monitor formats; replication lag exceeding 15 minutes; partial dashboard failure with CLI intact. Workaround available.
  - *Mitigation Target:* **< 24 Hours**
  - *Status Update Cadence:* Every 4 hours during business hours.

- **Severity 3 (P3) — Minor Defect / Non-Blocking Bug:**
  - *Criteria:* Cosmetic anomalies in UI canvas, non-blocking fallback comment warnings, non-critical telemetry stream latency.
  - *Mitigation Target:* Next scheduled patch release / sprint cycle.
  - *Status Update Cadence:* Upon status change or release deployment.

- **Severity 4 (P4) — General Inquiry / Guidance / Feature Request:**
  - *Criteria:* AST rule configuration consultation, syntax questions, capacity planning, feature suggestions.
  - *Mitigation Target:* Informational resolution.
  - *Status Update Cadence:* Asynchronous follow-up.

### 3.2 Response SLA Commitments by Tier

| Severity Level | Tier 5 (Enterprise) | Tier 4 (Cluster Large) | Tier 3 (Cluster Medium) | Tier 2 (Cluster Small) | Tier 1 (Developer Pro) | Tier 0 (Community) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **P0 Disaster** | **15 Minutes** (24/7) | — | — | — | — | — |
| **P1 Critical** | **15 Minutes** (24/7) | **1 Hour** (24/7) | **2 Hours** (24/7) | **4 Hours** (12/5) | 24 Hours (Bus. Days) | Best Effort |
| **P2 Major** | **1 Hour** | **2 Hours** | **4 Hours** | **8 Hours** | 48 Hours | Best Effort |
| **P3 Normal** | **4 Hours** | **8 Hours** | **12 Hours** | **24 Hours** | 72 Hours | Community Triage |
| **P4 Inquiry** | **8 Hours** | **16 Hours** | **24 Hours** | **48 Hours** | 5 Business Days | Discussions |

---

## 4. Support Key Performance Indicators (KPIs) & Operational Reliability Standards

`<dcouplr/>` benchmarks and audits internal support operations against rigorous site reliability metrics. The following targets are tracked 24x7x365:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   <dcouplr/> SRE OPERATIONAL TARGETS                   │
├─────────────────────┬──────────────────────┬───────────────────────────┤
│ METRIC              │ TARGET BENCHMARK     │ CONTRACTUAL COMMITMENT    │
├─────────────────────┼──────────────────────┼───────────────────────────┤
│ SLA Compliance Rate │ 99.9% Monthly        │ Credit-backed (Tiers 3–5) │
│ MTTA (P0/P1)        │ < 5 Minutes          │ All 24/7 Tiers            │
│ MTTI (Triage)       │ < 30m (T5) / < 1h(T4)│ SRE Root-Cause Analysis   │
│ MTTM (Mitigation)   │ < 4 Hours (95% P1)   │ Temporary Workaround/Fix  │
│ War-Room Latency    │ < 5m (T5) / < 15m(T4)│ Active Video Bridge       │
│ First-Contact Res.  │ ≥ 75% for P3/P4      │ Single-Response Solved    │
│ CSAT Score          │ ≥ 98.5% Positive     │ Post-Resolution Survey    │
└─────────────────────┴──────────────────────┴───────────────────────────┘
```

### Definitions:
- **First Response Time (FRT):** The elapsed time from ticket submission to first meaningful technical response from an assigned engineer (automated acknowledgments do not satisfy FRT).
- **Mean Time to Acknowledge (MTTA):** Duration from customer alert dispatch to human engineer claim and assignment in the internal queue.
- **Mean Time to Investigate (MTTI):** Time required to isolate root cause, capture AST reproducers, and establish a remediation hypothesis.
- **Mean Time to Mitigate (MTTM):** Time elapsed until the customer's production system is restored to safe, operable status (via patch, configuration adjustment, or fallback bypass).
- **War-Room Spin-Up Latency:** Elapsed time from P1 declaration to active WebRTC video bridge with certified `<dcouplr/>` incident leads present.

---

## 5. In-VPC SRE Emergency Incident War-Room Protocol

For critical incidents (P0/P1), `<dcouplr/>` activates live emergency incident war-rooms powered by an embedded, zero-egress WebRTC bridge (`JitsiMeetExternalAPI` / secure Google Meet bridge):

### 5.1 Architecture & Privacy Isolation
- **Zero Third-Party Accounts:** Customer engineers join via a time-scoped single-click URL generated directly by their local cluster (`http://127.0.0.1:8000/support#bridges`).
- **Peer-to-Peer Encryption:** Audio, video, and screen sharing are negotiated over encrypted WebRTC channels directly or through the customer-managed Zero-Trust Tunnel (`<dcouplr/> AirLock™`).
- **Volatile Diagnostic Ring Buffer:** Diagnostic inspections use `<dcouplr/>`'s in-memory telemetry ring buffer (`TelemetryRingBuffer`). Telemetry snapshots never leave volatile RAM.

### 5.2 War-Room Command Structure
During an active incident bridge, the following four roles are staffed:

1. **Incident Commander (IC):** Drives the incident timeline, maintains the root-cause hypothesis board, and coordinates all actions with customer engineering leadership.
2. **Triage SRE:** Directly inspects live socket bindings, Raft quorum health (`/healthz`), and eBPF DNAT routing rules.
3. **AST Compiler Specialist:** Live-patches failing query tokens, evaluates translation fallback comments, and provides emergency syntax transpilation overrides.
4. **Technical Account Manager (TAM):** Logs action items, manages executive communications, and drafts the comprehensive RCA (Root Cause Analysis) document within 48 hours of resolution.

### 5.3 Time-Boxing & Cryptographic Auditing
- Bridge sessions default to 2-hour windows with explicit customer renewal.
- All bridge spin-ups, participant joins, and configuration mutator actions are recorded in immutable local audit logs (`schemas/migrations/0002_add_preflight_check_audit_logs.sql`) with HMAC-SHA256 signatures.

---

## 6. Contractual Service Level Agreement (SLA) Credit Policy

To back our commitments with real financial accountability, `<dcouplr/>` provides automatic service credits to active commercial subscribers (Tiers 3, 4, and 5) if contractual response times are breached:

| SLA Breach Event (Within Monthly Billing Cycle) | Service Credit Applied to Next Monthly Invoice |
| :--- | :---: |
| **First Confirmed P1 Response SLA Breach** | **10% Credit** of monthly subscription fee |
| **Second Confirmed P1 Response SLA Breach** | **25% Credit** of monthly subscription fee |
| **Third Confirmed P1 Response SLA Breach** | **50% Credit** of monthly subscription fee |

### Credit Request Procedure:
1. Customer submits a credit claim within 30 days of incident closure referencing the Ticket ID and timestamp.
2. The `<dcouplr/>` TAM validates the ticket audit log against our internal MTTA/FRT telemetry.
3. Approved credits are automatically applied to the following month's invoice or deducted from annual renewal balances.

---

## 7. How to Initiate Support & Escalation Channels

### 7.1 Dispatching via Terminal / CLI
Customers can dispatch support tickets directly from any workstation or server where the CLI is installed:

```bash
# Create an emergency ticket with local diagnostics bundled:
dcouplr support ticket create \
  --severity P1 \
  --title "Replication block sync stalled on VPC-us-east-1" \
  --description "AWS MGN replication lag exceeded 15 minutes during Wave 2 cutover"

# Response confirmation:
# [OK] Ticket CUST-TKT-884192 created successfully.
# [SLA] Response guaranteed within 1 Hour (Cluster Large Tier).
# [BRIDGE] Ephemeral SRE War-Room URL: https://dcouplr.local:8000/support#bridge-884192
```

### 7.2 In-VPC Customer Operations Web Portal
Navigate to the local web interface on port 8000:
```
http://127.0.0.1:8000/support
```
From this portal, engineers can view real-time SLA count-down timers, upload sanitized monitor definitions, submit incident notes, and join active WebRTC war-room bridges with a single click.

### 7.3 Direct Enterprise Escalation Channels
- **Priority Email Desk (Tiers 1–5):** `support@dcouplr.com` (Prefix urgent issues with `[P1 CRITICAL]` in the subject).
- **Shared Slack Connect / Microsoft Teams (Tiers 3–5):** `#dcouplr-<customer-org>-warroom` (Monitored 24/7/365 by primary on-call SREs).
- **Direct PagerDuty / Opsgenie Integration (Tiers 4–5):** Bidirectional webhook configured during customer pilot onboarding, instantly paging our engineering on-call rotation.
- **Executive Hotline (Tier 5):** Direct phone bridge to `<dcouplr/>` VP of Engineering and Head of SRE.

---

*Copyright &copy; 2026 Varinull Corporation. All rights reserved.*  
*For questions regarding commercial support contracts or bespoke enterprise SLAs, contact [procurement@dcouplr.com](mailto:procurement@dcouplr.com).*
