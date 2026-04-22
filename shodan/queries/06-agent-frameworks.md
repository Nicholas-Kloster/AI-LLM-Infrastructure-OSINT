# 6. Agent Frameworks

_Section verified: April 22, 2026 15:21_

Autonomous agent runtimes — software that decides what to do, then executes tools, code, and shell commands on behalf of a user. Exposed instances often grant unauthenticated control over an agent that already has filesystem, network, and code-execution capabilities on its host.

## Autonomous Agents

| Shodan Query | Count | Notes |
|---|---|---|
| `"SuperAGI" port:3000` | 0 | |
| `http.title:"SuperAGI"` | 9 | Title fingerprint — catches relocated/proxied deployments |
| `"AgentGPT" port:3000` | 0 | |
| `http.title:"AgentGPT"` | 9 | Title fingerprint |
| `"BabyAGI" port:8080` | 0 | |
| `"MetaGPT" port:8000` | 0 | |
| `"OpenDevin" port:3000` | 0 | Full dev environment — shell/file/git access |
| `http.title:"OpenDevin"` | 4 | Title fingerprint |
| `http.title:"OpenHands"` | 215 | OpenDevin renamed to OpenHands — same shell/browser/file authority |
| `"autogpt-next-web" port:3000` | 0 | AutoGPT-Next-Web UI — exposed backend = agent + key leak |
| `"SWE-agent" port:8000` | 0 | |
| `"Devika" port:1337` | 0 | |
| `"GPT Researcher" port:8000` | 0 | |
| `"Phidata" port:8000` | 0 | |
| `("Clawdbot" OR "OpenClaw") port:18789` | 166 | 2026 mass-exposure agent framework — shell, browser, email, calendar |
| `"PrivateGPT" port:8080` | — | Local RAG agent (often zero auth) — see also §7 |
| `("AutoGen" OR "microsoft autogen") port:8000` | 0 | Microsoft AutoGen multi-agent framework |

**Agent framework exposure is a different class of finding.** A reachable agent is not just data disclosure — it is a delegated-authority system acting on behalf of its operator. OpenClaw, OpenDevin, and similar frameworks have shell, browser, email, and calendar primitives. An unauthenticated hit is the operator's hands on your keyboard.
