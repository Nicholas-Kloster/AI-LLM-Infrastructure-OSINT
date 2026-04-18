# 9. AI Code Assistants

_New in v2 · Section verified: April 2026_

Self-hosted code assistants index proprietary source code to produce completions. Exposure of the assistant backend leaks the indexed codebase.

> Tier legend: **T1** unauthenticated by default · **T2** auth often misconfigured / known bypasses · **T3** recon / fingerprint only.

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Tabby" OR "TabbyML" port:8080` | T2 | Self-hosted code completion, indexes private repos |
| `http.title:"Tabby"` | T3 | |
| `"Cody" "sourcegraph" port:7080` | T2 | Self-hosted Cody |
| `"Continue" port:65432` | T3 | Continue.dev server mode |
| `"Refact" port:8008` | T2 | |
| `"FauxPilot" port:5000` | T2 | |
| `"LocalPilot" port:8080` | T2 | |
| `"bigcode" "/v2/models" port:8080` | T3 | |
