# `<dcouplr/>` Community Core CLI User Guide

**Scope:** Apache 2.0 Community Core only  
**Repository:** [Varinull/dcouplr-community](https://github.com/Varinull/dcouplr-community)

---

## Commands

```text
Usage: dcouplr [OPTIONS] COMMAND [ARGS]...

Commands:
  doctor     Local preflight diagnostics
  version    CLI version and build tag
  translate  Transpile monitors / configs to a target format
  parse      Inspect the AST of a source query
```

### `dcouplr doctor`
```bash
dcouplr doctor
dcouplr doctor --json
dcouplr doctor --verbose
```

### `dcouplr version`
```bash
dcouplr version
```

### `dcouplr translate`
```bash
dcouplr translate -i <input_file> -t <target_format> [-o <output_file>]
```

Targets: `grafana`, `prometheus`, `otel`, `opentofu` (as supported by your build).

```bash
dcouplr translate -i datadog_alert.json -t prometheus -o alert_rules.yaml
dcouplr translate -i cloudwatch_alarm.json -t grafana
```

### `dcouplr parse`
```bash
dcouplr parse -q "avg(last_10m):avg:aws.ec2.cpuutilization{region:us-east-1} > 90"
```

---

## Sanitization notice

Before parse/translate, Community Core attempts to mask private IPs, cloud key patterns, and internal-looking hostnames in memory. Always review outputs before applying them in production.

---

## Related manuals

- [GETTING_STARTED.md](GETTING_STARTED.md)
- [ENGINEER_USER_MANUAL.md](ENGINEER_USER_MANUAL.md)
- [COMMUNITY_CORE.md](COMMUNITY_CORE.md)
