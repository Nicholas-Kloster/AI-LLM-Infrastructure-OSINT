# POSTECH — 18 Cloud Subscriptions Including 1-Trillion-Parameter Model

_NuClide Research · 2026-05-01_

---

## Summary

Pohang University of Science and Technology (POSTECH), South Korea's top STEM research university, is running a 31-model Ollama instance with 18 active cloud proxy subscriptions — including `kimi-k2:1t-cloud` (Kimi K2 Thinking, 1 trillion parameters) and `deepseek-v3.1:671b-cloud`. Raw Ollama port publicly accessible, no authentication. All 31 models injectable via CVE-2025-63389.

---

## Infrastructure

| Field | Value |
|---|---|
| IP | 141.223.84.47 |
| Org | Pohang University of Science and Technology |
| Country | South Korea |
| Open ports | 11434 (Ollama — **public**) |

---

## Cloud Proxy Subscriptions (18)

| Model | Provider | Notes |
|---|---|---|
| kimi-k2:1t-cloud | Moonshot AI | **1 trillion parameter model** |
| deepseek-v3.1:671b-cloud | DeepSeek | 671B parameter model |
| qwen3-coder:480b-cloud | Alibaba Qwen | 480B coding model |
| gpt-oss:120b-cloud | OpenAI | 120B GPT-OSS |
| kimi-k2.6:cloud | Moonshot AI | — |
| kimi-k2.5:cloud | Moonshot AI | — |
| kimi-k2-thinking:cloud | Moonshot AI | — |
| glm-5.1:cloud | Zhipu AI | — |
| glm-5:cloud | Zhipu AI | — |
| glm-4.7:cloud | Zhipu AI | — |
| glm-4.6:cloud | Zhipu AI | — |
| deepseek-v4-pro:cloud | DeepSeek | — |
| deepseek-v4-flash:cloud | DeepSeek | — |
| deepseek-v3.2:cloud | DeepSeek | — |
| minimax-m2.7:cloud | MiniMax | — |
| minimax-m2.5:cloud | MiniMax | — |
| minimax-m2.1:cloud | MiniMax | — |
| minimax-m2:cloud | MiniMax | — |
| qwen3.5:cloud | Alibaba | — |
| qwen3-coder-next:cloud | Alibaba | — |
| nemotron-3-super:cloud | NVIDIA | — |
| gemini-3-flash-preview:cloud | Google | — |

---

## Findings

### F1 — 18 Cloud Subscriptions Exposed (CRITICAL)

All 18 cloud proxy subscriptions are accessible on the unauthenticated port. Any internet actor can:
- Enumerate all cloud subscriptions via `/api/tags`
- Inject system prompts into cloud proxy models via CVE-2025-63389
- Drain operator API quotas through the exposed port

The subscription portfolio includes frontier models: Kimi K2 (1T), DeepSeek V3.1 (671B), Qwen3-Coder (480B).

### F2 — Model Injection on Research Infrastructure (CRITICAL)

All 31 models injectable. POSTECH researchers using these models receive outputs shaped by injected system prompts.

---

## Remediation

```bash
OLLAMA_HOST=127.0.0.1:11434
systemctl restart ollama
```

---

## Disclosure

- **Discovered:** 2026-05-01
- **Status:** Pending outreach to POSTECH IT Security
