# 5. AI Gateways, Proxies & Monitoring

_Section verified: April 2026_

The control and observability plane between applications and LLM providers. Gateways centralize provider API keys; observability platforms log every prompt and response — when exposed, they reveal both the secrets and the conversational data flowing through them.

> Tier legend: 🟥 **T1** unauthenticated by default · 🟧 **T2** auth often misconfigured / known bypasses · 🟨 **T3** recon / fingerprint only.

## AI Gateway / Proxy

| Shodan Query | Tier | Notes |
|---|---|---|
| `"LiteLLM" "proxy" port:4000` | 🟧 | Master key in env — check section 11 |
| `http.html:"litellm" "/chat/completions"` | 🟧 | LiteLLM on non-default ports / behind reverse proxies |
| `"AI Gateway" port:8787 "Cloudflare"` | 🟨 | |
| `"Kong" "ai-proxy" port:8000` | 🟧 | |
| `"kong" http.html:"ai-proxy" OR "ai-prompt"` | 🟧 | Kong AI plugins — proxies provider keys + prompt templates |
| `"tyk" http.html:"ai-"` | 🟧 | Tyk API gateway with AI middleware plugin |
| `"unify" "router"` | 🟨 | Unify AI router — multi-provider fallback, keys in config |
| `"Portkey" port:8787` | 🟨 | |
| `"portkey" "gateway"` | 🟨 | Portkey gateway generic form |
| `"Helicone" port:8080` | 🟧 | |

## LLM Observability / Monitoring

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Langfuse" port:3000` | 🟧 | Logs every prompt/response — high-value PII leak |
| `"Langsmith" port:1984` | 🟧 | |
| `"Phoenix" "Arize" port:6006` | 🟧 | Trace data, often contains prompt history |
| `"Prometheus" "/metrics" "llm"` | 🟧 | |
| `http.title:"PromptLayer"` | 🟧 | PromptLayer observability — logs every prompt/response with keys |

## Document Loaders / Parsers

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Unstructured" port:8000 "/general/v0"` | 🟧 | |
| `"Apache Tika" port:9998` | 🟧 | SSRF history |
| `"Docling" port:8000` | 🟨 | |
| `"LlamaParse" port:8000` | 🟨 | |
