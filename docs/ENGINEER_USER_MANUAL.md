# `<dcouplr/>` Community Core — Engineer Field Manual

**Document ID:** `VA-MAN-ENG-COMMUNITY-001`  
**Scope:** Apache 2.0 Community Core only  
**License:** Apache License 2.0  

This is the public engineer guide for [Varinull/dcouplr-community](https://github.com/Varinull/dcouplr-community). It does **not** cover paid tiers, admin vaults, or proprietary IP proofs. See [COMMUNITY_CORE.md](COMMUNITY_CORE.md).

---

## 1. What you get

Community Core is a **local-first** Python CLI that:

1. Reads vendor monitor / IaC definitions on your machine  
2. Scrubs sensitive tokens in memory (private IPs, keys, hostnames)  
3. Parses into UIT-IR (`UITGraph`)  
4. Emits open targets (PromQL, Grafana alerting YAML, OpenTofu HCL)  
5. Reports a fidelity score and explicit unmapped fallbacks  

No hosted translation hop. No Community Core requirement to call a public SaaS translator.

---

## 2. Install and preflight

```bash
# Linux / macOS
curl -fsSL https://raw.githubusercontent.com/Varinull/dcouplr-community/main/install.sh | sh

# Windows PowerShell
irm https://raw.githubusercontent.com/Varinull/dcouplr-community/main/install.ps1 | iex

dcouplr doctor
dcouplr version
```

---

## 3. Everyday cookbook

### 3.1 Datadog monitor → Prometheus

```bash
dcouplr translate -i monitor.json -t prometheus -o alert_rules.yaml
```

### 3.2 CloudWatch alarm → Grafana

```bash
dcouplr translate -i cloudwatch_alarm.json -t grafana -o grafana_alert.yaml
```

### 3.3 Inspect a query AST

```bash
dcouplr parse -q "avg(last_5m):avg:system.cpu.user{env:prod} > 80"
```

### 3.4 Optional local HTTP evaluation

```bash
curl -s http://127.0.0.1:8000/v1/supported-matrix
```

Only Community evaluation routes are documented: see [API_REFERENCE_AND_TIERS.md](API_REFERENCE_AND_TIERS.md).

---

## 4. Sanitization (operator view)

Before parse/emit, Community Core attempts to mask in volatile memory:

- RFC 1918 private addresses  
- Cloud access key patterns  
- Internal-looking hostnames  

Treat outputs as still requiring human review before production apply. Community Core does **not** replace your change-management process.

---

## 5. Unmapped fallbacks

When a vendor function has no clean mapping, Community Core keeps an explicit comment such as:

```yaml
# [UNMAPPED-RESOURCE-FALLBACK]: requires operator review
```

Do not treat silence as success — check for these markers.

---

## 6. Support

Best-effort via GitHub Issues. Details: [SUPPORT_TIERS_SLAS_AND_KPIS.md](SUPPORT_TIERS_SLAS_AND_KPIS.md).

Commercial capacity, contractual SLAs, and in-VPC admin consoles are outside this manual.

---

## 7. Related Community Core docs

- [GETTING_STARTED.md](GETTING_STARTED.md)  
- [CLI_USER_GUIDE.md](CLI_USER_GUIDE.md)  
- [TURNKEY_MIGRATION_RECIPES.md](TURNKEY_MIGRATION_RECIPES.md)  
- [COMMUNITY_CORE.md](COMMUNITY_CORE.md)  
