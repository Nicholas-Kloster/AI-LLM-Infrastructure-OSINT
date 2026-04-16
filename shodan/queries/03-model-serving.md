# 3. Model Serving & Inference

The runtime layer that exposes models over HTTP. Most modern serving stacks emulate the OpenAI API surface (`/v1/chat/completions`, `/v1/models`, `/v1/embeddings`), making them trivially fingerprintable — and often trivially abusable as free compute.

## Inference Servers

| Shodan Query | Notes |
|---|---|
| `"Triton Inference Server" port:8000` | |
| `"/v1/models" "openai" port:8000` | OpenAI-compatible API |
| `"vLLM" port:8000` | |
| `"TGI" "text-generation-inference" port:8080` | |
| `"llama.cpp" port:8080` | |
| `"koboldcpp" port:5001` | |
| `"/v1/chat/completions" port:8000` | Any OpenAI-compat endpoint |
| `"/v1/embeddings" port:8000` | |

## Embedding / Reranking

| Shodan Query | Notes |
|---|---|
| `"TEI" "text-embeddings-inference" port:8080` | |
| `"infinity" "embedding" port:7997` | |
| `"Jina" "embeddings" port:8080` | |
| `"Cohere" "rerank" port:8000` | |
| `"sentence-transformers" port:8080` | |
