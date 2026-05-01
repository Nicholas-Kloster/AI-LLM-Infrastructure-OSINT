# University AI Infrastructure Exposure — Global Overview

_NuClide Research · 2026-05-01_

---

## Scale

Shodan returns **225 university Ollama instances** (port 11434) and **84 university Open WebUI instances** (port 3000) as of 2026-05-01. The following findings were confirmed through active probing on the same date.

---

## Confirmed Exposures by Severity

### CRITICAL — Cloud Proxy Live (200 OK)

Direct quota drain at operator expense, no authentication:

| Institution | Country | Model | Tokens |
|-------------|---------|-------|--------|
| Purdue University Northwest | US-IN | qwen3-coder-next:cloud | 4 |
| Purdue University Northwest | US-IN | gemma4:31b-cloud | 2 |
| Purdue University Northwest | US-IN | gpt-oss:20b-cloud | **61** |
| SUNY Buffalo | US-NY | gemma4:31b-cloud | 2 |

### CRITICAL — Auth Disabled

Open inference for any internet actor:

| Institution | Country | Instance Name |
|-------------|---------|---------------|
| UC Santa Barbara | US-CA | "AI Lab (Open WebUI)" |

### CRITICAL — Cloud Proxy + Credential Leak

Operator API credentials (SSH pubkey + username) in 401 response:

| Institution | Country | Username | Leak Type |
|-------------|---------|----------|-----------|
| Columbia University | US-NY | `seascvn066` | Personal account |
| Chulalongkorn University | Thailand | `llm` | Generic service account |
| Technical Univ. of Crete | Greece | `arian` | Personal account |
| Hanoi University | Vietnam | `04aa6fb5e0b8` | Docker container ID |

### HIGH — Large Cloud Proxy Portfolio (No 200 OK, No Cred Leak)

| Institution | Country | Cloud Proxies | Largest |
|-------------|---------|---------------|---------|
| POSTECH | South Korea | 18 | kimi-k2:1t-cloud (1T params) |
| Shiv Nadar University | India | 18 | deepseek-v3.1:671b-cloud |
| Hanoi University | Vietnam | 18 | multiple |
| KTH Royal Inst. of Tech. | Sweden | 2 (2 nodes) | — |
| Keio University | Japan | 2 | — |
| Univ. of Western Ontario | Canada | 1 | — |
| University of Newcastle | Australia | 1 | — |
| JKUAT | Kenya | 1 | — |

---

## Abliterated / Uncensored Models on University Servers

Safety fine-tuning removed by design. Accessible to unauthenticated callers:

| Institution | Country | Model |
|-------------|---------|-------|
| KTH Royal Inst. of Tech. | Sweden | `hf.co/OBLITERATUS/gemma-4-E4B-it-OBLITERATED:latest` (running as **root**) |
| Shiv Nadar University | India | `vishalraj/qwen3-30b-abliterated:latest` + `uandinotai/dolphin-uncensored:latest` |
| Brno Univ. of Technology | Czech Republic | `seamon67/Gemma3-Abliterated:27b-q4_K_M` |

---

## Agentic Models with Tool Execution

| Institution | Country | Model | Capability |
|-------------|---------|-------|------------|
| Duke University | US-NC | `qwen3.6-27b-agent:latest` | "Prefer using available tools to inspect files" |

---

## Largest Deployments

| Institution | Country | Total Models | Largest Local Model |
|-------------|---------|-------------|---------------------|
| Shiv Nadar University | India | 76 | DeepSeek-V3-0324:671b (376GB) |
| SUNY Buffalo | US-NY | 26 | mixtral:8x22b (74GB) |
| POSTECH | South Korea | 31 | (mostly cloud) |
| Hanoi University | Vietnam | 31 | (mostly cloud) |
| NTUA Athens | Greece | 20 | deepseek-coder-v2:236b (123GB) |

---

## Geographic Coverage

| Region | Institutions Confirmed | Notes |
|--------|----------------------|-------|
| North America | Columbia, UCSB, Duke, SUNY Buffalo, Purdue NW, UWO, Dalhousie | 7 |
| Asia-Pacific | POSTECH, Shiv Nadar, Hanoi, Chulalongkorn, Keio, Newcastle | 6 |
| Europe | KTH, TechCrete, NTUA, Brno, Aberystwyth | 5 |
| Africa | JKUAT, Covenant | 2 |

---

## Attack Patterns Documented

1. **Open WebUI auth bypass**: UI auth on port 3000 does not protect raw Ollama on port 11434
2. **Cloud proxy quota drain**: Free-tier cloud models (gemma4-cloud, gpt-oss-cloud, qwen3-coder-next-cloud) return 200 OK without credentials
3. **Credential leak via 401**: Ollama Connect username + SSH pubkey in 401 error response body
4. **Docker binding misconfig**: `-p 11434:11434` binds to 0.0.0.0 by default
5. **Agent model injection**: File inspection agents injectable via CVE-2025-63389
6. **RAG pipeline injection**: Embedding models signal active RAG pipelines; injection affects document-augmented responses

---

## Common Fix

```bash
OLLAMA_HOST=127.0.0.1:11434
systemctl restart ollama

# Docker:
docker run -p 127.0.0.1:11434:11434 ollama/ollama
```

**CVE-2025-63389** — All Ollama versions. `first_patched_version: null`.
