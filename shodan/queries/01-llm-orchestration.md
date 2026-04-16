# 1. LLM Orchestration Platforms

Low-code/no-code builders, agent runtimes, and chain orchestrators. These platforms typically expose a web UI that, when unauthenticated, grants direct access to flow editors, API keys stored in nodes, and execution endpoints.

## Flowise

| Shodan Query | Notes |
|---|---|
| `title:"Flowise" port:443` | |
| `"Flowise" "Express" port:3000` | |
| `http.html:"Low-code LLM apps builder"` | HTML title fingerprint |
| `"Flowise" "chatflows" port:3000 401` | Auth enabled, confirms instance |
| `"X-Powered-By: Express" "/api/v1" "chatflow"` | Flowise fingerprint |

## Other Orchestrators

| Shodan Query | Notes |
|---|---|
| `"Dify" port:80 "LLM"` | |
| `"LangFlow" port:7860` | |
| `"LangChain" port:8000` | |
| `"Haystack" port:8000 "pipeline"` | |
| `"AutoGPT" port:8000` | |
| `"Open WebUI" port:3000` | |
| `"text-generation-webui" port:7860` | |
| `"LocalAI" port:8080` | |
| `"Ollama" port:11434` | |
| `"LiteLLM" port:8000` | |
| `"AnythingLLM" port:3001` | |
| `"Jan" port:1337` | |

## Prompt / Chain Management

| Shodan Query | Notes |
|---|---|
| `"PromptFlow" port:8080 "Microsoft"` | |
| `"Semantic Kernel" port:8080` | |
| `"CrewAI" port:8000` | |
| `"LangGraph" "studio" port:8123` | |
| `"n8n" port:5678 "workflow"` | AI workflow automation |
| `"Rivet" port:4567` | |
| `"PromptLayer" port:8000` | |
