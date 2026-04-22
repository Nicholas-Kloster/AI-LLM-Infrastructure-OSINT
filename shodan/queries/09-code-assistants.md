# 9. AI Code Assistants

_New in v2 · Section verified: April 2026_

Self-hosted code assistants index proprietary source code to produce completions. Exposure of the assistant backend leaks the indexed codebase.

| Shodan Query | Notes |
|---|---|
| `"Tabby" OR "TabbyML" port:8080` | Self-hosted code completion, indexes private repos |
| `http.title:"Tabby"` | |
| `"Cody" "sourcegraph" port:7080` | Self-hosted Cody |
| `"Continue" port:65432` | Continue.dev server mode |
| `"Refact" port:8008` | |
| `"FauxPilot" port:5000` | |
| `"LocalPilot" port:8080` | |
| `"bigcode" "/v2/models" port:8080` | |
