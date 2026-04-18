# AI/LLM Infrastructure OSINT

> Open-source intelligence and security research focused on the exposed control plane of modern AI/ML infrastructure.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Research: Authorized Only](https://img.shields.io/badge/Research-Authorized%20Only-red.svg)](DISCLAIMER.md)
[![Maintained by NuClide](https://img.shields.io/badge/Maintained%20by-NuClide-purple.svg)](#about)
[![Reference: v2.1](https://img.shields.io/badge/Reference-v2.1%20%C2%B7%20Apr%202026-teal.svg)](shodan/Shodan_AI_Reference.pdf)

---

## Mission

The AI/ML stack moved faster than its security model. Vector databases ship without auth by default. LLM gateways log every prompt to disk. Inference servers expose `/v1/models` to the internet. Fine-tuning dashboards proxy GPU compute to anyone who finds the URL. MCP servers wire shell, filesystem, and database tools to LLMs over unauthenticated HTTP.

This repository is a living catalogue of **fingerprints, queries, exposure patterns, and detection logic** for the infrastructure that runs modern AI — built so that defenders can find their own assets before adversaries do.

## Scope

| Category | Examples |
|----------|----------|
| **LLM Orchestration** | Flowise, Langflow, Dify, Open WebUI, LiteLLM, Ollama, Clawdbot |
| **Vector Databases** | ChromaDB, Qdrant, Weaviate, Milvus, pgvector, Redis Search, ClickHouse, Cassandra |
| **Object Storage & Artifact Stores** | MinIO, Harbor, Docker Registry v2 — where the models and vectors actually live |
| **Model Serving** | vLLM, Triton, TGI, llama.cpp, LM Studio, GPT4All, NVIDIA NIM |
| **Training & Experiments** | MLflow, Kubeflow, Ray, ClearML, Argilla, Label Studio, Feast |
| **AI Gateways & Observability** | LiteLLM Proxy, Portkey, Langfuse, Helicone, Phoenix/Arize |
| **Agent Frameworks** | SuperAGI, OpenDevin, MetaGPT, Clawdbot, AutoGen |
| **RAG Stacks & Self-Hosted AI Apps** | h2oGPT, Danswer/Onyx, Quivr, Khoj, RAGFlow, LibreChat |
| **Image Generation** | ComfyUI, Stable Diffusion, AUTOMATIC1111, InvokeAI, Fooocus |
| **AI Code Assistants** | Tabby, self-hosted Cody, Continue, Refact |
| **MCP Servers** | Model Context Protocol exposed over HTTP/SSE |
| **Credential & Config Leaks** | `OPENAI_API_KEY` / `ANTHROPIC_API_KEY` / `GROQ_API_KEY` / `HF_TOKEN` exposure, `.env` files |
| **Container & Orchestration** | Docker daemon, Kubernetes, kubelet, etcd, Consul, Vault |
| **GPU & Compute Dashboards** | NVIDIA DCGM, Ray dashboard, RunPod, Vast.ai, GPUStack |

## Repository Structure

```
.
├── reference/                      # Tool-agnostic reference material
│   ├── ports.md                    # Common AI/LLM infrastructure ports
│   └── terminology.md              # AI/ML stack terminology primer
├── shodan/                         # Shodan query reference
│   ├── Shodan_AI_Reference.pdf     # Polished PDF (v2.1, April 2026)
│   └── queries/                    # Per-category markdown sources
│       ├── 01-llm-orchestration.md
│       ├── 02-vector-databases.md  # incl. Object Storage & Artifact Stores
│       ├── 03-model-serving.md
│       ├── 04-training-experiments.md
│       ├── 05-gateways-monitoring.md
│       ├── 06-agent-frameworks.md
│       ├── 07-rag-stacks.md        # new in v2
│       ├── 08-image-generation.md  # new in v2
│       ├── 09-code-assistants.md   # new in v2
│       ├── 10-mcp-servers.md       # new in v2
│       ├── 11-credential-leaks.md
│       ├── 12-containers.md        # expanded in v2
│       ├── 13-backup-snapshot.md
│       ├── 14-gpu-compute.md
│       ├── 15-fingerprinting.md
│       └── appendix-cve.md         # new in v2
├── censys/                         # Censys equivalents (planned)
├── fofa/                           # FOFA queries (planned)
├── zoomeye/                        # ZoomEye queries (planned)
├── dorks/                          # Google / GitHub dorks (planned)
├── nuclei-templates/               # Detection templates (planned)
├── case-studies/                   # Anonymized exposure writeups (planned)
├── DISCLAIMER.md
├── CONTRIBUTING.md
└── LICENSE
```

## Quick Start

**Browse by category:**
- [Shodan Query Index](shodan/README.md) — 15 categories + CVE cross-reference appendix
- [Common AI/LLM Ports Reference](reference/ports.md)
- [AI/ML Terminology Primer](reference/terminology.md)
- [Download the polished PDF reference (v2.1)](shodan/Shodan_AI_Reference.pdf)

**Search across all queries:**
```bash
git clone https://github.com/Nicholas-Kloster/AI-LLM-Infrastructure-OSINT.git
cd AI-LLM-Infrastructure-OSINT
grep -r "qdrant" shodan/queries/
grep -rn " T1 " shodan/queries/    # all unauth-by-default queries
```

## Tier System

Every query in v2.x is tagged with an exposure tier:

- **T1** — Unauthenticated by default. A positive hit is typically a live, interactive target.
- **T2** — Requires misconfiguration or has known auth-bypass CVEs. One additional probe confirms exposure.
- **T3** — Recon / fingerprint only. Use for inventory and pivoting, not as an immediate finding.

See [shodan/README.md](shodan/README.md#how-to-read-the-tables) for the full legend.

## Roadmap

- [x] Shodan reference (v1) — 9 categories, ~120 queries
- [x] Common AI/LLM ports reference
- [x] **Shodan reference (v2)** — 15 categories + CVE appendix, exposure tiers, RAG/Image-gen/Code-assistants/MCP added
- [x] **Shodan reference (v2.1)** — Object Storage subsection, GPT4All / NVIDIA NIM / AutoGen, ClickHouse / Cassandra / txtai, Feast / Tecton, terminology primer
- [x] MCP server exposure research
- [ ] Censys query equivalents
- [ ] FOFA / ZoomEye / Hunter.how queries
- [ ] GitHub dorks for leaked AI keys (`OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, `HF_TOKEN`, `GROQ_API_KEY`)
- [ ] Nuclei templates for unauth detection (ChromaDB, Qdrant, Ollama, vLLM, ComfyUI, MCP/SSE)
- [ ] Local model server fingerprinting expansion (LM Studio, Jan, GPT4All)
- [ ] Anonymized case studies of real exposures
- [ ] Defensive hardening guides per category

## Contributing

PRs welcome — see [CONTRIBUTING.md](CONTRIBUTING.md). The bar is:
- Queries should be **verifiable** (you've seen them return real results).
- Tag every new query with an exposure tier (T1/T2/T3).
- Add a `Notes` column when the query reveals something specific (auth state, version disclosure, snapshot exposure).
- New categories should map to a real, deployed-in-the-wild AI/ML platform.

## Disclaimer

Read [DISCLAIMER.md](DISCLAIMER.md). Short version: this material is for **authorized security research, defensive asset discovery, and threat hunting only**. Touching infrastructure you don't own or have explicit permission to test is illegal in most jurisdictions. Don't.

## About

Maintained by **NuClide** — independent ICS/OT and AI infrastructure security research.

CISA disclosures: [CVE-2025-4364](https://nvd.nist.gov/vuln/detail/CVE-2025-4364), [ICSA-25-140-11](https://www.cisa.gov/news-events/ics-advisories/icsa-25-140-11).

Companion tooling: [aimap](https://github.com/Nicholas-Kloster/aimap) — AI/ML infrastructure scanner that defenders can run against their own networks.

Contact: [@Nicholas-Kloster](https://github.com/Nicholas-Kloster)
