# 6. Agent Frameworks

_Section verified: April 22, 2026 15:21_

Autonomous agent runtimes — software that decides what to do, then executes tools, code, and shell commands on behalf of a user. Exposed instances often grant unauthenticated control over an agent that already has filesystem, network, and code-execution capabilities on its host.

## OpenHands / OpenDevin

OpenDevin was renamed to OpenHands in late 2024. Both names are in active use on the internet — deployments predate the rename, and many tutorials still reference the old name. The framework provides a full software engineering environment: shell execution, file I/O, browser automation, and git. An unauthenticated hit is not information disclosure — it is a dev environment running as the deployer.

| Shodan Query | Notes |
|---|---|
| `http.html:"openhands"` | 237 hits — canonical HTML fingerprint, catches proxied/renamed deployments |
| `http.title:"OpenHands" http.status:200` | 215 hits — all title-match hits return 200; zero 401s, confirming the entire OpenHands population is openly accessible with no auth-gating detected |
| `http.title:"OpenHands"` | 215 hits — title-level match, high confidence |
| `http.html:"openhands" port:443` | 21 hits — TLS-proxied subset |
| `"OpenHands"` | 21 hits — banner-level match |
| `http.title:"OpenDevin"` | 4 hits — legacy name, pre-rename deployments still live |
| `http.html:"opendevin"` | 6 hits — legacy HTML fingerprint |
| `"OpenDevin"` | 0 hits — legacy banner match, extinct |
| `product:"OpenHands"` | 0 hits — no Shodan product facet registered |
| `product:"OpenDevin"` | 0 hits — no Shodan product facet registered |
| `"OpenDevin" port:3000` | 0 hits — original default port, dead on the public internet |
| `"OpenHands" port:3000` | 0 hits — same deployment pattern shift |

**Fingerprint note:** The legacy `"OpenDevin"` bare banner query returns 0 while `http.html:"opendevin"` returns 6 — confirming the same banner-vs-HTML gap documented in §2 for Qdrant. Banner grabs miss the term when Shodan's initial crawl doesn't reflect it in headers or root response. Use `http.html:` as primary fingerprint.

## OpenClaw / Clawdbot

Primary coverage is in §1 (LLM Orchestration Platforms), which documents the OpenClaw gateway fingerprint. This subsection addresses the agent-framework lens: Clawdbot is the agent UI layer that connects to an OpenClaw gateway. `http.title:"Clawdbot"` matches the agent control panel, which is the target — not the gateway alone.

**Pollution warning on bare terms:** `"openclaw"` = 157,812 hits and `http.html:"openclaw"` = 83,925 — massively polluted by unrelated projects sharing the string "openclaw" (gaming, network tooling). `"clawdbot"` bare = 2,424, still significantly polluted. The port-restricted and title-specific variants are the only clean fingerprints.

| Shodan Query | Notes |
|---|---|
| `http.html:"clawdbot"` | 1,933 hits — ⚠️ some pollution; cross-ref §1 |
| `http.title:"Clawdbot Control"` | 1,770 hits — canonical agent control panel fingerprint; matches §1 data |
| `http.title:"Clawdbot Control" http.status:200` | 1,770 hits — entire control panel population returns 200; zero 401s, confirming no auth-gating at the HTTP layer across all 1,770 deployments |
| `http.title:"Clawdbot"` | 1,818 hits — slightly broader than Control subset |
| `("Clawdbot" OR "OpenClaw") port:18789` | 166 hits — gateway default port; grouped OR required (see §1) |
| `"clawdbot" port:18789` | 29 hits — Clawdbot on gateway port |
| `"openclaw" "agent"` | 22,156 hits — ⚠️ narrowing attempt still polluted; "agent" is too generic |

**Cross-reference §1** for the `port:18789 ("openclaw" OR "clawdbot")` gateway fingerprint and the full deployment note. The Clawdbot control panel (1,818–1,933 title/html hits) is significantly larger than what the port-restricted gateway query captures (166), suggesting most deployments proxy Clawdbot on 80/443 behind the gateway.

## AutoGPT / AutoGPT-Next-Web

