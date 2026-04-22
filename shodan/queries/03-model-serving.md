# 3. Model Serving & Inference

_Section verified: April 2026_

The runtime layer that exposes models over HTTP. Most modern serving stacks emulate the OpenAI API surface (`/v1/chat/completions`, `/v1/models`, `/v1/embeddings`), making them trivially fingerprintable — and often trivially abusable as free compute (LLMjacking).

> Tier legend: 🟥 **T1** unauthenticated by default · 🟧 **T2** auth often misconfigured / known bypasses · 🟨 **T3** recon / fingerprint only.

## Inference Servers

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Triton Inference Server" port:8000` | 🟧 | |
| `"/v1/models" "openai" port:8000` | 🟥 | OpenAI-compatible API, no auth by default |
| `"vLLM" port:8000` | 🟥 | No native auth |
| `"TGI" "text-generation-inference" port:8080` | 🟥 | |
| `"llama.cpp" port:8080` | 🟥 | |
| `"koboldcpp" port:5001` | 🟥 | |
| `"/v1/chat/completions" port:8000` | 🟥 | Any OpenAI-compat endpoint |
| `"/v1/embeddings" port:8000` | 🟥 | |
| `port:1234 "/v1/models"` | 🟥 | LM Studio (very common local inference) |
| `port:8000 "openai" "model"` | 🟥 | vLLM / any OpenAI-compat with model list |
| `port:8080 "llama" "completion"` | 🟥 | llama.cpp server (enhanced fingerprint) |
| `port:4891 "GPT4All"` | 🟥 | GPT4All desktop LLM server |
| `"NVIDIA NIM" port:8000` | 🟧 | NVIDIA NIM inference microservices (2026 deployments) |
| `"sglang" port:30000` | 🟥 | SGLang structured-generation server, OpenAI-compat |
| `"lmdeploy" port:23333` | 🟥 | InternLM LMDeploy inference engine |
| `"aphrodite" "engine" port:2242` | 🟥 | Aphrodite Engine (vLLM fork, common in hobby/SillyTavern stacks) |
| `"koboldcpp" OR "koboldai"` | 🟥 | Complements port:5001 form — catches relocated/AI Horde workers |
| `"Seldon" "deployment" port:8000` | 🟧 | Seldon Core model deployment on k8s |

```
GET /v1/models HTTP/1.1  {"object":"list","data":[{"id":"meta-llama/Llama-3.1-70B-Instruct",
  "object":"model","created":1713300000,"owned_by":"vllm"}]}
```
Typical vLLM response on an exposed port. Model name reveals what's hosted; `/v1/chat/completions` on the same port is reachable with no auth.

## Embedding / Reranking

| Shodan Query | Tier | Notes |
|---|---|---|
| `"TEI" "text-embeddings-inference" port:8080` | 🟥 | |
| `"infinity" "embedding" port:7997` | 🟥 | |
| `"Jina" "embeddings" port:8080` | 🟧 | |
| `"Cohere" "rerank" port:8000` | 🟨 | |
| `"sentence-transformers" port:8080` | 🟥 | |

## Audio / Speech / Vision Inference

| Shodan Query | Tier | Notes |
|---|---|---|
| `"whisper.cpp" "/inference"` | 🟥 | whisper.cpp HTTP server — free ASR compute |
| `"faster-whisper" port:8000` | 🟥 | Same inference surface, CTranslate2 backend |
| `"coqui" "tts" port:5002` | 🟥 | Coqui TTS server |
| `"piper" "tts"` | 🟥 | Piper neural TTS (common on RPi/edge) |
| `http.title:"Bark"` | 🟧 | Suno Bark text-to-audio WebUI |
| `"vocode" "voice"` | 🟧 | Vocode voice-agent framework — often phone/webhook integrations |
| `"paddleocr" port:8080` | 🟧 | PaddleOCR service — free OCR compute + uploaded doc disclosure |
