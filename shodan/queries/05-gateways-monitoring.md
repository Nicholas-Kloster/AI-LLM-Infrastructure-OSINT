# 5. AI Gateways, Proxies & Monitoring

The control and observability plane between applications and LLM providers. Gateways centralize provider API keys; observability platforms log every prompt and response — when exposed, they reveal both the secrets and the conversational data flowing through them.

## AI Gateway / Proxy

| Shodan Query | Notes |
|---|---|
| `"LiteLLM" "proxy" port:4000` | |
| `"AI Gateway" port:8787 "Cloudflare"` | |
| `"Kong" "ai-proxy" port:8000` | |
| `"Portkey" port:8787` | |
| `"Helicone" port:8080` | |

## LLM Observability / Monitoring

| Shodan Query | Notes |
|---|---|
| `"Langfuse" port:3000` | Logs every prompt/response |
| `"Langsmith" port:1984` | |
| `"Phoenix" "Arize" port:6006` | Traces |
| `"Prometheus" "/metrics" "llm"` | |

## Document Loaders / Parsers

| Shodan Query | Notes |
|---|---|
| `"Unstructured" port:8000 "/general/v0"` | |
| `"Apache Tika" port:9998` | |
| `"Docling" port:8000` | |
| `"LlamaParse" port:8000` | |
