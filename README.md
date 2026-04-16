# AI/LLM Infrastructure OSINT

> Open-source intelligence and security research focused on the exposed control plane of modern AI/ML infrastructure.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Research: Authorized Only](https://img.shields.io/badge/Research-Authorized%20Only-red.svg)](DISCLAIMER.md)
[![Maintained by NuClide](https://img.shields.io/badge/Maintained%20by-NuClide-purple.svg)](#about)

---

## Mission

The AI/ML stack moved faster than its security model. Vector databases ship without auth by default. LLM gateways log every prompt to disk. Inference servers expose `/v1/models` to the internet. Fine-tuning dashboards proxy GPU compute to anyone who finds the URL.

This repository is a living catalogue of **fingerprints, queries, exposure patterns, and detection logic** for the infrastructure that runs modern AI — built so that defenders can find their own assets before adversaries do.

## Scope

| Category | Examples |
|----------|----------|
| **LLM Orchestration** | Flowise, Langflow, Dify, Open WebUI, LiteLLM, Ollama |
| **Vector Databases** | ChromaDB, Qdrant, Weaviate, Milvus, pgvector, Redis Search |
| **Model Serving** | vLLM, Triton, TGI, llama.cpp, OpenAI-compatible endpoints |
| **Training & Experiments** | MLflow, Kubeflow, Ray, ClearML, Argilla, Label Studio |
| **AI Gateways & Observability** | LiteLLM Proxy, Portkey, Langfuse, Helicone, Phoenix/Arize |
| **Agent Frameworks** | SuperAGI, AgentGPT, MetaGPT, OpenDevin, CrewAI |
| **Credential & Config Leaks** | OPENAI_API_KEY exposure, unauth Docker, snapshot buckets |
| **GPU & Compute Dashboards** | NVIDIA DCGM, Ray dashboard, RunPod, Vast.ai |

## Repository Structure

```
.
├── shodan/                         # Shodan query reference
│   ├── Shodan_AI_Reference.pdf     # Original PDF (downloadable artifact)
│   └── queries/                    # Per-category markdown sources
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
- [Shodan Query Index](shodan/README.md)
- [Download the polished PDF reference](shodan/Shodan_AI_Reference.pdf)

**Search across all queries:**
```bash
git clone https://github.com/Nicholas-Kloster/AI-LLM-Infrastructure-OSINT.git
cd AI-LLM-Infrastructure-OSINT
grep -r "qdrant" shodan/queries/
```

## Roadmap

- [x] Shodan reference (v1) — 9 categories, ~120 queries
- [ ] Censys query equivalents
- [ ] FOFA / ZoomEye / Hunter.how queries
- [ ] GitHub dorks for leaked AI keys (`OPENAI_API_KEY`, `ANTHROPIC_API_KEY`, `HF_TOKEN`)
- [ ] Nuclei templates for unauth detection (ChromaDB, Qdrant, Ollama, vLLM)
- [ ] MCP server exposure research
- [ ] Local model server fingerprinting (LM Studio, Jan, GPT4All)
- [ ] Anonymized case studies of real exposures
- [ ] Defensive hardening guides per category

## Contributing

PRs welcome — see [CONTRIBUTING.md](CONTRIBUTING.md). The bar is:
- Queries should be **verifiable** (you've seen them return real results).
- Add a `Notes` column when the query reveals something specific (auth state, version disclosure, snapshot exposure).
- New categories should map to a real, deployed-in-the-wild AI/ML platform.

## Disclaimer

Read [DISCLAIMER.md](DISCLAIMER.md). Short version: this material is for **authorized security research, defensive asset discovery, and threat hunting only**. Touching infrastructure you don't own or have explicit permission to test is illegal in most jurisdictions. Don't.

## About

Maintained by **NuClide** — independent ICS/OT and AI infrastructure security research.

CISA disclosures: [CVE-2025-4364](https://nvd.nist.gov/vuln/detail/CVE-2025-4364), [ICSA-25-140-11](https://www.cisa.gov/news-events/ics-advisories/icsa-25-140-11).

Contact: [@Nicholas-Kloster](https://github.com/Nicholas-Kloster)
