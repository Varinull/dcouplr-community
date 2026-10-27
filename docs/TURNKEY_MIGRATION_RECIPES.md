# <dcouplr/> Turnkey Migration Recipes (Recipes 1–8)

**Classification:** Developer & Platform Engineering Recipe Cookbook  
**Governing Architectural Standard:** `VA-SPEC-FINOPS-BRIDGE-001` / `D-41`  
**License:** Apache 2.0 (Open-Core Community)  
**Security Boundary:** 100% In-VPC / Localhost Execution ($0.00 WAN Egress)  

---

## Overview

The **`<dcouplr/>` Turnkey Migration Recipes** provide platform engineers, SREs, and cloud architects with battle-tested, copy-paste recipes for migrating alert definitions, dashboard metrics, and infrastructure-as-code from proprietary ecosystems to open cloud-native standards.

All recipes operate **100% in volatile memory** with zero external network egress.

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        TURNKEY RECIPES SUMMARY MATRIX                                  │
├──────────┬─────────────────────────────────────┬───────────────────────────────────────┤
│ Recipe   │ Source                              │ Target                                │
├──────────┼─────────────────────────────────────┼───────────────────────────────────────┤
│ Recipe 1 │ Datadog Monitor JSON                │ Grafana 11 Unified Alerting YAML      │
│ Recipe 2 │ New Relic NRQL Alert JSON           │ Prometheus Alertmanager YAML          │
│ Recipe 3 │ AWS CloudWatch Metric Alarms        │ OpenTelemetry / PromQL Rules          │
│ Recipe 4 │ Azure Monitor Metric Alerts         │ PromQL (Azure Arc & Google Anthos)    │
│ Recipe 5 │ HashiCorp Terraform HCL             │ OpenTofu Multi-Cloud Landing Zone HCL │
│ Recipe 6 │ Cloud-Native Block Replication      │ AWS MGN / GCP M4CE / Azure Migrate    │
│ Recipe 7 │ Enterprise Identity (Okta/Entra ID) │ Zero-Trust OIDC SSO Integration       │
│ Recipe 8 │ Distributed Raft Consensus Mesh     │ In-VPC High Availability Cluster      │
└──────────┴─────────────────────────────────────┴───────────────────────────────────────┘
```

---

## Recipe 1: Datadog Monitor JSON &rarr; Grafana 11 Unified Alerting

### Objective
Extract Datadog monitors via API or backup export and convert them deterministically into Grafana 11 Unified Alerting rule groups.

### Step 1: Export Datadog Monitor
Create `datadog_monitor.json`:
```json
{
  "name": "Production Database P95 Latency",
  "type": "metric alert",
  "query": "avg(last_5m):avg:postgresql.query.time{env:prod,cluster:us-east-1} > 250",
  "message": "Critical: Postgres latency P95 exceeded 250ms @slack-sre-channel @pagerduty",
  "tags": ["env:prod", "service:billing", "team:sre"],
  "options": {
    "thresholds": {
      "critical": 250.0,
      "warning": 180.0
    }
  }
}
```

### Step 2: Transpile via `<dcouplr/>` CLI
```bash
dcouplr translate \
  --source datadog \
  --target grafana \
  --input datadog_monitor.json \
  --output grafana_alert.yaml
```

### Result: `grafana_alert.yaml`
```yaml
apiVersion: 1
groups:
  - orgId: 1
    name: production-database-p95-latency
    folder: Infrastructure-Alerts
    interval: 1m
    rules:
      - uid: dcouplr-pg-lat-001
        title: Production Database P95 Latency
        condition: C
        data:
          - refId: A
            queryType: prometheus
            relativeTimeRange:
              from: 300
              to: 0
            model:
              expr: avg_over_time(postgresql_query_time_seconds{env="prod",cluster="us-east-1"}[5m])
          - refId: B
            queryType: math
            model:
              expression: $A > 0.25
        annotations:
          summary: "Production Database P95 Latency"
          description: "Postgres latency P95 exceeded 250ms"
        labels:
          severity: critical
          env: prod
          team: sre
```

---

## Recipe 2: New Relic NRQL &rarr; Prometheus Alertmanager

### Objective
Translate New Relic NRQL alerts with aggregator functions (`average()`, `percentile()`, `rate()`) into standard Prometheus alerting rules.

### Step 1: Input `nrql_alert.json`
```json
{
  "name": "High HTTP 5xx Error Rate",
  "nrql": {
    "query": "SELECT count(*) FROM Transaction WHERE response.status >= 500 AND appName = 'checkout' FACET host",
    "since_value": "3"
  },
  "terms": [
    {
      "operator": "above",
      "threshold": "50",
      "time_function": "all",
      "duration": 5
    }
  ]
}
```

### Step 2: Transpile Command
```bash
dcouplr translate \
  --source new_relic \
  --target prometheus \
  --input nrql_alert.json \
  --output prometheus_rule.yaml
```

### Result: `prometheus_rule.yaml`
```yaml
groups:
  - name: checkout-http-errors
    rules:
      - alert: HighHttp5xxErrorRate
        expr: sum by (host) (rate(http_requests_total{app="checkout",status=~"5.."}[3m])) > 50
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "High HTTP 5xx Error Rate on host {{ $labels.host }}"
          description: "Error rate exceeded 50 errors/sec over 5 minutes."
