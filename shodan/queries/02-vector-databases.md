# 2. Vector Databases

_Section verified: April 2026_

The storage layer for RAG, embeddings, and long-term LLM memory. Many of these ship without authentication enabled by default — exposed instances often disclose collection names, schema, embedding model, and the LLM provider keys used to generate vectors.

> Tier legend: **T1** unauthenticated by default · **T2** auth often misconfigured / known bypasses · **T3** recon / fingerprint only.

## ChromaDB

| Shodan Query | Tier | Notes |
|---|---|---|
| `"chroma"` | T3 | 2,311 hits — ⚠️ noisy; "chroma" collides with chroma-subsampling, color-correction UIs, Razer Chroma; use as last resort |
| `product:"Chroma"` | T1 | 1,838 hits — canonical product facet; sampled hits on 8000/8100 confirm vector DB |
| `product:"Chroma" port:8000` | T1 | 1,139 hits — traditional default port |
| `product:"Chroma" "uvicorn"` | T1 | 353 hits — highest-confidence: Chroma ASGI server + product facet |
| `"chroma" "uvicorn" port:8000` | T1 | 230 hits — same intent, no product-facet dependency |
| `http.title:"Chroma"` | T2 | 94 hits — title match, some non-DB Chroma products mixed in |
| `http.html:"chromadb"` | T2 | 76 hits — name-specific HTML body match |
| `"chromadb"` | T2 | 47 hits — name-specific banner match |
| `http.html:"/api/v1/heartbeat"` | T1 | 22 hits — heartbeat path leaked into HTML response |
| `"chroma" port:8100` | T3 | 13 hits — post-0.5 default port |
| `product:"Chroma" port:8100` | T3 | 13 hits — product facet on new default |
| `"chroma" "0.6"` | T3 | 12 hits — current version era |
| `"chromadb" port:8000` | T3 | 5 hits — narrow |
| `"chroma" "0.5"` | T3 | 2 hits — transitional auth-optional era |
| `"chroma" "0.4"` | T3 | 1 hit — pre-auth legacy era, nearly extinct |

**Fingerprint note:** Shodan does **not** index arbitrary HTTP endpoint paths (`/api/v1/heartbeat`, `/api/v1/collections`, `/openapi.json`) in the root banner — it crawls `/` and headers only. Path-based fingerprints only work when the path string appears inside HTML response bodies (see `http.html:"/api/v1/heartbeat"` above). This is a general lesson applicable across the catalogue.

**Status-code caveat:** Chroma returns 404 at `/` regardless of auth configuration (no root handler registered), so `http.status:200` and `http.status:401` both return 0 for `product:"Chroma"`. Auth-enabled vs unauth cannot be distinguished from Shodan banner data alone — probe `/api/v1/heartbeat` live to tell them apart.

```
{"nanosecond heartbeat": 1713384722842816000}
```
What a T1 Chroma heartbeat response looks like. No auth, full API reachable.

## Qdrant

| Shodan Query | Tier | Notes |
|---|---|---|
| `http.html:"qdrant"` | T1 | 949 hits — canonical fingerprint; HTML-body match catches what banner doesn't |
| `http.html:"qdrant" port:443` | T1 | 331 hits — TLS-fronted deployments (proxy or native) |
| `http.html:"qdrant" port:80` | T1 | 221 hits — plaintext proxy-fronted |
| `"qdrant"` | T2 | 189 hits — banner-level match, narrower subset |
| `"qdrant" "vector"` | T2 | 31 hits — banner + domain term |
| `http.html:"qdrant" port:6333` | T1 | 27 hits — **direct default-port exposure; highest-risk subset, no proxy auth layer** |
| `"qdrant" "dashboard"` | T1 | 17 hits — web dashboard accessible, full read/write |
| `http.title:"Qdrant"` | T3 | 13 hits — title match |
| `http.html:"Qdrant Web UI"` | T2 | 6 hits — dashboard UI marker |
| `http.html:"Qdrant Dashboard"` | T1 | 3 hits — specific dashboard HTML |
| `"qdrant" "collections"` | T3 | 1 hit — banner + API term |
| `http.html:"qdrant-version"` | T3 | 1 hit — response header string leaked into HTML |

