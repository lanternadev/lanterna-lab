## Homelab Retrieval-Augmented Generation (RAG)  

**A Retrieval-Augmented Generation (RAG) system designed to ingest, embed, and query ~100,000 Artificial Intelligence research papers.**

Retrieval-Augmented Generation (RAG) systems ground a large language model's responses in a specific, authoritative knowledge base. My current RAG system has consumed over 100,000 Artificial Intelligence research papers. 

Each paper is broken into smaller sections ("chunks") and each of these is processed into an “embedding,” a mathematical representation that makes it possible to compare concepts and ideas rather than just matching keywords. 

When a question is asked, it too is converted into mathematical embeddings. The system can then match these embeddings (question & content) and provide the results to the LLM model. The language model then uses the retrieved content, along with other instructions such as tone and style, to generate a clear natural-language answer that stays grounded in the source material.

## Project Goals
1.	Hands-on learning: Build my first non-trivial Linux software project, gaining practical experience with Python, Bash, GitHub, monitoring, and related tooling.
2.	Deep dive into RAG & AI: Explore the architecture, components, and best practices of retrieval-augmented generation systems.
3.	User Experience & Prompt Engineering: Design intuitive interfaces and effective, measurable prompts. 
4.	Practical research assistant: Maintain a weekly-updated repository of the latest AI research, enriched through my RAG pipeline and queried via LLMs.

The lab ingests ~100,000 AI research papers from **SharePoint**, **OpenAlex**, and other sources. It extracts metadata and ACLs, and generates vector embeddings for retrieval-augmented LLM reasoning. Each week I will add the latest available papers. 

## Current Ingestion & Embedding Status 
- PDFs in MinIO: 105688  
- JSON metadata in PostgreSQL: 105688  
- Distinct chunked papers: 105688  
- Papers with ANY chunk embedded: 17524  
- Fully embedded papers (ALL chunks): 17523  
- Any-embed progress: 17524/105688 (16.58%) — Remaining (any): 88164
- Full-embed progress: 17523/105688 (16.58%) — Remaining (full): 88165   
  <sup>21 August 2025 03:28:41</sup>

## Project Structure  

Each functional stage of the pipeline has:  
- a **folder** in the repository (code & configs)  
- a **dedicated VM** (runtime environment)  

This **1:1 mapping** enforces clear separation of concerns and makes it easy to evolve, test, or swap out stages independently. 

