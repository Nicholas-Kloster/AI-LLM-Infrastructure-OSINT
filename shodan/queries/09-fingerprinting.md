# 9. Fingerprinting Canaries

Generic fingerprints that catch services regardless of branding. Useful when a target operator has stripped HTTP titles or moved services to non-default ports, but the underlying framework still leaks its identity through favicon hashes, headers, or API surface.

## Favicon Hashes

| Shodan Query | Notes |
|---|---|
| `http.favicon.hash:-1294819032` | Gradio |
| `http.favicon.hash:1279780014` | Streamlit |
| `http.favicon.hash:-1848965666` | Jupyter |

## Generic AI Service Detection

| Shodan Query | Notes |
|---|---|
| `"Server: uvicorn" "/docs" "FastAPI"` | Any FastAPI ML service |
| `"/v1/chat/completions" port:8000` | OpenAI-compatible endpoint |
| `"/v1/embeddings" port:8000` | |
| `"model" "temperature" "max_tokens" port:8000` | |