**Fingerprint field lesson (generalizable):** `"qdrant"` bare returns 189 hits but `"qdrant" port:6333` returns **0** — not a bug. Bare `"<term>"` matches Shodan's banner text (headers + initial response). On port 6333, Qdrant's REST root returns JSON without the literal word "qdrant" in headers, so banner match misses. The `http.html:` field parses response bodies and does catch it. Always try both `"<term>"` and `http.html:"<term>"`.

**Path indexing:** Qdrant's `/telemetry`, `/snapshots`, and `/points/search` endpoints don't surface in Shodan at all — even the terms "telemetry", "points", "snapshots" paired with "qdrant" return 0 on every combination tested. These endpoints require live probing, not Shodan queries.

**Deployment shift:** 922 of 949 Qdrant instances are no longer on port 6333 (80+443+proxies = ~90% of exposure). Same pattern as n8n and Flowise.

## Weaviate

| Shodan Query | Tier | Notes |
|---|---|---|
| `http.html:"weaviate"` | T1 | 1,647 hits — canonical fingerprint |
| `"weaviate"` | T1 | 1,326 hits — banner match, anonymous access by default |
| `http.html:"weaviate" port:8080` | T1 | **899 hits** — ~55% of instances still on default port; direct unauth access likely |
| `"weaviate" port:8080` | T1 | 564 hits — banner + default port |
| `"weaviate" port:80` | T2 | 206 hits — plaintext proxy-fronted |
| `"weaviate" port:443` | T2 | 118 hits — TLS-fronted |
| `"weaviate" "meta"` | T2 | 19 hits — meta endpoint term in banner |
| `http.title:"Weaviate"` | T3 | 10 hits — title match |
| `http.title:"Weaviate Console"` | T3 | 3 hits — console UI title |
| `http.html:"Weaviate Console"` | T3 | 3 hits — console UI HTML |
| `"weaviate" "schema"` | T3 | 1 hit — schema term in banner |

**Deployment anomaly:** Weaviate is the only vector DB verified so far that **did not** mass-migrate off its default port. 899 of 1,647 instances (~55%) remain directly exposed on 8080, vs Qdrant (3% on 6333) and Flowise (~0% on 3000). Likely explanation: Weaviate is more often run behind application backends rather than user-facing reverse proxies — the embedding service sits between an app and the data, not between a user and a UI. The practical impact: direct 8080 hits are higher-confidence "this is a real Weaviate API, no auth layer in front" signals.

**Path indexing:** All 5 original `/v1/schema`, `/v1/objects`, `/v1/meta`, `/v1/nodes`, `/v1/backups` queries returned 0. Shodan doesn't crawl these paths. Live-probing the `/v1/meta` endpoint on confirmed Weaviate hosts is required to enumerate version + module config.

**Module fingerprint gap:** `"text2vec-openai"` and `"generative-openai"` both return 0 alone and paired — module config is server-side metadata, not reflected in root banner or HTML. To detect OpenAI-key-backed Weaviate deployments, probe `/v1/meta` live on the 899 port:8080 hits.

**Auth reality check:** Weaviate ships with anonymous access enabled unless `AUTHENTICATION_ANONYMOUS_ACCESS_ENABLED=false` is explicitly set. With 899 direct-exposure hits, assume most are unauthenticated absent contrary evidence.

## Milvus

| Shodan Query | Tier | Notes |
|---|---|---|
| `"milvus" port:19530` | T1 | gRPC API, auth optional |
| `"milvus" port:9091 "/api/v1/health"` | T3 | |
| `"milvus" port:9091 "/metrics"` | T2 | Prometheus metrics leak |
| `"milvus" "Attu" port:8000` | T1 | Milvus admin GUI |
| `http.title:"Attu" "Milvus"` | T1 | |
| `"milvus" port:2379 "etcd"` | T2 | Metadata store exposed |
| `"milvus" "MinIO" port:9000` | T2 | Object storage — raw vectors |

## Object Storage & Artifact Stores

Vector databases are the search layer. Object storage is where the models, embeddings, raw documents, training datasets, and DB snapshots actually live on disk. Milvus, Weaviate, and most RAG pipelines back onto MinIO or S3 by default. A compromised bucket is typically a higher-impact finding than the index in front of it.

