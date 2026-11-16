# `<dcouplr/>` Community Core — Public Scope Guide

**Document ID:** `VA-DOC-COMMUNITY-CORE-001`  
**Audience:** Open-source users of [Varinull/dcouplr-community](https://github.com/Varinull/dcouplr-community)  
**License:** Apache License 2.0  
**Status:** Authoritative boundary for everything published in this repository  

---

## What this repository is

Community Core is the **free, Apache 2.0** distribution of `<dcouplr/>`:

- Local CLI (`dcouplr doctor`, `translate`, `parse`, `version`)
- Installers (`install.sh`, `install.ps1`)
- **Engineer user manuals and operational runbooks** for Community Core only

---

## What must never appear here

| Keep out | Examples |
| --- | --- |
| **Paid commercial tiers** | Developer Pro, Cluster Small/Medium/Large, Custom Enterprise, ACV/overage tables, contractual SLA credit schedules |
| **IP math / proprietary logic** | Formal theorems, invariant calculus, Presburger / Kirchhoff proofs, HighwayHash bounds, USPTO claim text, algebraic \(\Phi/\Psi\) disclosure |
| **Admin / governance** | `/admin`, `/company-admin`, `/v1/admin/*`, Dual-DB vault ops, Raft admin join APIs |
| **Production readiness / scorecards** | Enterprise readiness gates, GA scorecards, performance “proof” dossiers, certification matrices |
| **Commercial consoles** | Paid fleet licensing UI, upgrade checkout, war-room SLA entitlements |

Commercial editions, readiness audits, and IP filings live **outside** this Apache repository.

---

## What *is* published here

| Doc | Role |
| --- | --- |
| [GETTING_STARTED.md](GETTING_STARTED.md) | Install + first translate |
| [CLI_USER_GUIDE.md](CLI_USER_GUIDE.md) | CLI reference |
| [ENGINEER_USER_MANUAL.md](ENGINEER_USER_MANUAL.md) | Hands-on Community Core cookbook |
| [TURNKEY_MIGRATION_RECIPES.md](TURNKEY_MIGRATION_RECIPES.md) | Recipe index |
| [RUNBOOK_LIFT_AND_SHIFT_HYPERSCALER_HANDOVER.md](RUNBOOK_LIFT_AND_SHIFT_HYPERSCALER_HANDOVER.md) | Lift & shift operator runbook |
| [WALLBOARD_OPTIONS_BUILD_AND_DEPLOYMENT_RUNBOOK.md](WALLBOARD_OPTIONS_BUILD_AND_DEPLOYMENT_RUNBOOK.md) | Wallboard operator runbook |
| [API_REFERENCE_AND_TIERS.md](API_REFERENCE_AND_TIERS.md) | Community evaluation API only |
| [SUPPORT_TIERS_SLAS_AND_KPIS.md](SUPPORT_TIERS_SLAS_AND_KPIS.md) | Community support (best effort) |

---

## Community Core commands

```bash
dcouplr doctor
dcouplr version
dcouplr translate -i monitor.json -t prometheus -o alert_rules.yaml
dcouplr parse -q "avg(last_5m):avg:system.cpu.user{env:prod} > 80"
```

## Community evaluation HTTP (optional local server)

| Method | Path |
| --- | --- |
| `GET` | `/healthz` |
| `GET` | `/v1/supported-matrix` |
| `POST` | `/v1/parse` |
| `POST` | `/v1/generate` |
| `POST` | `/v1/translate` |

---

## Maintainer checklist before sync

1. Docs are manuals/runbooks only — no readiness scorecards or performance-proof dossiers.  
2. No paid tier prices or SLA credit tables.  
3. No `/admin` or `/v1/admin` page/API docs.  
4. No formal IP math / patent claim language.  
5. Point commercial questions to [https://dcouplr.com](https://dcouplr.com) without republishing commercial matrices.

Copyright © 2026 Varinull Corporation. Apache 2.0.
