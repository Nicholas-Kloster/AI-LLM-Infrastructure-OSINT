# 11. Credential Leaks & Misconfigs

_Section verified: April 2026_

The high-impact category. Exposed environment files and provider keys collapse the attack chain from "exposed service" to "full provider account takeover" in a single step.

> Tier legend: 🟥 **T1** unauthenticated by default · 🟧 **T2** auth often misconfigured / known bypasses · 🟨 **T3** recon / fingerprint only.

## Exposed API Keys

| Shodan Query | Tier | Notes |
|---|---|---|
| `"OPENAI_API_KEY" port:8000` | 🟥 T1 | Env var leaked in banner/error page |
| `"sk-" "openai" port:3000` | 🟧 T2 | OpenAI classic key format |
| `"sk-proj" port:3000 OR port:8000` | 🟥 T1 | OpenAI project keys (newer format) |
| `"ANTHROPIC_API_KEY"` | 🟥 T1 | Env-var name form |
| `"sk-ant-" -site:anthropic.com` | 🟥 T1 | Raw Anthropic key prefix — catches leaks where the env var name was stripped |
| `"sk-ant-api03-"` | 🟥 T1 | Anthropic v3 key format, narrower regex |
| `http.html:"MISTRAL_API_KEY"` | 🟥 T1 | Mistral env var leaked in page body |
| `http.html:"DEEPSEEK_API_KEY"` | 🟥 T1 | DeepSeek env var — frequent in 2026 self-hosted stacks |
| `"gsk_" OR "GROQ_API_KEY" port:8000` | 🟥 T1 | Groq API keys (fast-growing in 2026) |
| `"AIzaSy" OR "GOOGLE_API_KEY" "generativelanguage"` | 🟥 T1 | Gemini / Google AI keys |
| `"hf_" OR "HF_TOKEN" port:8080` | 🟥 T1 | Hugging Face token |
| `".env" "API_KEY" port:8000` | 🟥 T1 | Exposed .env file |
| `http.html:".env" ("OPENAI" OR "ANTHROPIC" OR "GROQ")` | 🟥 T1 | .env files leaking multiple providers |
| `".claude/settings.json"` | 🟥 T1 | Claude Code project config — reveals enabled MCP servers, hooks, allowed tools; pivots to §10 |

**Reporting note:** Keys surfaced via Shodan are already exposed to the index and to anyone who paid for a Shodan subscription. Rotation is the fix; reporting to the key owner is the disclosure. Do not test the key against the provider API — that crosses the line from passive recon into credentialed access.
