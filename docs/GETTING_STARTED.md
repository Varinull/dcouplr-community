# `<dcouplr/>` Community Core — Getting Started

**Scope:** Apache 2.0 Community Core only  
**Repository:** [Varinull/dcouplr-community](https://github.com/Varinull/dcouplr-community)  

Full public boundary: [COMMUNITY_CORE.md](COMMUNITY_CORE.md).

---

## Overview

`<dcouplr/>` Community Core is a local-first CLI that converts vendor monitoring rules and cloud configs into open targets (Prometheus, Grafana, OpenTofu) on your machine. Configs are not sent to a hosted translation service.

---

## 1. Installation

### Linux & macOS
```bash
curl -fsSL https://dcouplr.com/install.sh | sh && dcouplr doctor
```
Or from GitHub:
```bash
curl -fsSL https://raw.githubusercontent.com/Varinull/dcouplr-community/main/install.sh | sh && dcouplr doctor
```

### Windows PowerShell
```powershell
irm https://dcouplr.com/install.ps1 | iex
```
Or from GitHub:
```powershell
irm https://raw.githubusercontent.com/Varinull/dcouplr-community/main/install.ps1 | iex
```

---

## 2. Preflight
```bash
dcouplr doctor
```

---

## 3. First translation

Save `monitor.json`:
```json
{
  "name": "High CPU Load Alert",
  "type": "metric alert",
  "query": "avg(last_5m):avg:system.cpu.user{environment:production} > 85",
  "message": "CPU usage is above 85% on production host"
}
```

```bash
dcouplr translate -i monitor.json -t prometheus -o alert_rules.yaml
```

---

## 4. Next reading (Community Core manuals only)

- [CLI_USER_GUIDE.md](CLI_USER_GUIDE.md)
- [ENGINEER_USER_MANUAL.md](ENGINEER_USER_MANUAL.md)
- [TURNKEY_MIGRATION_RECIPES.md](TURNKEY_MIGRATION_RECIPES.md)
- [COMMUNITY_CORE.md](COMMUNITY_CORE.md)

Commercial product pages: [https://dcouplr.com](https://dcouplr.com) (not mirrored in this repository).
