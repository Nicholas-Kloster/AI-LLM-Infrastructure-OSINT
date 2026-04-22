# 6. Agent Frameworks

_Section verified: April 2026_

Autonomous agent runtimes — software that decides what to do, then executes tools, code, and shell commands on behalf of a user. Exposed instances often grant unauthenticated control over an agent that already has filesystem, network, and code-execution capabilities on its host.

> Tier legend: 🟥 **T1** unauthenticated by default · 🟧 **T2** auth often misconfigured / known bypasses · 🟨 **T3** recon / fingerprint only.

## Autonomous Agents

| Shodan Query | Tier | Notes |
|---|---|---|
| `"SuperAGI" port:3000` | 🟧 T2 | |
| `http.title:"SuperAGI"` | 🟧 T2 | Title fingerprint — catches relocated/proxied deployments |
| `"AgentGPT" port:3000` | 🟧 T2 | |
| `http.title:"AgentGPT"` | 🟧 T2 | Title fingerprint |
| `"BabyAGI" port:8080` | 🟧 T2 | |
| `"MetaGPT" port:8000` | 🟧 T2 | |
| `"OpenDevin" port:3000` | 🟧 T2 | Full dev environment — shell/file/git access |
| `http.title:"OpenDevin"` | 🟧 T2 | Title fingerprint |
| `http.title:"OpenHands"` | 🟧 T2 | OpenDevin renamed to OpenHands — same shell/browser/file authority |
| `"autogpt-next-web" port:3000` | 🟧 T2 | AutoGPT-Next-Web UI — exposed backend = agent + key leak |
| `"SWE-agent" port:8000` | 🟧 T2 | |
| `"Devika" port:1337` | 🟧 T2 | |
| `"GPT Researcher" port:8000` | 🟨 T3 | |
| `"Phidata" port:8000` | 🟨 T3 | |
| `"Clawdbot" OR "OpenClaw" port:18789` | 🟥 T1 | 2026 mass-exposure agent framework — shell, browser, email, calendar |
| `"PrivateGPT" port:8080` | 🟧 T2 | Local RAG agent (often zero auth) — see also §7 |
| `"AutoGen" OR "microsoft autogen" port:8000` | 🟧 T2 | Microsoft AutoGen multi-agent framework |

**Agent framework exposure is a different class of finding.** A reachable agent is not just data disclosure — it is a delegated-authority system acting on behalf of its operator. OpenClaw, OpenDevin, and similar frameworks have shell, browser, email, and calendar primitives. An unauthenticated hit is the operator's hands on your keyboard.
