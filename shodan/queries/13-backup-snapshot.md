# 13. Backup / Snapshot Exposure

_Section verified: April 2026_

> Tier legend: 🟥 **T1** unauthenticated by default · 🟧 **T2** auth often misconfigured / known bypasses · 🟨 **T3** recon / fingerprint only.

| Shodan Query | Tier | Notes |
|---|---|---|
| `"qdrant" "/snapshots" port:6333` | 🟥 | Downloadable .snapshot files — full vector DB |
| `"milvus" "MinIO" port:9000 "bucket"` | 🟥 | Raw vectors in object storage |
| `"chroma" "/persist" port:8000` | 🟧 | Persistent directory exposure |
| `"weaviate" "/v1/backups" port:8080` | 🟧 | |
| `"elasticsearch" "/_snapshot" port:9200` | 🟧 | |
| `"/var/lib/docker" "overlay2"` | 🟧 | Container layer paths leaking |
| `"backup.tar" OR "dump.sql" port:80` | 🟥 | HTTP-served backup files |
