# 🧭 `<dcouplr/>` Community Core CLI User Guide

**Document Classification:** Developer Enablement & Technical Reference  
**License:** Apache License 2.0  
**Repository:** [Varinull/dcouplr-community](https://github.com/varinull/dcouplr-community)  

---

## 📌 Available Commands

The Community Core CLI provides the following essential command-line tools:

```text
Usage: dcouplr [OPTIONS] COMMAND [ARGS]...

  <dcouplr/> Zero-Egress In-VPC Observability & IaC Transpiler

Commands:
  doctor     Run local system and dependency preflight diagnostics
  version    Display current CLI version, build tag, and environment
  translate  Transpile monitoring alert rules and dashboards into target formats
  parse      Inspect the Abstract Syntax Tree (AST) of a source query
```

---

## 1. `dcouplr doctor`
Runs a diagnostic scan on your local terminal environment to verify system prerequisites:

```bash
dcouplr doctor
```

Flags:
- `--json`: Output preflight diagnostic results as structured JSON.
- `--verbose`: Print detailed memory and path inspection diagnostics.

---

## 2. `dcouplr version`
Displays client binary version, platform, architecture, and licensing tier:

```bash
dcouplr version
```

Example Output:
```text
<dcouplr/> Community Core v1.0.0
Architecture: linux/amd64 (Statically linked)
License: Apache-2.0
Egress Guarantee: 0.00 KB (Zero-Egress In-VPC Boundary)
```

---

## 3. `dcouplr translate`
Converts monitoring alert rules and telemetry definitions into modern open standards without sending data over the network:

```bash
dcouplr translate -i <input_file> -t <target_format> [-o <output_file>]
```

Supported Targets (`-t`):
- `grafana` / `prometheus`: Generates Prometheus/Mimir alert rules or Grafana 11 dashboard JSON.
- `otel`: Generates OpenTelemetry Collector metric transformation processors.
- `opentofu`: Transpiles infrastructure blocks into OpenTofu HCL.

Examples:
```bash
# Translate Datadog alert to Prometheus Rule YAML
dcouplr translate -i datadog_alert.json -t prometheus -o alert_rules.yaml

# Translate CloudWatch alarm JSON to Grafana alerting format
dcouplr translate -i cloudwatch_alarm.json -t grafana
```

---

## 4. `dcouplr parse`
Validates and inspects the formal AST structure of an alert or telemetry query:

```bash
dcouplr parse -q "avg(last_10m):avg:aws.ec2.cpuutilization{region:us-east-1} > 90"
```

Useful for validating query syntax and tag taxonomy before migrating.

---

## 5. 🛡️ In-Memory Sanitization Notice
Before any parsing or translation begins, `<dcouplr/>` executes an automated in-memory regex scrubber:
- RFC 1918 Private IPs (e.g. `10.0.0.0/8`, `192.168.0.0/16`)
- Cloud access keys (`AKIA...`, private tokens)
- Internal domain suffixes (`*.internal`, `*.corp`)

All masked tokens remain localized strictly within volatile RAM and are never written to disk or sent across network boundaries.
