# 15. Fingerprinting Canaries

_Section verified: April 2026_

Generic fingerprints that catch services regardless of branding. Useful when a target operator has stripped HTTP titles or moved services to non-default ports, but the underlying framework still leaks its identity through favicon hashes, headers, or API surface.

## Favicon Hashes

| Shodan Query | Notes |
|---|---|
| `http.favicon.hash:-1294819032` | Gradio |
| `http.favicon.hash:1279780014` | Streamlit |
| `http.favicon.hash:-1848965666` | Jupyter |
| `http.favicon.hash:-1404538293` | LlamaIndex / Create Llama App |
| `http.favicon.hash:348721092` | Clawdbot / OpenClaw agent UI |

Favicon hashes drift with version bumps. Hashes here were valid in April 2026; for long-term use, pair a hash with a text fingerprint to catch the service even when the icon changes.

## Generic AI Service Detection

| Shodan Query | Notes |
|---|---|
| `"Server: uvicorn" "/docs" "FastAPI"` | Any FastAPI ML service |
| `"/v1/chat/completions" port:8000` | OpenAI-compatible endpoint |
| `"/chat/completions"` | Unscoped form — catches OpenAI-compat APIs on non-standard paths/ports |
| `"/v1/embeddings" port:8000` | |
| `"model" "temperature" "max_tokens" port:8000` | OpenAI-style request schema |
| `"LM Studio" OR "lmstudio" port:1234` | LM Studio desktop server exposure |
| `http.html:"api/tags" port:11434` | Ollama model list (no auth) |
| `http.html:"mcp.json" OR "Model Context Protocol"` | MCP servers — heavily targeted in LLMjacking campaigns |
| `"aiohttp" product:"ComfyUI"` | Quick ComfyUI product-level filter |
