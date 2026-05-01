# University AI Infrastructure Exposures

_NuClide Research — ongoing · Updated 2026-05-01_

Unauthenticated Ollama and Open WebUI instances discovered on university networks. Organized by country / state.

## Naming Convention

Files: `<CC>-<STATE>-<org-slug>.md`  
Example: `US-CA-ucsb.md` = United States, California, UC Santa Barbara

---

## Confirmed Findings

| File | Institution | Country/State | Severity | Key Finding |
|------|-------------|---------------|----------|-------------|
| [US-NY-columbia.md](US-NY-columbia.md) | Columbia University | US · NY | CRITICAL | Cloud proxy (deepseek-v4-pro) + cred leak (username: seascvn066) |
| [US-CA-ucsb.md](US-CA-ucsb.md) | UC Santa Barbara | US · CA | CRITICAL | Auth **disabled**, open inference, "AI Lab", macOS user `marcos` leaked |
| [US-NY-suny-buffalo.md](US-NY-suny-buffalo.md) | SUNY Buffalo | US · NY | CRITICAL | Cloud proxy **200 OK** confirmed, 26 models, RAG pipeline components |
| [US-NC-duke.md](US-NC-duke.md) | Duke University | US · NC | HIGH | Agent model with file inspection tools, function-calling, injection surface |
| [US-IN-purdue-northwest.md](US-IN-purdue-northwest.md) | Purdue University Northwest | US · IN | CRITICAL | **3 cloud proxies live (200 OK)**: qwen3-coder-next, gemma4:31b, gpt-oss:20b |
| [JP-Keio.md](JP-Keio.md) | Keio University | Japan | HIGH | Dual DeepSeek cloud proxy, qwen3.5:122b (75GB) accessible without auth |
| [CA-ON-western-ontario.md](CA-ON-western-ontario.md) | University of Western Ontario | Canada · ON | HIGH | Cloud proxy (deepseek-v4-pro), 9 models including vision-language |

---

## Discovery Queries (Shodan)

```
# University Ollama instances
http.html:"Ollama is running" org:"university"   → 225 results (2026-05-01)

# University Open WebUI instances  
http.html:"Open WebUI" port:3000 org:"university" → 84 results (2026-05-01)
```

Cross-referencing same-IP hits across both queries identifies confirmed auth-bypass hosts (Open WebUI auth + raw Ollama port on same machine).

---

## Methodology

1. Pull Shodan hits for university-attributed IPs
2. Cross-reference Ollama (11434) and Open WebUI (3000) on same IP
3. Probe `/api/config` for `auth: false`
4. Probe `/api/tags` on port 11434 for model inventory + cloud proxy models
5. Check `/api/show` for system prompts on all models
6. Cloud proxy: attempt inference → 401 response exposes Ollama Connect creds

---

## Scale (sampled 2026-05-01)

| Query | Count |
|-------|-------|
| University Ollama (port 11434) | 225 |
| University Open WebUI (port 3000) | 84 |
| Auth disabled (Open WebUI) | ~5–10% of Open WebUI set |
| Raw Ollama open (no Open WebUI auth) | ~30–40% of co-deployed |
| Cloud proxy models in university set | ~10–15% of open Ollama |
