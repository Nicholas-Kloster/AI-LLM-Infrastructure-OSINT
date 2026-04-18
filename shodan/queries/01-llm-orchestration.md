# 1. LLM Orchestration Platforms

_Section verified: April 2026_

Low-code/no-code builders, agent runtimes, and chain orchestrators. These platforms typically expose a web UI that, when unauthenticated, grants direct access to flow editors, API keys stored in nodes, and execution endpoints.

> Tier legend: **T1** unauthenticated by default · **T2** auth often misconfigured / known bypasses · **T3** recon / fingerprint only. See [shodan/README.md](../README.md#how-to-read-the-tables).

## Flowise

| Shodan Query | Tier | Notes |
|---|---|---|
| `title:"Flowise" port:443` | T3 | Basic fingerprint, mixed auth status |
| `"Flowise" "Express" port:3000` | T2 | Default port, often deployed without auth |
| `http.html:"Low-code LLM apps builder"` | T3 | HTML title fingerprint |
| `"Flowise" "chatflows" port:3000 401` | T3 | Auth enabled, confirms instance |
| `"X-Powered-By: Express" "/api/v1" "chatflow"` | T2 | Flowise fingerprint — see CVE-2024-36420 |

**CVE note:** CVE-2024-36420 (auth bypass via path traversal) affects Flowise < 1.8.2. Exposed pre-auth instances are RCE candidates, not just info disclosure.

## Other Orchestrators

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Dify" port:80 "LLM"` | T3 | Dify fingerprint |
| `"LangFlow" port:7860` | T2 | Often deployed unauth |
| `"LangChain" port:8000` | T3 | Library fingerprint, app varies |
| `"Haystack" port:8000 "pipeline"` | T3 | |
| `"AutoGPT" port:8000` | T2 | |
| `"Open WebUI" port:3000` | T2 | Auth possible, often disabled |
| `"text-generation-webui" port:7860` | T1 | Rarely deployed with auth |
| `"LocalAI" port:8080` | T1 | No auth by default |
| `"Ollama" port:11434` | T1 | No auth support — exposure = full access |
| `"LiteLLM" port:8000` | T2 | Proxy, master key often leaked in env |
| `"AnythingLLM" port:3001` | T2 | Known auth bypass history |
| `"Jan" port:1337` | T3 | Desktop app, rare server exposure |
| `http.title:"Create Llama App"` | T2 | LlamaIndex default UI (common RAG starter) |
| `http.favicon.hash:-1404538293` | T3 | LlamaIndex favicon fingerprint |
| `"private gpt" port:8080 OR port:8001` | T2 | PrivateGPT local RAG app |
| `http.title:"Clawdbot Control"` | T1 | OpenClaw / Clawdbot agent control UI |
| `port:18789 "openclaw" OR "clawdbot"` | T1 | OpenClaw gateway — shell, browser, email access |
| `"Chainlit" port:8000 OR port:8080` | T2 | Popular conversational UI layer on LangChain |

**OpenClaw / Clawdbot:** This is not a passive reconnaissance target. A publicly reachable OpenClaw gateway is an agent with shell execution, browser automation, email send, and calendar write on whoever deployed it. Treat positive hits as live compromise surface, not data disclosure.

## Prompt / Chain Management

| Shodan Query | Tier | Notes |
|---|---|---|
| `"PromptFlow" port:8080 "Microsoft"` | T3 | |
| `"n8n" port:5678 "workflow"` | T2 | AI workflow automation, RCE history |
| `"LangGraph" "studio" port:8123` | T3 | |
| `"Rivet" port:4567` | T3 | Desktop app, server mode uncommon |
