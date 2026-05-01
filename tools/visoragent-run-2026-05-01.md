# VisorAgent Multi-Fingerprint Run — 2026-05-01

**Started:** Fri May  1 12:42:09 AM CDT 2026  
**Corpus:** bypass-prompts.json (10 cases, 6 techniques)  
**Discovery:** 12 Shodan fingerprints  
**Targets:** 243 endpoints  
**Total probes:** 2430  

---

## Shodan Discovery

| Fingerprint | Total Hits |
|-------------|-----------|
| port:11434 (Ollama) | 226,663 |
| http.html:"open-webui" | 19,378 |
| http.title:"Open WebUI" | 19,187 |
| "SillyTavern" | 9,015 |
| http.html:"/v1/chat/completions" | 6,632 |
| http.html:"/v1/models" | 6,536 |
| "llama.cpp" port:8080 | 469 |
| "vLLM" | 350 |
| "sglang" | 54 |
| "koboldcpp" port:5001 | 61 |
| port:1234 "model" | 66 |
| "LM Studio" | 13 |

---

## Progress Log

**[00:47:11]** [674/2430] 20.213.233.110 bp_academic_02

