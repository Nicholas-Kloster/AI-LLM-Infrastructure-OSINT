# 5. AI Gateways, Proxies & Monitoring

_Section verified: April 2026_

The control and observability plane between applications and LLM providers. Gateways centralize provider API keys; observability platforms log every prompt and response — when exposed, they reveal both the secrets and the conversational data flowing through them.

> Tier legend: 🟥 **T1** unauthenticated by default · 🟧 **T2** auth often misconfigured / known bypasses · 🟨 **T3** recon / fingerprint only.

## AI Gateway / Proxy

| Shodan Query | Tier | Notes |
|---|---|---|
| `"LiteLLM" "proxy" port:4000` | 🟧 T2 | Master key in env — check section 11 |
| `http.html:"litellm" "/chat/completions"` | 🟧 T2 | LiteLLM on non-default ports / behind reverse proxies |
| `"AI Gateway" port:8787 "Cloudflare"` | 🟨 T3 | |
| `"Kong" "ai-proxy" port:8000` | 🟧 T2 | |
| `"kong" http.html:"ai-proxy" OR "ai-prompt"` | 🟧 T2 | Kong AI plugins — proxies provider keys + prompt templates |
| `"tyk" http.html:"ai-"` | 🟧 T2 | Tyk API gateway with AI middleware plugin |
| `"unify" "router"` | 🟨 T3 | Unify AI router — multi-provider fallback, keys in config |
| `"Portkey" port:8787` | 🟨 T3 | |
| `"portkey" "gateway"` | 🟨 T3 | Portkey gateway generic form |
| `"Helicone" port:8080` | 🟧 T2 | |

## LLM Observability / Monitoring

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Langfuse" port:3000` | 🟧 T2 | Logs every prompt/response — high-value PII leak |
| `"Langsmith" port:1984` | 🟧 T2 | |
| `"Phoenix" "Arize" port:6006` | 🟧 T2 | Trace data, often contains prompt history |
| `"Prometheus" "/metrics" "llm"` | 🟧 T2 | |
| `http.title:"PromptLayer"` | 🟧 T2 | PromptLayer observability — logs every prompt/response with keys |

## Document Loaders / Parsers

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Unstructured" port:8000 "/general/v0"` | 🟧 T2 | |
| `"Apache Tika" port:9998` | 🟧 T2 | SSRF history |
| `"Docling" port:8000` | 🟨 T3 | |
| `"LlamaParse" port:8000` | 🟨 T3 | |