AutoGPT is largely moribund since mid-2025; development stalled and the project fragmented. The `http.html:"AutoGPT"` fingerprint at 32 hits is already documented in §1 — cross-reference there. AutoGPT-Next-Web (the Next.js UI wrapper) never achieved significant production deployment. Counts here are low and declining.

| Shodan Query | Notes |
|---|---|
| `http.html:"autogpt"` | 32 hits — canonical HTML fingerprint; already documented in §1 |
| `http.title:"AutoGPT"` | 16 hits — title match |
| `"AutoGPT"` | 8 hits — banner match |
| `"AutoGPT" port:443` | 4 hits — TLS-proxied subset |
| `"autogpt-next-web"` | 1 hit — Next-Web UI, near-extinct |
| `"AutoGPT" "next-web"` | 1 hit — combined marker |
| `product:"AutoGPT"` | 0 hits — no Shodan product facet |
| `"AutoGPT" port:3000` | 0 hits — original default port, dead |

**Cross-reference §1** for the `http.html:"AutoGPT"` 32-hit count, originally catalogued under Other Orchestrators.

## SuperAGI

SuperAGI is a controllable autonomous agent framework with a web UI, tool integrations (code execution, browser, shell), and a marketplace of "superhuman agents." The `http.html:"superagi"` query at 10 hits is the strongest fingerprint found; the bare banner is noisier.

| Shodan Query | Notes |
|---|---|
| `http.html:"superagi"` | 10 hits — canonical HTML fingerprint (case-insensitive match) |
| `http.html:"superagi" http.status:200` | 6 hits — open-access subset; 4 of 10 html hits appear auth-gated or non-200 |
| `http.title:"SuperAGI"` | 9 hits — title match, high confidence |
| `"SuperAGI"` | 5 hits — banner-level match |
| `"SuperAGI" port:443` | 5 hits — TLS-proxied subset (all banner hits appear proxied) |
| `product:"SuperAGI"` | 0 hits — no Shodan product facet |
| `"SuperAGI" port:3000` | 0 hits — original default port, dead |
| `"SuperAGI" port:8080` | 0 hits — alternate port tested, dead |

**Fingerprint note:** The full 10-hit `http.html:"superagi"` population vs 9-hit `http.title:"SuperAGI"` suggests a near-total overlap — SuperAGI's HTML template includes both the title and body string. Use `http.title:"SuperAGI"` as the cleaner query; avoids case collision with unrelated strings containing "superagi".

## AgentGPT

