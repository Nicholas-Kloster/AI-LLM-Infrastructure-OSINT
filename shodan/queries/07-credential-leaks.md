# 7. Credential Leaks & Misconfigurations

The high-impact category. Exposed environment files, unauthenticated container daemons, and downloadable database snapshots collapse the attack chain from "exposed service" to "full provider account takeover" in a single step.

## Exposed API Keys

| Shodan Query | Notes |
|---|---|
| `"OPENAI_API_KEY" port:8000` | |
| `"sk-" "openai" port:3000` | |
| `"ANTHROPIC_API_KEY"` | |
| `"HF_TOKEN" port:8080` | |
| `".env" "API_KEY" port:8000` | |

## Docker / Container Misconfigurations

| Shodan Query | Notes |
|---|---|
| `"Docker" port:2375 "api"` | Unauth Docker daemon |
| `"Portainer" port:9000` | |

## Backup / Snapshot Exposure

| Shodan Query | Notes |
|---|---|
| `"qdrant" "/snapshots" port:6333` | Downloadable .snapshot files |
| `"milvus" "MinIO" port:9000 "bucket"` | Raw vectors in object storage |
| `"chroma" "/persist" port:8000` | |
| `"weaviate" "/v1/backups" port:8080` | |
| `"elasticsearch" "/_snapshot" port:9200` | |
