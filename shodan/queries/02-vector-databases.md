# 2. Vector Databases

_Section verified: April 2026_

The storage layer for RAG, embeddings, and long-term LLM memory. Many of these ship without authentication enabled by default — exposed instances often disclose collection names, schema, embedding model, and the LLM provider keys used to generate vectors.

> Tier legend: **T1** unauthenticated by default · **T2** auth often misconfigured / known bypasses · **T3** recon / fingerprint only.

## ChromaDB

| Shodan Query | Tier | Notes |
|---|---|---|
| `"chroma" port:8000 "api/v1"` | T1 | Pre-0.5 versions default to no auth |
| `"/api/v1/heartbeat" "nanosecond"` | T1 | Most reliable Chroma fingerprint |
| `"chroma" "0.4" port:8000` | T1 | Pre-auth default era |
| `"chromadb" "/api/v2" port:8000` | T2 | Newer multi-tenant, auth optional |
| `"/api/v1/collections" "ChromaDB" -401` | T1 | No auth confirmed |
| `"chroma" "/openapi.json" port:8000` | T2 | Full API spec exposed |
| `"Server: uvicorn" "/api/v1/collections"` | T3 | ChromaDB fingerprint |

```
{"nanosecond heartbeat": 1713384722842816000}
```
What a T1 Chroma heartbeat response looks like. No auth, full API reachable.

## Qdrant

| Shodan Query | Tier | Notes |
|---|---|---|
| `"qdrant" port:6333 "/collections"` | T1 | No auth by default |
| `"qdrant" port:6333 "/dashboard"` | T1 | Web dashboard, full read/write |
| `"qdrant" "/telemetry" port:6333` | T2 | Version + config disclosure |
| `"qdrant" "points" "vectors" port:6333` | T1 | |
| `"qdrant" "snapshots" port:6333` | T1 | Downloadable full-DB backups |
| `http.title:"Qdrant Dashboard"` | T3 | |

## Weaviate

| Shodan Query | Tier | Notes |
|---|---|---|
| `"weaviate" port:8080 "/v1/schema"` | T1 | Anonymous access enabled by default |
| `"weaviate" port:8080 "/v1/objects"` | T1 | |
| `"weaviate" port:8080 "/v1/meta"` | T2 | Version + module disclosure |
| `"weaviate" port:8080 "/v1/nodes"` | T2 | Cluster topology |
| `"weaviate" port:8080 "/v1/backups"` | T2 | Backup enumeration |
| `"weaviate" "modules" "text2vec-openai"` | T2 | Reveals OpenAI embedder configured (key stored server-side, not returned) |
| `"weaviate" "modules" "generative-openai"` | T2 | LLM key configured server-side |
| `http.title:"Weaviate Console"` | T3 | |

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
