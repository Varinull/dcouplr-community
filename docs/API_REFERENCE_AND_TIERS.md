# <dcouplr/> API Reference & 3-Tier Security Architecture

**Document Identifier:** `VA-SPEC-API-TIERS-001`  
**Classification:** Public Engineering & API Reference Specification  
**Version:** 1.0.0  
**Status:** Approved & Implemented  
**Security Boundary:** 100% In-VPC Execution ($0.00 Unauthorized Data Egress)  

---

## 1. Executive Summary & Design Rationale

In enterprise infrastructure migration and telemetry platforms, exposing internal primitives indiscriminately poses severe security and operational hazards. In `<dcouplr/>`, internal primitives—such as Raft voter node registration (`/v1/raft/cluster/join`), WAL database compaction (`/v1/storage/compaction/*`), and point-in-time state machine snapshotting (`/v1/storage/snapshot/create`)—must never be visible to public crawlers or unauthorized network actors.

Conversely, prospective developers evaluating the platform need frictionless, zero-barrier trial access to transpilation engines and documentation, while enterprise customers in production require robust, cryptographically verifiable authentication (API Keys, Bearer JWTs, and federated OIDC SSO) to manage multi-VPC state migrations.

To satisfy these requirements, `<dcouplr/>` enforces a **3-Tier API Exposure and Access Architecture**:

```
+---------------------------------------------------------------------------------------------------+
|                                 <dcouplr/> 3-Tier API Architecture                               |
+-------------------------------------+---------------------------------+---------------------------+
| TIER 1: Public / Evaluation         | TIER 2: Customer Authenticated  | TIER 3: Internal Admin    |
| (Open Discovery & Probes)           | (API Keys / Bearer Tokens)      | (Hidden & Admin-Only)     |
+-------------------------------------+---------------------------------+---------------------------+
| • Included in OpenAPI & Swagger UI  | • Included in OpenAPI / Swagger | • Excluded from Schema    |
| • No credentials required           | • Requires X-API-Key or Bearer  |   (include_in_schema=False)|
| • Evaluators, Probes & CLI Trials   | • Enterprise Workloads & FinOps | • Guarded by require_admin|
| • Rate Limited (120 req/min)        | • Rate Limited (1,200 req/min)  | • Storage, Consensus, Drills|
+-------------------------------------+---------------------------------+---------------------------+
```

---

## 2. Access Tier Definitions

### Tier 1: Public / Evaluation & Probes (Open)
- **Target Audience:** Prospective evaluators, open-source CLI users, automated liveness/readiness probes (Kubernetes, AWS ALB, Cloudflare), and customer support inquiries.
- **Authentication:** None required.
- **OpenAPI / Swagger UI Visibility:** Included (`include_in_schema=True`).
- **Core Endpoints:**
  - `GET /healthz` - Kubernetes liveness and readiness probe.
  - `GET /v1/system/doctor` - Diagnostic environment doctor and kernel capability audit.
  - `POST /v1/parse` - Evaluates vendor metric strings and generates AST syntax representations.
  - `POST /v1/generate` - Emits target AST primitives from intermediate representations.
  - `POST /v1/translate` - Single-shot transpilation between vendor formats.
  - `GET /v1/supported-matrix` - Returns current source-to-target transpilation support matrix.
  - `GET /v1/recipes` - Public Turnkey Migration Recipe catalog.
  - `GET /v1/telemetry/metrics` - Read-only in-memory telemetry metrics and Raft term state.
  - `GET /v1/telemetry/topology` - Read-only multi-VPC topology graph representation.
  - `GET /v1/telemetry/topology-pulse` - Sanitized 1.14 KB JSON snapshot for 24x7 NOC wallboards.
  - `GET /v1/telemetry/cost-analysis` - Zero-egress FinOps operating cost audit.

### Tier 2: Customer Authenticated (Keys & Tokens Required)
- **Target Audience:** Licensed enterprise organizations, platform engineering teams, automated CI/CD migration pipelines.
- **Authentication:** Mandatory. Requires caller to present:
  - `X-API-Key: <token>` header, OR
  - `Authorization: Bearer <token>` header.
- **Role Requirement:** `Role.DEVELOPER` or `Role.ADMIN`.
- **OpenAPI / Swagger UI Visibility:** Included (`include_in_schema=True`) with security scheme tags.
- **Core Endpoints:**
  - `POST /v1/compiler/transpile-alert-rules` - Multi-monitor custom alert recalibration with rightsizing scale factors.
  - `POST /v1/iac/state-migration` - Infrastructure-as-Code state transpilation and HCL translation.
  - `POST /v1/recipes/register` - Custom recipe registration.
  - `POST /v1/recipes/{recipe_id}/run` - Automated recipe execution runner.
  - `POST /v1/telemetry/topology/vpcs` - Dynamic multi-VPC topology node registration.
  - `POST /v1/telemetry/topology/lift-and-shift` - Full lift-and-shift blueprint synthesis (OpenTofu IaC, eBPF DNAT, subnets).
  - `POST /v1/telemetry/topology/replication-plan` - Generates hyperscaler block replication staging scripts.
  - `GET /v1/telemetry/vpcs/{vpc_id}/egress-attestation` - Cryptographic SHA-256 zero-egress proof for FinOps disputes.
  - `GET /v1/telemetry/vpcs/{vpc_id}/monitoring-bundle` - In-VPC active monitor bundle extraction.

### Tier 3: Internal Admin & Infrastructure Operations (Hidden from Schema)
- **Target Audience:** Internal platform administrators, automated consensus nodes, SRE recovery harnesses.
- **Authentication:** Strict administrative credentials required (`DCOUPLR_ADMIN_KEY` or verified admin session).
- **OpenAPI / Swagger UI Visibility:** Excluded (`include_in_schema=False`).
- **Core Endpoints:**
  - `POST /v1/raft/cluster/join` - Node join and peer voter registration.
  - `POST /v1/storage/compaction/*` - WAL database vacuuming and memory compaction.
  - `POST /v1/storage/snapshot/*` - Point-in-time state machine snapshotting.
  - `POST /v1/telemetry/circuit-breaker/drill` - Chaos network partition and trip drills.

---

## 3. Interactive Documentation & Swagger UI

- **Interactive Swagger UI:** Accessible locally or on deployed instances at `/docs/swagger` or `/docs`.
- **OpenAPI Specification JSON:** Accessible at `/docs/openapi.json` or `/static/openapi.json`.
