# `<dcouplr/>` Community Core

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows-brightgreen.svg)](#installation)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](#installation)

> **Local-first AST transpiler built on UIT-IR**  
> Parse vendor monitors in RAM, score fidelity, and emit open targets (PromQL, Grafana, OpenTofu) on your host—not through a hosted translation service.

**Public scope:** [docs/COMMUNITY_CORE.md](docs/COMMUNITY_CORE.md) — this repository publishes Community Core engineer manuals and runbooks only.

---

## What is `<dcouplr/>` Community Core?

**`<dcouplr/>`** Community Core is a free Apache 2.0 Python CLI for DevOps, Platform Engineers, and SREs:

```text
vendor monitor JSON / HCL
        ↓ parse + scrub (RAM)
UITGraph  · monitors · resources · policies · edges
        ↓ emit + fidelity score
PromQL / Grafana YAML / OpenTofu
```

Install via `install.sh` / `install.ps1`, or clone this repo and use the installers.

### What it does

- **UIT-IR spine:** Source definitions parse into a ground-up graph, then emit to open targets.
- **In-memory scrubbing:** Private IPs, credentials, and internal hostnames can be masked in RAM before parse/emit.
- **Deterministic AST path:** Formal trees and fidelity scoring—not an LLM rewrite step.
- **Explicit unmapped fallbacks:** Unmapped functions surface in `# [UNMAPPED-RESOURCE-FALLBACK]` comments.
- **Local-first:** Runs on your workstation or private host; you control network posture.

This repository does **not** publish paid-edition matrices, GA audit dossiers, internal governance vault docs, or proprietary IP proofs.

---

## Installation

### Linux & macOS
```bash
curl -fsSL https://dcouplr.com/install.sh | sh && dcouplr doctor
```
```bash
curl -fsSL https://raw.githubusercontent.com/Varinull/dcouplr-community/main/install.sh | sh && dcouplr doctor
```

### Windows PowerShell
```powershell
irm https://dcouplr.com/install.ps1 | iex
```
```powershell
irm https://raw.githubusercontent.com/Varinull/dcouplr-community/main/install.ps1 | iex
```

### Direct scripts
- [install.sh](install.sh)
- [install.ps1](install.ps1)

---

## Quickstart

```bash
dcouplr doctor
```

```bash
dcouplr translate -i monitor.json -t prometheus -o alert_rules.yaml
```

```bash
dcouplr parse -q "avg(last_5m):avg:system.cpu.user{region:us-east-1} > 80"
```

---

## Engineer manuals & runbooks

| Guide | Focus |
| :--- | :--- |
| [**Community Core scope**](docs/COMMUNITY_CORE.md) | What is (and is not) published here |
| [**Getting Started**](docs/GETTING_STARTED.md) | Install + first translate |
| [**CLI User Guide**](docs/CLI_USER_GUIDE.md) | `translate`, `parse`, `doctor`, `version` |
| [**Engineer User Manual**](docs/ENGINEER_USER_MANUAL.md) | Hands-on Community Core cookbook |
| [**Turnkey Migration Recipes**](docs/TURNKEY_MIGRATION_RECIPES.md) | Recipe index |
| [**Lift & Shift Runbook**](docs/RUNBOOK_LIFT_AND_SHIFT_HYPERSCALER_HANDOVER.md) | Hyperscaler handover patterns |
| [**Wallboard Runbook**](docs/WALLBOARD_OPTIONS_BUILD_AND_DEPLOYMENT_RUNBOOK.md) | NOC display options |
| [**Community API Reference**](docs/API_REFERENCE_AND_TIERS.md) | Local evaluation routes only |
| [**Community Support**](docs/SUPPORT_TIERS_SLAS_AND_KPIS.md) | GitHub Issues / best effort |

For commercial editions, see [https://dcouplr.com](https://dcouplr.com) — commercial matrices are not mirrored here.

---

## Community & Support

- **Issues:** [GitHub Issues](https://github.com/Varinull/dcouplr-community/issues)
- **Website:** [https://dcouplr.com](https://dcouplr.com)
- **This repository:** [https://github.com/Varinull/dcouplr-community](https://github.com/Varinull/dcouplr-community)

---

## License

Apache License, Version 2.0 — see [LICENSE](LICENSE).

Copyright © 2026 Varinull Corporation. All rights reserved.
