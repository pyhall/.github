# PyHall — The Trust Signal for AI Worker Governance

> Stripe is the trust signal for payments. AWS is the trust signal for infrastructure.
> **PyHall is the trust signal for AI worker governance.**

Every agentic system dispatches workers. Almost none of them ask: *should this worker be trusted with this job, under these conditions, with this data?*

PyHall answers that question — with a cryptographic evidence receipt for every decision.

---

## What It Does

When an agent needs a capability, it contacts the Hall. The Hall routes the request to the right worker — after verifying controls, computing blast radius, and enforcing governance policy. Every routing decision produces a **RouteDecision**: an immutable evidence receipt your security team can actually read.

- **Capability routing** — agents request what they need (`cap.doc.summarize`), not which tool to call
- **Blast radius scoring** — computed before anything executes
- **Worker attestation** — SHA-256 artifact hashes verify workers haven't been tampered with
- **Ban propagation** — compromised workers blocked across the network instantly
- **Policy gates** — MSAVX step-up, human-in-the-loop, data label enforcement, all built in

---

## The Stack

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

## Quick Start

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
```

---

## The Protocol

PyHall implements **WCP — Worker Class Protocol** — the open standard for AI worker governance.

WCP is language-agnostic. The Python, TypeScript, and Go SDKs are all conformant reference implementations. Build on any of them; the RouteDecision evidence receipts are identical.

[![WCP Spec](https://img.shields.io/badge/WCP-v0.1-0050D4?style=flat-square)](https://workerclassprotocol.dev)
[![PyPI](https://img.shields.io/pypi/v/pyhall-wcp?style=flat-square&label=PyPI)](https://pypi.org/project/pyhall-wcp/)
[![npm](https://img.shields.io/npm/v/@pyhall/core?style=flat-square&label=npm)](https://www.npmjs.com/package/@pyhall/core)
[![License](https://img.shields.io/badge/license-Apache%202.0-green?style=flat-square)](https://github.com/pyhall/pyhall-python/blob/main/LICENSE)

---

**[pyhall.dev](https://pyhall.dev)** · **[WCP Spec](https://workerclassprotocol.dev)** · **[Registry](https://api.pyhall.dev)**