```

---

## Recipe 3: AWS CloudWatch Alarms &rarr; PromQL & OpenTelemetry

### Objective
Convert AWS CloudWatch metric alarms into open PromQL rules compatible with Prometheus, VictoriaMetrics, and Thanos.

### Step 1: Input `cloudwatch_alarm.json`
```json
{
  "AlarmName": "HighEC2CpuUtilization",
  "MetricName": "CPUUtilization",
  "Namespace": "AWS/EC2",
  "Statistic": "Average",
  "Period": 300,
  "EvaluationPeriods": 2,
  "Threshold": 85.0,
  "ComparisonOperator": "GreaterThanThreshold",
  "Dimensions": [
    {"Name": "AutoScalingGroupName", "Value": "prod-api-asg"}
  ]
}
```

### Step 2: Transpile Command
```bash
dcouplr translate \
  --source cloudwatch \
  --target prometheus \
  --input cloudwatch_alarm.json \
  --output ec2_cpu_rule.yaml
```

### Result: `ec2_cpu_rule.yaml`
```yaml
groups:
  - name: ec2-utilization
    rules:
      - alert: HighEC2CpuUtilization
        expr: avg_over_time(aws_ec2_cpuutilization_average{auto_scaling_group_name="prod-api-asg"}[5m]) > 85.0
        for: 10m
        labels:
          severity: warning
        annotations:
          summary: "EC2 CPU Utilization high in prod-api-asg"
```

---

## Recipe 4: Azure Monitor &rarr; PromQL (Azure Arc & Google Cloud Anthos)

### Objective
Standardize Azure Monitor metric alerts into vendor-agnostic PromQL for multi-cloud deployments across Azure Arc and GCP Anthos.

### Step 1: Input `azure_alert.json`
```json
{
  "name": "AzureAppServiceHighMemory",
  "condition": {
    "allOf": [
      {
        "metricNamespace": "Microsoft.Web/serverfarms",
        "metricName": "MemoryPercentage",
        "operator": "GreaterThan",
        "threshold": 90,
        "timeAggregation": "Average",
        "windowSize": "PT5M"
      }
    ]
  }
}
```

### Step 2: Transpile Command
```bash
dcouplr translate \
  --source azure_monitor \
  --target prometheus \
  --input azure_alert.json \
  --output azure_arc_alert.yaml
```

### Result: `azure_arc_alert.yaml`
```yaml
groups:
  - name: azure-app-service
    rules:
      - alert: AzureAppServiceHighMemory
        expr: avg_over_time(azure_web_serverfarms_memorypercentage_average[5m]) > 90
        for: 5m
        labels:
          severity: critical
```

---

## Recipe 5: HashiCorp Terraform HCL &rarr; OpenTofu Multi-Cloud Landing Zone

### Objective
Migrate proprietary or legacy Terraform provider blocks and network resources to open-source OpenTofu HCL with automated `/24` subnet slicing.

### Step 1: Input `terraform_source.tf`
```hcl
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  tags = {
    Name = "legacy-prod-vpc"
  }
}
```

### Step 2: Transpile Command
```bash
dcouplr translate \
  --source terraform_hcl \
  --target opentofu \
  --input terraform_source.tf \
  --output target_landing_zone.tofu.tf
```

---

## Recipe 6: Cloud-Native Block Replication (AWS MGN, GCP M4CE, Azure Migrate)

### Objective
Generate native hyperscaler replication launch templates and agent installation commands for continuous block-level disk synchronization.

### Execution via REST API
```bash
curl -X POST http://127.0.0.1:8000/v1/telemetry/topology/lift-and-shift \
  -H "Content-Type: application/json" \
  -d '{
    "source_vpc_id": "dc-onprem-core-01",
    "target_cloud": "AWS",
    "target_region": "us-east-1",
    "target_cidr": "10.100.0.0/16"
  }' -o aws_replication_bundle.json
```

---

## Recipe 7: Enterprise Identity (Okta & Entra ID OIDC SSO)

### Objective
Configure corporate Single Sign-On (SSO) with Okta or Microsoft Entra ID to authenticate engineers accessing `<dcouplr/>` Web Studio without password sprawl.

### Environment Configuration
```bash
export DCOUPLR_OIDC_ISSUER="https://yourcompany.okta.com/oauth2/default"
export DCOUPLR_OIDC_CLIENT_ID="0oa1b2c3d4e5f6g7h8"
export DCOUPLR_OIDC_CLIENT_SECRET="secret_token_here"
export DCOUPLR_OIDC_REDIRECT_URI="http://127.0.0.1:8000/v1/auth/callback"
```

---

## Recipe 8: Distributed Raft Consensus Mesh Setup

### Objective
Deploy a 3-node fault-tolerant `<dcouplr/>` cluster in separate Availability Zones inside your private VPC.

### Node Initializations:
```bash
# Node 1 (Bootstrap Leader - 10.100.1.10)
dcouplr serve --cluster-bind 10.100.1.10:8300 --bootstrap-cluster

# Node 2 (Peer - 10.100.2.10)
dcouplr serve --cluster-bind 10.100.2.10:8300 --join 10.100.1.10:8300

# Node 3 (Peer - 10.100.3.10)
dcouplr serve --cluster-bind 10.100.3.10:8300 --join 10.100.1.10:8300
```
