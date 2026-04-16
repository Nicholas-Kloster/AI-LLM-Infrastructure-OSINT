# Shodan Queries — AI/ML Infrastructure

Living catalogue of Shodan dorks for fingerprinting exposed AI/ML control-plane infrastructure.

**Polished PDF reference:** [Shodan_AI_Reference.pdf](Shodan_AI_Reference.pdf)
**Living markdown source:** see [`queries/`](queries/) — these are the files to PR against.

## Index

| # | Category | Examples |
|---|----------|----------|
| 1 | [LLM Orchestration Platforms](queries/01-llm-orchestration.md) | Flowise, Langflow, Dify, Open WebUI, Ollama, n8n |
| 2 | [Vector Databases](queries/02-vector-databases.md) | ChromaDB, Qdrant, Weaviate, Milvus, pgvector |
| 3 | [Model Serving & Inference](queries/03-model-serving.md) | vLLM, Triton, TGI, llama.cpp, TEI |
| 4 | [Training, Fine-Tuning & Experiments](queries/04-training-experiments.md) | MLflow, Kubeflow, Ray, ClearML, Argilla |
| 5 | [AI Gateways, Proxies & Monitoring](queries/05-gateways-monitoring.md) | LiteLLM, Portkey, Langfuse, Helicone |
| 6 | [Agent Frameworks](queries/06-agent-frameworks.md) | SuperAGI, AgentGPT, MetaGPT, OpenDevin |
| 7 | [Credential Leaks & Misconfigurations](queries/07-credential-leaks.md) | API keys, unauth Docker, snapshot exposure |
| 8 | [GPU & Compute Dashboards](queries/08-gpu-compute.md) | NVIDIA DCGM, RunPod, Vast.ai |
| 9 | [Fingerprinting Canaries](queries/09-fingerprinting.md) | Favicon hashes, generic detection |

## Search across all queries

```bash
grep -rn "qdrant"   queries/
grep -rn "port:8000" queries/
grep -rn "API_KEY"  queries/
```

## Adding a new query

1. Find the category file it belongs in (or open an issue to propose a new category).
2. Add a row to the appropriate Markdown table.
3. Include a `Notes` cell when the query reveals something specific — auth state, version, snapshot exposure, default credentials.
4. Open a PR. See [CONTRIBUTING.md](../CONTRIBUTING.md).

## Versioning

The PDF reference is regenerated periodically from the markdown sources. Check the date on the cover page; the markdown is always more current.
