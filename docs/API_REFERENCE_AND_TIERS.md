# `<dcouplr/>` Community Core API Reference

**Document ID:** `VA-SPEC-API-COMMUNITY-001`  
**Scope:** Local evaluation APIs for Community Core only  
**License:** Apache License 2.0  

---

## Design rule

This document lists **only** endpoints that Community Core users may rely on for local evaluation. It does **not** describe admin, company-governance, Raft join, storage compaction, or other commercial/in-VPC operations surfaces.

Authoritative boundary: [COMMUNITY_CORE.md](COMMUNITY_CORE.md).

---

## Community evaluation endpoints

These assume you are running a local evaluation process on your own host (for example `127.0.0.1`). No cloud credentials are required for Community Core CLI usage; the HTTP surface is optional.

| Method | Path | Auth | Purpose |
| --- | --- | --- | --- |
| `GET` | `/healthz` | None | Liveness / readiness |
| `GET` | `/v1/supported-matrix` | None | Source → target capability matrix |
| `POST` | `/v1/parse` | None | Parse vendor input into UIT-IR |
| `POST` | `/v1/generate` | None | Emit target artifacts from UIT-IR |
| `POST` | `/v1/translate` | None | End-to-end transpile |

### Example

```bash
curl -s http://127.0.0.1:8000/healthz

curl -s -X POST http://127.0.0.1:8000/v1/translate \
  -H "Content-Type: application/json" \
  -d @payload.json
```

Prefer the CLI for day-to-day Community Core work:

```bash
dcouplr translate -i monitor.json -t prometheus -o alert_rules.yaml
```

---

## Explicitly out of scope (not documented here)

- Internal governance consoles and their private HTTP surfaces
- Hidden operator-only OpenAPI groups
- Consensus join / storage compaction / snapshot administration APIs
- License census, upgrade checkout, Dual-DB profile switching
- Paid-edition fleet APIs

If you need commercial or in-VPC enterprise surfaces, use the commercial product documentation outside this repository ([https://dcouplr.com](https://dcouplr.com)).

---

## Related

- [CLI_USER_GUIDE.md](CLI_USER_GUIDE.md)  
- [COMMUNITY_CORE.md](COMMUNITY_CORE.md)  
