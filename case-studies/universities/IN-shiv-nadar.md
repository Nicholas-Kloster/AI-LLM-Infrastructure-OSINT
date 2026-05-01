# Shiv Nadar University — 76 Models Including 376GB Local DeepSeek + 18 Cloud Subscriptions

_NuClide Research · 2026-05-01_

---

## Summary

Shiv Nadar Institution of Eminence (India, Noida) is running a shared AI infrastructure cluster with multiple exposed nodes, collectively serving 76 models including a 376GB local DeepSeek-V3-0324 (671B parameters), `qwen3:235b` (132GB), and 18 cloud proxy subscriptions. Raw Ollama port publicly accessible on multiple nodes.

---

## Infrastructure

| Node | IP | Hostname |
|------|-----|----------|
| Node 1 | 103.27.166.36 | 36-166-27-103.noida.snu.in |
| Node 2 | 103.27.166.38 | 38-166-27-103.noida.snu.in |
| Node 3 | 103.27.166.39 | 39-166-27-103.noida.snu.in |

All nodes in the 103.27.166.0/24 subnet (`noida.snu.in`). All bind Ollama to 0.0.0.0:11434 without authentication.

---

## Model Scale (per node, 76 models)

| Model | Size | Notes |
|---|---|---|
| lordoliver/DeepSeek-V3-0324:671b-q4_k_m | **376 GB** | 671B parameter DeepSeek |
| qwen3:235b | 132 GB | 235B MoE model |
| llama4:latest | 62 GB | Local |
| gpt-oss:120b | 60 GB | OpenAI open model |
| llama3.2-vision:90b | 50 GB | Vision-language, 90B |
| (53 more local models) | varies | |
| (18 cloud proxy models) | 0 GB each | DeepSeek, MiniMax, Kimi, Qwen, GLM, Gemini, Nemotron, NVIDIA |

---

## Findings

### F1 — Multi-Node Cluster Fully Exposed (CRITICAL)

Three nodes on the `noida.snu.in` subnet all expose port 11434 without authentication. The shared model set is injectable across all nodes simultaneously.

### F2 — 671B Local Model Accessible for Free Inference (HIGH)

`lordoliver/DeepSeek-V3-0324:671b-q4_k_m` (376GB) is accessible without authentication. Any internet actor can run inference against one of the most capable open-weight models on dedicated university GPU infrastructure at the operator's cost.

### F3 — 18 Cloud Subscriptions Exposed (CRITICAL)

Same cloud proxy portfolio as POSTECH (shared Ollama Connect account or same ecosystem): DeepSeek, MiniMax, Kimi, Qwen, GLM, Gemini, Nemotron/NVIDIA.

### F4 — Model Injection on All 76 Models (CRITICAL)

CVE-2025-63389 applies to all 76 models across all three nodes. Any researcher on this shared cluster using these models receives outputs under attacker-controlled instructions after injection.

---

## Remediation

```bash
# All nodes
OLLAMA_HOST=127.0.0.1:11434
systemctl restart ollama
```

---

## Disclosure

- **Discovered:** 2026-05-01
- **Status:** Pending outreach to Shiv Nadar IT Security
