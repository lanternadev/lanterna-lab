# Homelab Retrieval-Augmented Generation (RAG)

A Retrieval-Augmented Generation (RAG) system designed to ingest, embed, and query around 100,000 artificial intelligence research papers.

👉 **Note**: This repo is a **shop window**.  
The actual working codebase is private, as this is first and foremost a *learning tool*.  
When components are production-ready, they’ll be released here or as separate open-source modules.

Retrieval-Augmented Generation (RAG) systems ground a large language model's responses in a specific, authoritative knowledge base. This system has consumed over 100,000 AI research papers. Each paper is broken into smaller sections ("chunks") and each of these is processed into an embedding, a mathematical representation that makes it possible to compare concepts and ideas rather than just matching keywords.

When a question is asked, it too is converted into embeddings. The system matches these embeddings against the stored research and provides the results to the language model. The model then uses the retrieved content, along with style and instruction prompts, to generate a natural-language answer that stays grounded in the source material.

## Project goals
1. Hands-on learning: Build a non-trivial Linux software project, gaining practical experience with Python, Bash, GitHub, monitoring, and related tooling.
2. Deep dive into RAG and AI: Explore the architecture, components, and best practices of retrieval-augmented generation systems.
3. User experience and prompt engineering: Design intuitive interfaces and effective, measurable prompts.
4. Practical research assistant: Maintain a weekly-updated repository of the latest AI research, enriched through the RAG pipeline and queried via LLMs.

The lab ingests AI research papers from SharePoint, OpenAlex, and other sources. It extracts metadata and ACLs, and generates vector embeddings for retrieval-augmented reasoning. Each week new papers are added.

## Current ingestion and embedding status
- PDFs in object storage: 105688  
- JSON metadata in PostgreSQL: 105688  
- Distinct chunked papers: 105688  
- Papers with any chunk embedded: 17524  
- Fully embedded papers: 17523  
- Any-embed progress: 17524/105688 (16.58%)  
- Full-embed progress: 17523/105688 (16.58%)  
  21 August 2025 03:28:41

## Project structure

Each functional stage of the pipeline has its own folder in the repository and its own dedicated VM. This one-to-one mapping enforces clear separation of concerns and makes it easy to evolve, test, or swap out stages independently.

| Repo folder   | VM name           | Description                                                                 |
|---------------|------------------|-----------------------------------------------------------------------------|
| Management    | lab-1-mgmt01     | Management and orchestration (Terraform, Ansible, backups)                  |
| Database      | lab-1-db01       | Metadata in PostgreSQL and vector database in Qdrant                        |
| Embed         | lab-1-embed01    | Embedding service                                                           |
| Ingestion     | lab-1-ingestion01| Data ingestion from SharePoint and OpenAlex                                  |
| UI            | lab-1-ui01       | User interface layer                                                         |
| Retrieval     | lab-1-retrieval01| Retrieval microservice and orchestration                                     |
| Storage       | lab-1-storage01  | Object storage                                                               |
| Monitoring    | lab-1-monitoring01| Monitoring and logging (Prometheus, Grafana, Elasticsearch, etc.)           |
| Model         | lab-1-model01    | Training, fine-tuning, reranking, and evaluation experiments                |

## Project timeline (2025–2027)

| Dates           | Project                          | Notes                                                                 |
|-----------------|----------------------------------|-----------------------------------------------------------------------|
| Jun – Aug 2025  | Core RAG build                   | Self-hosted ingestion, storage, retrieval; query LLM in cloud; ~100k docs; basic CLI |
| Sept – Oct 2025 | Prompt engineering and UX        | Design interfaces, prototype user flows, build first prompt libraries |
| Nov 2025 – Jan 2026 | SharePoint security integration | End-to-end permission flow; SharePoint in cloud, RAG infra on-prem; fast propagation of ACL changes |
| Feb – Apr 2026  | Kubernetes/Terraform/Ansible     | Containerise and refactor deployment into infrastructure as code form |
| May – Jul 2026  | Metrics and golden set           | Dashboards, golden dataset evaluation; retrieval metrics (Precision@k, Recall@k, MRR@k) |
| Aug – Oct 2026  | Domain LLM                       | Fine-tune on AI/ML corpora; adapters and workflows                    |
| Nov 2026 – Jan 2027 | Graph retriever and re-ranking | Multi-hop, relationship-aware retrieval; hybrid pipeline              |
| Feb – Jun 2027  | Cloud migration                  | Hybrid to cloud-native migration; metric parity maintained            |
| Jul – Dec 2027  | Cloud: land and expand           | Scale out with cloud services, optimisation, managed workloads        |

## RAG lab infrastructure

Hosted on a Minisforum UM890 Pro running Proxmox  
- Ryzen 9 8945HS  
- 64 GB DDR5 RAM  
- 2 TB NVMe  

## CI/CD overview

A branch-based promotion pipeline is in place with two environments per service, orchestrated by GitHub Actions.

- dev → staging: every push/merge to dev triggers a workflow that builds, runs tests, and deploys to staging on the target VM.  
- main → production: merging dev into main triggers a workflow that deploys to production, optionally gated by manual approval.  

Endpoints expose /health and /version for post-deploy checks.

## AuthZ service for Azure AD and SharePoint (Nov 2025 – Jan 2026)

A lightweight authorization microservice written in C#/.NET 8. It evaluates whether a given user can perform an action on a resource, using Azure AD groups/roles and SharePoint ACLs as sources of truth.

Python services call the AuthZ API to obtain permit/deny decisions and Qdrant filters.

## RAG CLI — Knowledge Repository Manager

A command-line interface for centralising utilities, including adding standalone documents, managing metadata, validating pairs, deduplication, ingestion, and repository health checks.

Example commands:
- rag testfilter  
- rag count  
- rag fetch  
- rag download  
- rag add <pdf>  
- rag validate  
- rag ingest  
- rag dedupe  
- rag status  

## Future exploratory areas

- Data lineage tracking  
- Federated learning  
- Homomorphic encryption  
- Advanced retrieval benchmarks  
- Drift detection  
