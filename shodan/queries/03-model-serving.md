# 3. Model Serving & Inference

_Section verified: April 2026_

The runtime layer that exposes models over HTTP. Most modern serving stacks emulate the OpenAI API surface (`/v1/chat/completions`, `/v1/models`, `/v1/embeddings`), making them trivially fingerprintable — and often trivially abusable as free compute (LLMjacking).

> Tier legend: 🟥 **T1** unauthenticated by default · 🟧 **T2** auth often misconfigured / known bypasses · 🟨 **T3** recon / fingerprint only.

## Inference Servers

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Triton Inference Server" port:8000` | 🟧 T2 | |
| `"/v1/models" "openai" port:8000` | 🟥 T1 | OpenAI-compatible API, no auth by default |
| `"vLLM" port:8000` | 🟥 T1 | No native auth |
| `"TGI" "text-generation-inference" port:8080` | 🟥 T1 | |
| `"llama.cpp" port:8080` | 🟥 T1 | |
| `"koboldcpp" port:5001` | 🟥 T1 | |
| `"/v1/chat/completions" port:8000` | 🟥 T1 | Any OpenAI-compat endpoint |
| `"/v1/embeddings" port:8000` | 🟥 T1 | |
| `port:1234 "/v1/models"` | 🟥 T1 | LM Studio (very common local inference) |
| `port:8000 "openai" "model"` | 🟥 T1 | vLLM / any OpenAI-compat with model list |
| `port:8080 "llama" "completion"` | 🟥 T1 | llama.cpp server (enhanced fingerprint) |
| `port:4891 "GPT4All"` | 🟥 T1 | GPT4All desktop LLM server |
| `"NVIDIA NIM" port:8000` | 🟧 T2 | NVIDIA NIM inference microservices (2026 deployments) |
| `"sglang" port:30000` | 🟥 T1 | SGLang structured-generation server, OpenAI-compat |
| `"lmdeploy" port:23333` | 🟥 T1 | InternLM LMDeploy inference engine |
| `"aphrodite" "engine" port:2242` | 🟥 T1 | Aphrodite Engine (vLLM fork, common in hobby/SillyTavern stacks) |
| `"koboldcpp" OR "koboldai"` | 🟥 T1 | Complements port:5001 form — catches relocated/AI Horde workers |
| `"Seldon" "deployment" port:8000` | 🟧 T2 | Seldon Core model deployment on k8s |

```
GET /v1/models HTTP/1.1  {"object":"list","data":[{"id":"meta-llama/Llama-3.1-70B-Instruct",
  "object":"model","created":1713300000,"owned_by":"vllm"}]}
```
Typical vLLM response on an exposed port. Model name reveals what's hosted; `/v1/chat/completions` on the same port is reachable with no auth.

## Embedding / Reranking

| Shodan Query | Tier | Notes |
|---|---|---|
| `"TEI" "text-embeddings-inference" port:8080` | 🟥 T1 | |
| `"infinity" "embedding" port:7997` | 🟥 T1 | |
| `"Jina" "embeddings" port:8080` | 🟧 T2 | |
| `"Cohere" "rerank" port:8000` | 🟨 T3 | |
| `"sentence-transformers" port:8080` | 🟥 T1 | |

## Audio / Speech / Vision Inference

| Shodan Query | Tier | Notes |
|---|---|---|
| `"whisper.cpp" "/inference"` | 🟥 T1 | whisper.cpp HTTP server — free ASR compute |
| `"faster-whisper" port:8000` | 🟥 T1 | Same inference surface, CTranslate2 backend |
| `"coqui" "tts" port:5002` | 🟥 T1 | Coqui TTS server |
| `"piper" "tts"` | 🟥 T1 | Piper neural TTS (common on RPi/edge) |
| `http.title:"Bark"` | 🟧 T2 | Suno Bark text-to-audio WebUI |
| `"vocode" "voice"` | 🟧 T2 | Vocode voice-agent framework — often phone/webhook integrations |
| `"paddleocr" port:8080` | 🟧 T2 | PaddleOCR service — free OCR compute + uploaded doc disclosure |