| Shodan Query | Tier | Notes |
|---|---|---|
| `"MinIO" port:9000` | T2 | S3-compatible store — default backing for Milvus/Weaviate/RAG |
| `"Server: MinIO" OR title:"MinIO Browser" port:9000` | T1 | Console / browser UI — often zero auth |
| `"MinIO" "bucket" OR "objects" port:9000` | T1 | Exposed bucket listing — models + vectors |
| `"MinIO Console" port:9001` | T2 | |
| `"MinIO" port:9000 -"auth"` | T1 | Public MinIO — object storage free-for-all |
| `"Docker Registry" "/v2/" port:5000` | T1 | Anonymous catalog read = full image list |
| `"registry/2.0" "/v2/_catalog" -"unauthorized"` | T1 | Open catalog, pull access typically follows |
| `"harbor" port:80 OR port:443` | T2 | Harbor registry — enterprise AI model containers |
| `http.title:"Harbor"` | T2 | |
| `port:51000 OR port:55000 "Docker Registry"` | T2 | Non-default registry ports, same exposure class |

**Triage note:** Anonymous `/v2/_catalog` read ≠ anonymous push. But anonymous pull of internal images leaks entire codebases, build-time secrets in layer history, and the full supply chain of whatever ships from that registry. Rate severity on _read vs. push_ separately before escalating. See §12 for Docker daemon and container runtime exposure (distinct from the artifact store).

## Elasticsearch / OpenSearch

| Shodan Query | Tier | Notes |
|---|---|---|
| `"elasticsearch" port:9200 "/_cat/indices" "vector"` | T2 | |
| `"elasticsearch" "8." port:9200 "dense_vector"` | T3 | v8+ native vector |
| `"elasticsearch" "knn" port:9200` | T3 | |
| `"elasticsearch" port:9200 "/_cluster/health"` | T1 | Cluster discovery |
| `"kibana" port:5601 "embeddings"` | T2 | |
| `"opensearch" port:9200 "knn"` | T3 | |
| `"opensearch-dashboards" port:5601` | T2 | |

## PostgreSQL + pgvector

| Shodan Query | Tier | Notes |
|---|---|---|
| `"PostgreSQL" port:5432 "pgvector"` | T3 | Most installs auth-gated |
| `"Supabase" port:5432` | T3 | Ships with pgvector |
| `"pgAdmin" port:5050` | T2 | Often default creds |
| `"Neon" "postgres" port:5432 "vector"` | T3 | |
| `"Timescale" port:5432 "pgvector"` | T3 | |

## Redis Vector Search

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Redis" port:6379 "search" "FT.SEARCH"` | T2 | |
| `"Redis Stack" port:6379` | T3 | |
| `"RedisInsight" port:8001` | T2 | Web GUI, auth optional |
| `"Redis" port:6379 -"AUTH" -"NOAUTH"` | T1 | No password set |

## Other Vector DBs

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Vespa" port:8080 "/document/v1"` | T2 | |
| `"typesense" port:8108 "/collections"` | T3 | API key required |
| `"typesense" port:8108 "/keys"` | T2 | API key enumeration attempt |
| `"marqo" port:8882 "/indexes"` | T2 | |
| `"lancedb" port:8000` | T3 | |
| `"surrealdb" port:8000 "/sql"` | T2 | |
| `"arangodb" port:8529 "/_api/collection"` | T2 | |
| `http.title:"ArangoDB Web Interface"` | T3 | |
| `"Zilliz" port:9091` | T3 | |
| `"MongoDB" port:27017 "vector"` | T3 | |
| `"MongoDB" port:27017 -"auth"` | T1 | Unauth MongoDB |
| `"mongo-express" port:8081` | T2 | Web admin, default creds common |
| `"clickhouse" port:8123 "vector" OR "similarity"` | T2 | ClickHouse OLAP + vector hybrid (analytics stacks) |
| `"cassandra" port:9042 "vector"` | T3 | Apache Cassandra 5.x+ vector extensions |
| `"txtai" port:8000` | T2 | Lightweight embeddings / semantic search DB |

## Graph Databases / Memory

| Shodan Query | Tier | Notes |
|---|---|---|
| `"Neo4j" port:7474 "browser"` | T2 | Default creds neo4j/neo4j common |
| `"neo4j" port:7474 "graph" OR "cypher"` | T2 | GraphRAG / knowledge graph workloads |
| `"Dgraph" port:8080 "/admin"` | T2 | |
| `"dgraph" "ratel" port:8000` | T2 | Web UI |
| `"memgraph" "query" port:7687` | T3 | In-memory graph DB, common in agent memory |
| `"Mem0" port:8000` | T2 | AI memory store |
