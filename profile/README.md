<div align="center">
  <img src="https://raw.githubusercontent.com/pyhall/.github/main/profile/banner.svg" alt="pyhall — Worker Class Protocol" width="100%"/>
</div>

<br/>

<div align="center">

> Stripe is the trust signal for payments. AWS is the trust signal for infrastructure.
> **pyhall is the trust signal for AI worker governance.**

[![WCP Spec](https://img.shields.io/badge/WCP-v0.1-0050D4?style=flat-square)](https://workerclassprotocol.dev)
[![PyPI](https://img.shields.io/pypi/v/pyhall-wcp?style=flat-square&label=PyPI&color=0050D4)](https://pypi.org/project/pyhall-wcp/)
[![npm](https://img.shields.io/npm/v/@pyhall/core?style=flat-square&label=npm&color=0050D4)](https://www.npmjs.com/package/@pyhall/core)
[![License](https://img.shields.io/badge/license-Apache%202.0-0050D4?style=flat-square)](https://github.com/pyhall/pyhall-python/blob/main/LICENSE)
[![pyhall.dev](https://img.shields.io/badge/pyhall.dev-docs-0050D4?style=flat-square)](https://pyhall.dev)

</div>

---

Every agentic system dispatches workers. Almost none of them ask: *should this worker be trusted with this job, under these conditions, with this data?*

pyhall answers that question — with a cryptographic evidence receipt for every routing decision.

---

## How it works

Trust is bound to the **full worker package**. Every execution re-verifies. Fail-closed on any mismatch.

```mermaid
flowchart LR
    subgraph BUILD ["  BUILD ONCE  "]
        direction TB
        B1["worker.py\n+ support files"] --> B2["Canonicalize\npackage"]
        B2 --> B3["SHA-256\npackage hash"]
        B3 --> B4["Sign with\nnamespace key\norg.* · x.*"]
        B4 --> B5["Attestation\ncredential (JSON)"]
        B5 --> REG[("Registry")]
    end

    subgraph RUN ["  EVERY EXECUTION  "]
        direction TB
        AG(["Agent"]) -->|"capability request"| PG["Policy Gate\n+ Blast Radius"]
        PG --> VER["Verify via Registry\nhash match · sig valid\nnamespace authorized"]
        VER -->|"ALLOW"| RD["Dispatch worker\n+ evidence receipt\ndecision_id · artifact_hash"]
        VER -->|"DENY"| REJ(["Rejected\nfail-closed"])
        RD --> WK(["Worker executes"])
        WK -->|"result"| AG
    end

    REG -.->|"attested hash\n+ signer state"| VER

    style BUILD fill:#f8faff,stroke:#0050D4,color:#0f172a
    style RUN fill:#f8faff,stroke:#0050D4,color:#0f172a
    style VER fill:#dbe8ff,stroke:#0050D4,color:#0f172a
    style RD fill:#dbe8ff,stroke:#0050D4,color:#0f172a
    style REG fill:#0050D4,stroke:#003a9b,color:#ffffff
```

---

## Four pillars

| | |
|---|---|
| **Govern** | Define who can run what worker, at what capability level, under which policy. Workers operate inside declared boundaries — or not at all. |
| **Attest** | Every worker carries a capability card. Every decision produces a verifiable proof — artifact hash, timestamp, correlation ID. |
| **Route** | Match work to workers by capability, policy tier, and QoS. Mismatches are rejected at the gate, not silently degraded at runtime. |
| **Observe** | Every routing decision is logged with proof. Query history, audit decisions, trace incidents. Live worker health in one monitor view. |

---

## The stack

| Component | Repo | Install |
|-----------|------|---------|
| **Python SDK** | [pyhall-python](https://github.com/pyhall/pyhall-python) | `pip install pyhall-wcp` |
| **TypeScript SDK** | [pyhall-typescript](https://github.com/pyhall/pyhall-typescript) | `npm install @pyhall/core` |
| **Go SDK** | [pyhall-go](https://github.com/pyhall/pyhall-go) | `go get github.com/pyhall/pyhall-go` |
| **CLI** | [pyhall-cli](https://github.com/pyhall/pyhall-cli) | `npm install -g @pyhall/cli` |
| **Hall Monitor** | [pyhall-desktop](https://github.com/pyhall/pyhall-desktop) | Desktop app — Mac / Win / Linux |
| **Taxonomy** | [pyhall-taxonomy](https://github.com/pyhall/pyhall-taxonomy) | 245 entities · 127 capabilities |
| **Registry** | [pyhall-registry](https://github.com/pyhall/pyhall-registry) | `api.pyhall.dev` |

---

## Quick start

```bash
pip install pyhall-wcp
```

```python
from pyhall import Hall

hall = Hall()
decision = hall.route({
    "capability_id": "cap.doc.summarize",
    "env": "prod",
    "data_label": "INTERNAL",
    "tenant_risk": "low",
    "qos_class": "P1",
    "tenant_id": "your-tenant",
    "correlation_id": "uuid-v4-here",
})

if not decision.denied:
    print(f"Dispatch: {decision.selected_worker_species_id}")
    print(f"Evidence: {decision.decision_id}")
    print(f"Hash:     {decision.artifact_hash}")
```

---

## Four steps to governed workers

```
01 PUBLISH    hall publish worker.wcp.yaml       Define a worker capability card
02 ENROLL     hall enroll --namespace x.you      Register with the pyhall registry
03 ROUTE      hall route --cap cap.doc.summarize Match request to the right worker
04 OBSERVE    hall decisions list                Audit every decision ever made
```

---

<div align="center">

**[pyhall.dev](https://pyhall.dev)** &nbsp;·&nbsp; **[WCP Spec](https://workerclassprotocol.dev)** &nbsp;·&nbsp; **[Registry](https://api.pyhall.dev)** &nbsp;·&nbsp; **[Docs](https://pyhall.dev/introduction/)**

</div>