| Repo Folder | VM Name            | Description                                                                 |
|-------------|--------------------|-----------------------------------------------------------------------------|
| [Management](https://github.com/lanternadev/rag-lab/tree/main/Management) | lab-1-mgmt01       | Management & orchestration (Terraform, Ansible, backups)                    |
| [Database](https://github.com/lanternadev/rag-lab/tree/main/Database)     | lab-1-db01         | Metadata (PostgreSQL) + Vector DB (Qdrant)                                  |
| [Embed](https://github.com/lanternadev/rag-lab/tree/main/Embed)           | lab-1-embed01      | Embedding Service (currently: nomic-embed-text-v1)                          |
| [Ingestion](https://github.com/lanternadev/rag-lab/tree/main/Ingestion)   | lab-1-ingestion01  | Data ingestion (SharePoint + OpenAlex pipelines)                            |
| [UI](https://github.com/lanternadev/rag-lab/tree/main/UI)                 | lab-1-ui01         | UI layer (Prototyping: Streamlit; Prod: React + TypeScript)                 |
| [Retrieval](https://github.com/lanternadev/rag-lab/tree/main/Retrieval)   | lab-1-retrieval01  | FastAPI retrieval microservice + LangChain orchestration                    |
| [Storage](https://github.com/lanternadev/rag-lab/tree/main/Storage)       | lab-1-storage01    | Object storage (MinIO)                                                      |
| [Monitoring](https://github.com/lanternadev/rag-lab/tree/main/Monitoring) | lab-1-monitoring01 | Monitoring & Logging (Prometheus, Grafana, Alertmanager, Filebeat → Elasticsearch) |
| [Model](https://github.com/lanternadev/rag-lab/tree/main/Model)           | lab-1-model01      | Training, fine-tuning, reranking, and evaluation experiments                |
## Project Timeline (2025–2027)

| Dates           | Project                          | Notes                                                                 |
|-----------------|----------------------------------|-----------------------------------------------------------------------|
| Jun – Aug 2025  | Core RAG Build (hybrid: on-prem + cloud LLM) | Self-hosted ingestion, storage, retrieval; query LLM (ChatGPT) in cloud; ~100k docs; basic CLI |
| Sept – Oct 2025 | Prompt Engineering & User Experience | Design intuitive interfaces, prototype user flows, and build first prompt libraries |
| Nov 2025 – Jan 2026 | SharePoint Security Integration (hybrid) | End-to-end permission flow; SharePoint in cloud, RAG infra on-prem; fast propagation of ACL changes |
| Feb – Apr 2026  | Kubernetes/Terraform/Ansible (on-prem) | Refactor deployment of existing pipeline services into containerized + IaC form |
| May – Jul 2026  | Metrics & Golden Set (on-prem)   | Optimise dashboards, observability stack, golden dataset evaluation; Grafana/Prometheus; Build evaluation pipeline with retrieval quality metrics (Precision@k, Recall@k, MRR@k, etc) |
| Aug – Oct 2026  | Domain LLM (hybrid)              | Fine-tune pipeline on AI/ML research corpora (local GPUs + cloud training options); LoRA/adapters; prompt workflows |
| Nov 2026 – Jan 2027 | Graph Retriever & Re-ranking (hybrid) | Multi-hop, relationship-aware retrieval with Mistral-7B (cloud); pipeline infra on-prem |
| Feb – Jun 2027  | Cloud Migration (hybrid → cloud-native) | Migrate pipeline to AWS/GCP; hybrid homelab ↔ cloud; ensure metric parity during transition |
| Jul – Dec 2027  | Cloud: Land & Expand | Fully cloud-based scaling, managed services, cost optimisation, cloud-first workloads |


## RAG Lab Infrastructure  

Hosted on a Minisforum UM890 Pro running Proxmox  
- Ryzen 9 8945HS  
- 64 GB DDR5 RAM  
- 2 TB NVMe
  
[Click here to read about my ongoing efforts to manage CPU & RAM allocation](https://github.com/lanternadev/rag-lab/blob/main/Management/deploy/vmsetup.md)


## CI/CD Overview

I am building a simple branch-based promotion pipeline with two environments per service, orchestrated by GitHub Actions:

- **dev → staging**  
  Every push/merge to `dev` triggers a workflow that builds, runs tests, and deploys to the staging endpoint on the target VM (separate systemd unit/port).

- **main → production**  
  Merging `dev` into `main` triggers a workflow that (optionally after manual approval or stricter checks) deploys to the production endpoint on the same VM (prod unit/port).

**Flow**

`feature branch → pull request → dev (auto deploy to staging) → verify (/health, /version) → PR dev→main → approval/tests → main (auto deploy to prod)`

**Endpoints (example: Embed)**

- Staging: `http://embed.example.com:8000/health`  
- Production: `https://embed.example.com/health`  

Both expose `/health` and `/version` for post-deploy checks.


## Performance & Tuning
Now that I have a functional pipeline, I'm learning how to measure and tune performance. Going forward I'll add more hardware, however the metrics I establish now will always be relevant. 

At present I’m running entirely on CPU (Ryzen 9 8945HS), with no GPU acceleration. While that imposes obvious limits, it's a nice opportunity to push system limits through careful configuration and monitoring. In other words, and in less spinny terms, my hardware is underspecced but I'm making the most of it by learning about tuning.

One of the main areas I’m exploring is PostgreSQL performance. These are some of the metrics I'm tracking:

## PostgreSQL Health Check Summary

| Check                        | Command (psql)                                                                                                                                          | Healthy Target                  |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|
| Global cache hit ratio        | SELECT round(sum(blks_hit)*100/nullif(sum(blks_hit)+sum(blks_read),0),2) FROM pg_stat_database;                                                         | > 95%                           |
| Per-database cache ratio      | SELECT datname, round(blks_hit*100.0/nullif(blks_hit+blks_read,0),2) FROM pg_stat_database ORDER BY blks_read DESC;                                      | > 95% for main DB               |
| Table-level IO hot spots      | SELECT relname, heap_blks_read, heap_blks_hit, round(heap_blks_hit*100.0/nullif(heap_blks_read+heap_blks_hit,0),2) AS hit_pct FROM pg_statio_user_tables ORDER BY heap_blks_read DESC LIMIT 15; | hit_pct > 95%, low reads        |
| Index-level IO hot spots      | SELECT relname, indexrelname, idx_blks_read, idx_blks_hit, round(idx_blks_hit*100.0/nullif(idx_blks_hit+idx_blks_read,0),2) AS hit_pct FROM pg_statio_user_indexes ORDER BY idx_blks_read DESC LIMIT 15; | hit_pct > 95%, low reads        |
| Slowest queries               | SELECT query, calls, round(mean_exec_time,2) ms FROM pg_stat_statements ORDER BY mean_exec_time DESC LIMIT 10;                                           | mean_exec_time < 50ms (OLTP)    |
| Chunk fetch query efficiency  | EXPLAIN (ANALYZE, BUFFERS) SELECT id, work_id, text FROM chunks WHERE embedded = FALSE ORDER BY created_at LIMIT 1000;                                   | Execution time < 50ms, mostly hits |


## AuthZ Service for Azure AD + SharePoint (Nov 2025 – Jan 2026)

A lightweight authorization microservice written in C#/.NET 8*  

It evaluates whether a given user can perform an action on a resource, using Azure AD groups/roles and SharePoint ACLs as sources of truth.
 
Python services call the AuthZ API to obtain Permit/Deny decisions and (for search) Qdrant filters.

### Features

- **JWT validation** against Azure AD (MSAL / Microsoft.Identity.Web).  
- **Policy engine** (RBAC + ABAC + contextual rules).  
- **SharePoint ACL integration** via Microsoft Graph API.  
- **Caching** with Redis for fast hot-path decisions (< 5ms).  
- **Webhooks**: Graph change notifications keep ACLs in sync.  
- **Explainability**: every decision includes matched policies and reasons.  
- **Obligations**: optional enforcement hints (`watermark`, `redact`, `log-purpose`).  
- **Qdrant integration**: generates vector DB filters consistent with SharePoint permissions.

## RAG CLI — Knowledge Repository Manager

I'm working on centralising the utilities I've build into a CLI. As well as some basic management tasks, this will enable users to add standalone documents to the system, experiment with download JSON filters and explore the repository.  

- `rag testfilter` — Preview OpenAlex filter results (count, sample titles). 
- `rag count` — Show total results for the current filter.  
- `rag fetch` — Fetch metadata pages to `/metadata/pages` (no PDFs).  
- `rag download` — Download per-work metadata and PDFs into `/metadata` and `/pdf`.  
- `rag add <pdf>` — Add a standalone PDF; enrich metadata via DOI/AI; validate and save.  
- `rag validate` — Validate all JSON/PDF pairs; quarantine failures.  
- `rag ingest` — Upsert validated works into Postgres.  
- `rag dedupe` — Merge duplicates using IDs and heuristics.  
- `rag status` — Show repository health (counts, quarantined files, last run info).

## Future Exploratory Areas

- **Data Lineage Tracking**: OpenLineage for permission/change audits  
- **Federated Learning**: Train models across homelab + cloud without raw data transfer  
- **Homomorphic Encryption**: Secure processing of sensitive documents  
- **RAGAS / LangSmith**: Benchmark retrieval quality beyond golden sets  
- **Drift Detection**: Monitor embedding/model decay in production  
