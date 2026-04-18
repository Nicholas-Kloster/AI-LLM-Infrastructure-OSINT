# 8. Image Generation & Diffusion

_New in v2 · Section verified: April 2026_

Image generation stacks are deployed at massive scale on consumer GPU hardware, frequently with `--listen 0.0.0.0` set as the default by unofficial install scripts. Custom-trained LoRAs and checkpoint files on these hosts often include private subject data.

> Tier legend: **T1** unauthenticated by default · **T2** auth often misconfigured / known bypasses · **T3** recon / fingerprint only.

| Shodan Query | Tier | Notes |
|---|---|---|
| `"ComfyUI" port:8188` | T1 | Default install listens on all interfaces, no auth |
| `http.title:"ComfyUI" OR product:"ComfyUI"` | T1 | Title/product fingerprint |
| `"comfyui" port:8188 OR port:5984` | T1 | aiohttp backend, both common ports |
| `"Stable Diffusion" port:7860` | T1 | AUTOMATIC1111 WebUI |
| `"AUTOMATIC1111" port:7860` | T1 | |
| `http.title:"Stable Diffusion"` | T1 | |
| `"InvokeAI" port:9090` | T2 | |
| `"Fooocus" port:7865` | T1 | |
| `"SwarmUI" port:7801` | T1 | |
| `"SD.Next" port:7860 "vlad"` | T1 | |
| `"Forge" "Stable Diffusion" port:7860` | T1 | |

**ComfyUI custom nodes** are an arbitrary Python execution surface. An exposed ComfyUI with the ComfyUI-Manager node (extremely common) allows remote installation of additional custom nodes, which is RCE by design.