AgentGPT (Reworkd) is a browser-based autonomous agent builder. The `http.html:"reworkd"` query (the maker's name) at 4 hits is the tightest fingerprint — less collision-prone than the product name alone.

| Shodan Query | Notes |
|---|---|
| `http.html:"agentgpt"` | 12 hits — canonical HTML fingerprint |
| `http.html:"agentgpt" http.status:200` | 12 hits — full population is open-access; 0 auth-gated instances; all AgentGPT deployments publicly accessible |
| `http.title:"AgentGPT"` | 9 hits — title match |
| `http.html:"reworkd"` | 4 hits — maker name in HTML; highest-confidence narrowing |
| `"AgentGPT"` | 0 hits — no banner presence |
| `product:"AgentGPT"` | 0 hits — no Shodan product facet |
| `"AgentGPT" port:3000` | 0 hits — original default port, dead |

## GPT Researcher

GPT Researcher (GPTR) is an autonomous research agent that queries the web, synthesizes reports, and outputs structured documents. The `http.html:"gpt_researcher"` variant (underscore) at 64 hits outperforms the hyphenated variant — the project's internal Python module name uses underscores, which surfaces in embedded JavaScript references and build artifacts.

| Shodan Query | Notes |
|---|---|
| `http.html:"gpt_researcher"` | 64 hits — canonical fingerprint; underscore = Python module name in bundled JS |
| `http.html:"gpt-researcher"` | 54 hits — hyphen variant, URL/config form |
| `http.html:"gpt-researcher" port:8000` | 27 hits — default port subset, direct-exposure candidates |
| `http.html:"gpt-researcher" port:8000 http.status:200` | 27 hits — all 27 default-port hits return 200; entire direct-deployment population is openly accessible |
| `http.html:"gpt-researcher" port:443` | 6 hits — TLS-proxied subset |
| `http.title:"GPT Researcher"` | 2 hits — title match, sparse |
| `"GPT Researcher"` | 2 hits — banner match, sparse |
| `"GPT Researcher" port:443` | 1 hit — TLS banner match |
| `product:"GPT Researcher"` | 0 hits — no Shodan product facet |
| `"GPT Researcher" port:8000` | 0 hits — bare banner on default port, dead |

**Fingerprint lesson:** The underscore vs hyphen split (64 vs 54 hits) reflects a general pattern — Python projects embed their module name (underscores) in frontend build artifacts differently from their URL slug (hyphens). When scanning Python-based web apps, always test both forms.

## MetaGPT

MetaGPT is a multi-agent framework that simulates software engineering roles (PM, architect, engineer, QA). Deployment surface is minimal — it is primarily used as a library/CLI tool, not a persistent web service. All four major Shodan fingerprinting methods return near-zero or zero.

| Shodan Query | Notes |
|---|---|
| `http.html:"metagpt"` | 3 hits — canonical HTML fingerprint |
| `"metagpt" http.status:200` | 1 hit — status-filtered banner match; same population as bare banner |
| `"MetaGPT"` | 1 hit — banner match |
| `http.title:"MetaGPT"` | 1 hit — title match |
| `product:"MetaGPT"` | 0 hits — no Shodan product facet |
| `"MetaGPT" port:8000` | 0 hits — default port tested, dead |
| `http.component:"Streamlit" "metagpt"` | 0 hits — wrapper search, no Streamlit deployments found |
| `http.component:"Gradio" "metagpt"` | 0 hits — wrapper search, no Gradio deployments found |
| `http.component:"FastAPI" "metagpt"` | 0 hits — wrapper search, no FastAPI deployments found |
| `"metagpt" "role"` | 0 hits — paired-token pivot, dead |
| `"metagpt" "architect"` | 0 hits — paired-token pivot, dead |
| `ssl:"metagpt"` | 0 hits — no TLS cert references found |

**Not currently visible on Shodan (effectively).** MetaGPT exposes no persistent HTTP service in standard deployments — it runs as a CLI pipeline and writes output to disk. The 1–3 hits across banner/html/title are noise or one-off demo environments. Wrapper search pivots (Streamlit/Gradio/FastAPI + "metagpt") all return zero, confirming the framework is not being wrapped into publicly-accessible UIs at any detectable scale. No meaningful internet-facing exposure surface to enumerate.

## BabyAGI

BabyAGI is one of the earliest task-management agent loops. The project has seen minimal active development since 2024 and its web deployment surface was always thin — it was designed as a terminal script, not a web service.

**Not currently visible on Shodan.** Exhaustive pivot coverage across six baseline variants and eight wrapper/alternate strategies all return zero or near-zero:

- `"BabyAGI"` — 0
- `http.html:"babyagi"` — 0
- `http.title:"BabyAGI"` — 0
- `product:"BabyAGI"` — 0
- `"BabyAGI" port:8080` — 0
- `"BabyAGI" port:3000` — 0
- `http.component:"Streamlit" "babyagi"` — 0
- `http.component:"Gradio" "babyagi"` — 0
- `http.component:"FastAPI" "babyagi"` — 0
- `"babyagi" "task_list"` — 0
- `"babyagi" "objective"` — 0
- `http.html:"babyagi" http.status:200` — 0
- `"BabyAGI" http.component:"Jupyter"` — 0
- `ssl:"babyagi"` — 1 (single anomalous cert mention; not a deployment signal)

**Blind spot:** BabyAGI's absence is not a fingerprinting failure — the framework was never packaged as a web service. Wrapper pivots (Streamlit/Gradio/FastAPI/Jupyter + "babyagi") all returned zero, confirming no wrapper deployment surface exists either. The single `ssl:"babyagi"` hit is a cert text coincidence. No Shodan query surface exists to enumerate this class of deployment.

## SWE-agent

SWE-agent (Princeton NLP) is a coding agent that uses a custom agent-computer interface (ACI) to write and run code against GitHub issues. It is primarily a research pipeline with no standard persistent web UI. The `http.html:"swe-agent"` query at 6 hits captures the only live fingerprint.

| Shodan Query | Notes |
|---|---|
| `http.html:"swe-bench"` | 25 hits — proxy indicator; SWE-bench benchmark pages surface research compute environments running SWE-agent workflows |
| `http.html:"swe-agent"` | 6 hits — only viable direct fingerprint found |
| `http.html:"swe-agent" -http.title:"GitHub"` | 6 hits — GitHub mirror exclusion has no effect; all 6 hits already non-GitHub |
| `"SWE-agent"` | 0 hits — no banner presence |
| `http.title:"SWE-agent"` | 0 hits — no title-level deployments found |
| `http.html:"sweagent"` | 0 hits — concatenated variant, dead |
| `product:"SWE-agent"` | 0 hits — no Shodan product facet |
| `"swe-agent" port:8000` | 0 hits — default port tested, dead |
| `http.component:"Streamlit" "swe-agent"` | 0 hits — wrapper search, no Streamlit deployments |
| `http.component:"Jupyter" "swe-agent"` | 0 hits — wrapper search, no Jupyter deployments |
| `"swe-agent" http.status:200` | 0 hits — status filter adds nothing |
| `ssl:"swe-agent"` | 0 hits — no TLS cert references |

**Blind spot:** SWE-agent ships as a CLI tool and Docker image; it does not expose a web UI by default. The 6 `http.html:"swe-agent"` hits are documentation pages, GitHub mirror indexes, or project portals — not live agent endpoints. The `http.html:"swe-bench"` proxy query at 25 hits surfaces research infrastructure (AI services, code evaluation platforms) that are adjacent to SWE-agent but not confirmable as agent deployments. No meaningful autonomous-agent exposure surface is enumerable via Shodan. Wrapper search pivots (Streamlit/Jupyter) all returned zero.

## Devika

Devika is an open-source agentic AI software engineer with a web UI. The bare `"Devika"` banner query is a severe name collision — "Devika" is a common South Asian personal name and appears in unrelated software projects.

**Pollution test results:** `"Devika"` bare = 3 hits (low count but unverified origin); `http.html:"devika"` = 19 hits — the word is common enough in other contexts that confidence is low without narrowing. `"Devika" "agent"` = 0 — attempting to narrow with "agent" returns nothing, confirming the framework's near-zero deployment surface, not a fingerprint win.

| Shodan Query | Notes |
|---|---|
| `http.html:"devika"` | 19 hits — ⚠️ pollution likely; "devika" appears in unrelated South Asian web content |
| `http.html:"devika" http.status:200` | 19 hits — status:200 filter has no narrowing effect; pollution is 200-OK content, not auth-gated noise |
| `http.html:"devika" -"profile" -"wedding" -"name"` | 19 hits — three-token pollution strip has zero effect; personal-name context uses different co-occurring terms than guessed |
| `http.title:"Devika"` | 4 hits — title match; lower noise than HTML body, most trustworthy signal |
| `"Devika"` | 3 hits — ⚠️ banner match, origin uncertain |
| `"Devika" -http.title:"Login" -http.title:"Sign In"` | 3 hits — login-page exclusion matches the same 3 bare banner hits; negation has no effect |
| `"Devika" "agent"` | 0 hits — narrowing attempt returns zero; no confirmed agent deployments |
| `http.html:"devikaai"` | 0 hits — concatenated brand form, dead |
| `product:"Devika"` | 0 hits — no Shodan product facet |
| `"Devika" port:1337` | 0 hits — original default port, dead |
| `http.html:"stitionai"` | 0 hits — GitHub org-name fingerprint; org references do not surface in indexed HTTP content |
| `http.html:"devika" http.component:"React"` | 0 hits — React component co-occurrence returns zero despite framework using React |

**Fingerprint note:** Until `http.title:"Devika"` is confirmed against live samples, treat all Devika counts as upper bounds. The 4-hit title match is the most trustworthy of the available signals. The `http.html:"stitionai"` GitHub-org pivot returned zero — the stitionai org name does not appear in any Shodan-indexed HTTP responses, confirming the org-name approach fails for this project. Personal-name pollution in the html:"devika" population is resistant to standard negation chains (profile/wedding/name tokens don't appear in the polluting content).

## Phidata / Agno

Phidata was renamed to **agno** in early 2025. Both names appear in the wild — Phidata persists in older deployments and documentation; agno is the current brand. The rename created a split fingerprint surface.

**Pollution warning on "agno":** `"agno"` bare = 262 hits — massively polluted. "Agno" is a Philippine municipality, a common abbreviation ("agnostic"), and appears in unrelated software. `http.html:"agno"` = 70 hits — still noisy. Narrowing with paired tokens `"agno" "phi"` or `"agno" "llm"` returns 0 and 1 respectively — the framework's HTML does not consistently co-locate recognizable tokens. Use `http.title:"Agno"` (11 hits) as the cleanest available fingerprint, with sampling recommended before treating as confirmed.

| Shodan Query | Notes |
|---|---|
| `"agno"` | 262 hits — ⚠️ severely polluted; Philippine geography + generic abbreviation collisions |
| `http.html:"agno"` | 70 hits — ⚠️ still polluted; sample before trusting |
| `http.html:"agno-agents"` | 21 hits — **canonical fingerprint**; package/import name `agno-agents` embeds cleanly in frontend build artifacts; multiple hits show `http.title:"Agent UI"` (Agno's default UI title) returning HTTP 200 |
| `http.title:"Agno"` | 11 hits — title-level fingerprint; secondary since package-name query wins |
| `ssl.cert.subject.cn:"*.agno.com"` | 6 hits — wildcard cert for agno.com; identifies vendor infrastructure (AWS 3.x/54.x/34.x), not third-party deployments |
| `ssl:"agno.com"` | 6 hits — identical population to cert subject query; same vendor infrastructure |
| `"Phidata"` | 2 hits — old brand, declining |
| `"agno" "llm"` | 1 hit — narrowed with LLM term; near-zero confirms thin deployment surface |
| `"agno" "phi"` | 0 hits — original brand paired with new, dead |
| `http.html:"phidata"` | 0 hits — HTML fingerprint gone post-rename |
| `http.html:"phidata" http.status:200` | 0 hits — old brand gone even with status filter; complete post-rename disappearance confirmed |
| `http.title:"Phidata"` | 0 hits — title fingerprint gone post-rename |
| `product:"Phidata"` | 0 hits — no Shodan product facet |
| `http.securitytxt:"phidata"` | 0 hits — no security.txt references to old brand |
| `http.securitytxt:"agno"` | 0 hits — no security.txt references to new brand |
| `http.securitytxt:"agno.com"` | 0 hits — no security.txt policy references |
| `http.html:"agno" http.component:"FastAPI"` | 0 hits — FastAPI co-occurrence returns zero |

**Rename blind spot:** The transition from Phidata → agno split the fingerprint surface. `http.html:"phidata"` went to zero while `"agno"` is polluted and no clean paired-token narrowing works. This is a general pattern when AI projects rebrand to short, dictionary-word names — the new name inherits massive unrelated index content. **Rescue query: `http.html:"agno-agents"`** (21 hits) — the Python package name `agno-agents` embeds cleanly in frontend build artifacts and is distinctive enough to be low-noise. Multiple confirmed `http.title:"Agent UI"` hits returning HTTP 200. This is now the canonical Agno fingerprint. The `ssl:"agno.com"` / `ssl.cert.subject.cn:"*.agno.com"` queries at 6 hits map vendor infrastructure, not third-party deployments.

## AutoGen

Microsoft AutoGen (now AutoGen 0.4+, also called "AG2" in some forks) is a multi-agent conversation framework. The AutoGen Studio web UI is the deployable component of interest for Shodan enumeration — the framework itself is a Python library.

**Pollution warning on bare "autogen":** `http.html:"autogen"` = 715 hits — highly polluted. "autogen" is a common programming term (auto-generated code, schema generators, protobuf tooling). The `"autogen" "microsoft"` narrowing (48 hits) is more reliable but still requires sampling.

| Shodan Query | Notes |
|---|---|
| `http.html:"autogen"` | 715 hits — ⚠️ severely polluted; "autogen" = common codegen term across all stacks |
| `ssl:"autogen"` | 103 hits — ⚠️ polluted; full-text cert search matches "autogen" in unrelated cert fields; sample shows blank CNs, not AutoGen framework infrastructure |
| `"autogen" "microsoft"` | 48 hits — Microsoft-scoped narrowing; best available banner query |
| `http.html:"autogen" "microsoft"` | 33 hits — HTML body + Microsoft co-occurrence |
| `("AutoGen" OR "microsoft autogen")` | 16 hits — OR query, grouped as required |
| `"X-Powered-By:" "AutoGen"` | 14 hits — custom HTTP header fingerprint; middleware self-identifying via response header; requires live sampling to confirm vs coincidental header collisions |
| `http.html:"autogen" "agent"` | 10 hits — "agent" co-occurrence; broader but more focused than bare html |
| `"AutoGen Studio"` | 3 hits — Studio-specific banner match |
| `product:"AutoGen"` | 0 hits — no Shodan product facet |
| `http.title:"AutoGen Studio"` | 0 hits — Studio UI title, no matches found |
| `http.html:"AutoGen Studio"` | 0 hits — mixed-case Studio UI string, dead |
| `http.html:"autogen-studio"` | 0 hits — hyphenated Studio variant, dead |
| `http.html:"autogen-studio" http.status:200` | 0 hits — Studio UI with status filter, no open deployments |
| `("AutoGen" OR "microsoft autogen") port:8000` | 0 hits — default port tested, dead |
| `"autogen" "multi-agent"` | 0 hits — paired-token pivot, dead |
| `"autogen" "conversation"` | 0 hits — paired-token pivot, dead |
| `http.securitytxt:"microsoft" http.html:"autogen"` | 0 hits — security.txt + Microsoft co-occurrence, dead |

**Fingerprint lesson:** AutoGen Studio is the only component that creates an HTTP endpoint — the framework itself is library-only. The `"X-Powered-By:" "AutoGen"` custom header query at 14 hits is a new candidate fingerprint — custom headers are harder to pollute than HTML body text. Requires live sampling. The `ssl:"autogen"` at 103 hits is confirmed polluted (blank cert CNs from unrelated services). Until `http.title:"AutoGen Studio"` or `product:"AutoGen"` starts returning hits, the best available paths are `"autogen" "microsoft"` (48 hits, paired query) and `"X-Powered-By:" "AutoGen"` (14 hits, custom header), with live sampling required on both.

---

**Agent framework exposure is a different class of finding.** A reachable agent is not just data disclosure — it is a delegated-authority system acting on behalf of its operator. OpenClaw, OpenDevin, and similar frameworks have shell, browser, email, and calendar primitives. An unauthenticated hit is the operator's hands on your keyboard.

**Quantified — agent frameworks deploy without HTTP auth as the default.** Across every confirmed-exposure population in this section, `http.status:200` matches 100% of the fingerprint count with zero `http.status:401` hits:

| Framework | Fingerprint hits | HTTP 200 | HTTP 401 |
|---|---|---|---|
| OpenHands (`http.title:"OpenHands"`) | 215 | 215 | 0 |
| Clawdbot Control (`http.title:"Clawdbot Control"`) | 1,770 | 1,770 | 0 |
| AgentGPT (`http.html:"agentgpt"`) | 12 | 12 | 0 |
| GPT Researcher (`http.html:"gpt-researcher" port:8000`) | 27 | 27 | 0 |
| SuperAGI (`http.html:"superagi"`) | 10 | 6 | ~4 (only framework with any HTTP-layer auth friction) |

The "delegated authority" framing above is not theoretical — ~2,000+ reachable instances catalogued here are fully open at the HTTP layer. Auth posture, if present, lives inside the application itself (session cookies, token prompts) rather than as a proxy gate. Live probing is required to distinguish "app-level auth enabled" from "fully anonymous access," but the absence of a 401 response means no reverse proxy or gateway is enforcing auth before the app receives the request.
