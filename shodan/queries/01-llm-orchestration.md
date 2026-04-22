# 1. LLM Orchestration Platforms

_Section verified: April 2026_

Low-code/no-code builders, agent runtimes, and chain orchestrators. These platforms typically expose a web UI that, when unauthenticated, grants direct access to flow editors, API keys stored in nodes, and execution endpoints.

> Tier legend: **T1** unauthenticated by default · **T2** auth often misconfigured / known bypasses · **T3** recon / fingerprint only. See [shodan/README.md](../README.md#how-to-read-the-tables).

## Flowise

| Shodan Query | Tier | Notes |
|---|---|---|
| `title:"Flowise" port:443` | T3 | 586 hits — title fingerprint, mixed auth |
| `product:"Flowise"` | T2 | 576 hits — Shodan product facet, canonical fingerprint |
| `http.html:"Low-code LLM apps builder"` | T3 | 572 hits — HTML title fingerprint |
| `"Flowise"` | T3 | 170 hits — broad banner match |
| `"Flowise" http.status:200` | T2 | 26 hits — 200-response subset, likely reachable |
| `"Flowise" "chatflows"` | T3 | 9 hits — banner + API term |
| `"Flowise" "Express"` | T3 | 6 hits — Express-fronted subset |
| `"X-Powered-By: Express" "Flowise"` | T3 | 6 hits — header + banner intersect |
| `"Flowise" http.status:401` | T3 | 4 hits — auth-enabled subset |

**Deployment note:** As of April 2026, `port:3000` (Flowise default) is effectively dead on the public internet — production deployments now sit behind 443 reverse proxies almost universally. Prefer `product:"Flowise"` as the canonical fingerprint.

**CVE note:** CVE-2024-36420 (auth bypass via path traversal) affects Flowise < 1.8.2. Exposed pre-auth instances are RCE candidates, not just info disclosure.

## Other Orchestrators

| Shodan Query | Tier | Notes |
|---|---|---|
| `http.title:"Open WebUI"` | T2 | 18,736 hits — largest AI UI fingerprint on the internet; Ollama frontend |
| `http.html:"dify"` | T3 | 8,750 hits — broad Dify HTML fingerprint |
| `http.title:"LiteLLM"` | T2 | 5,076 hits — LLM proxy, master key often leaked in env |
| `"Jan" port:1337` | T3 | 4,624 hits — desktop app in server mode |
| `http.title:"Dify"` | T3 | 2,614 hits — tighter Dify title fingerprint |
| `http.title:"Clawdbot Control"` | T1 | 1,770 hits ⚠️ `http.title:` is tokenized — sample before trusting, may include false positives |
| `http.html:"Chainlit"` | T2 | 1,144 hits — conversational UI layer on LangChain |
| `http.title:"Langflow"` | T2 | 844 hits — flow builder, often deployed unauth |
| `"AnythingLLM" port:3001` | T2 | 366 hits — known auth bypass history |
| `http.title:"Gradio"` | T3 | 225 hits — generic Gradio wrapper (covers oobabooga, demos, custom AI apps) |
| `port:18789 ("openclaw" OR "clawdbot")` | T1 | 165 hits — OpenClaw gateway (grouped OR required; unparenthesized breaks Shodan precedence) |
| `"LocalAI" port:8080` | T1 | 95 hits — no auth by default |
| `"Ollama" port:11434` | T1 | 37 hits — no auth support; exposure = full access |
| `http.html:"AutoGPT"` | T3 | 32 hits — project moribund since 2025, retained for completeness |
| `http.favicon.hash:-1404538293` | T3 | 11 hits — LlamaIndex favicon |
| `"LangChain" port:8000` | T3 | 6 hits — library fingerprint, app varies |
| `http.title:"Create Llama App"` | T2 | 6 hits — LlamaIndex default UI (RAG starter) |

**Verified April 2026.** Deployment note: the "service + default port" pattern that dominated 2024 is largely dead — most platforms moved behind 443/80 reverse proxies. Queries below 10 hits are retained when they still identify the platform uniquely.

**Dropped (verified 0 reliable hits across all variants tried):**
- **Haystack** — `"Haystack"` is a generic term polluting every fingerprint (search tools, monitoring, GDS frameworks); no clean way to isolate deepset's RAG framework on Shodan in April 2026
- **PrivateGPT** — strongest variants were `"zylon"` (73, noisy) and `http.html:"privategpt"` (7); not worth cataloguing
- **text-generation-webui (oobabooga)** — 0 to 8 hits across every variant; covered incidentally by `http.title:"Gradio"` above

**OpenClaw / Clawdbot:** This is not a passive reconnaissance target. A publicly reachable OpenClaw gateway is an agent with shell execution, browser automation, email send, and calendar write on whoever deployed it. Treat positive hits as live compromise surface, not data disclosure.

## Prompt / Chain Management

| Shodan Query | Tier | Notes |
|---|---|---|
| `"PromptFlow" port:8080 "Microsoft"` | T3 | |
| `"n8n" port:5678 "workflow"` | T2 | AI workflow automation, RCE history |
| `"LangGraph" "studio" port:8123` | T3 | |
| `"Rivet" port:4567` | T3 | Desktop app, server mode uncommon |
