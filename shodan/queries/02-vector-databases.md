# 2. Vector Databases

The storage layer for RAG, embeddings, and long-term LLM memory. Many of these ship without authentication enabled by default — exposed instances often disclose collection names, schema, embedding model, and the LLM provider keys used to generate vectors.

## ChromaDB

| Shodan Query | Notes |
|---|---|
| `"chroma" port:8000 "api/v1"` | |
| `"/api/v1/heartbeat" "nanosecond"` | |
| `"chromadb" port:5500` | |
| `"chroma" "0.4" port:8000` | Pre-auth default era |
| `"chromadb" "/api/v1/tenants"` | |
| `"chromadb" "/api/v2" port:8000` | Newer multi-tenant |
| `"/api/v1/collections" "ChromaDB" -401` | No auth confirmed |
| `"chroma" "/openapi.json" port:8000` | Full API spec exposed |
| `"Server: uvicorn" "/api/v1/collections"` | ChromaDB fingerprint |

## Qdrant

| Shodan Query | Notes |
|---|---|
| `"qdrant" port:6333 "/collections"` | |
| `"qdrant" port:6333 "/dashboard"` | |
| `"qdrant" "/telemetry" port:6333` | |
| `"qdrant" "points" "vectors" port:6333` | |
| `"qdrant" "snapshots" port:6333` | Downloadable DB backups |
| `http.title:"Qdrant Dashboard"` | |

## Weaviate

| Shodan Query | Notes |
|---|---|
| `"weaviate" port:8080 "/v1/schema"` | |
| `"weaviate" port:8080 "/v1/objects"` | |
| `"weaviate" port:8080 "/v1/meta"` | Version + module disclosure |
| `"weaviate" port:8080 "/v1/nodes"` | Cluster topology |
| `"weaviate" port:8080 "/v1/backups"` | Backup enumeration |
| `"weaviate" "modules" "text2vec-openai"` | Reveals embedder + OpenAI key |
| `"weaviate" "modules" "generative-openai"` | LLM key stored server-side |
| `http.title:"Weaviate Console"` | |

## Milvus

| Shodan Query | Notes |
|---|---|
| `"milvus" port:19530` | |
| `"milvus" port:9091 "/api/v1/health"` | |
| `"milvus" port:9091 "/metrics"` | Prometheus metrics |
| `"milvus" "Attu" port:8000` | Milvus admin GUI |
| `http.title:"Attu" "Milvus"` | |
| `"milvus" port:2379 "etcd"` | Metadata store |
| `"milvus" "MinIO" port:9000` | Object storage backing |

## Elasticsearch / OpenSearch

| Shodan Query | Notes |
|---|---|
| `"elasticsearch" port:9200 "/_cat/indices" "vector"` | |
| `"elasticsearch" "8." port:9200 "dense_vector"` | v8+ native vector |
| `"elasticsearch" "knn" port:9200` | |
| `"elasticsearch" port:9200 "/_cluster/health"` | |
| `"kibana" port:5601 "embeddings"` | |
| `"opensearch" port:9200 "knn"` | |
| `"opensearch-dashboards" port:5601` | |

## PostgreSQL + pgvector

| Shodan Query | Notes |
|---|---|
| `"PostgreSQL" port:5432 "pgvector"` | |
| `"Supabase" port:5432` | Ships with pgvector |
| `"pgAdmin" port:5050` | Often default creds |
| `"Neon" "postgres" port:5432 "vector"` | |
| `"Timescale" port:5432 "pgvector"` | |

## Redis Vector Search

| Shodan Query | Notes |
|---|---|
| `"Redis" port:6379 "search" "FT.SEARCH"` | |
| `"Redis Stack" port:6379` | |
| `"RedisInsight" port:8001` | Web GUI |
| `"Redis" port:6379 -"AUTH" -"NOAUTH"` | No password set |

## Other Vector DBs

| Shodan Query | Notes |
|---|---|
| `"Vespa" port:8080 "/document/v1"` | |
| `"typesense" port:8108 "/collections"` | |
| `"typesense" port:8108 "/keys"` | API key enumeration |
| `"marqo" port:8882 "/indexes"` | |
| `"lancedb" port:8000` | |
| `"surrealdb" port:8000 "/sql"` | |
| `"arangodb" port:8529 "/_api/collection"` | |
| `http.title:"ArangoDB Web Interface"` | |
| `"Zilliz" port:9091` | |
| `"MongoDB" port:27017 "vector"` | |
| `"MongoDB" port:27017 -"auth"` | |
| `"mongo-express" port:8081` | Web admin |

## Graph Databases / Memory

| Shodan Query | Notes |
|---|---|
| `"Neo4j" port:7474 "browser"` | |
| `"Dgraph" port:8080 "/admin"` | |
| `"dgraph" "ratel" port:8000` | Web UI |
| `"Memgraph" port:7687` | |
| `"Mem0" port:8000` | AI memory store |
