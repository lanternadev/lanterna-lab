# Project Milestones & Progress Log

This lab notebook records the history of the RAG homelab project: completed milestones, sprint outcomes, key design changes, and lessons learned.  

## 2025
### 25 May 2025
- Took delivery of the MINISFORUM UM890 Pro Mini PC

### Q2 2025
- **Initial ingestion pipeline**  
  - Downloaded and normalised ~20k AI research papers (OpenAlex).  
  - Prototyped embeddings with OpenAI `text-embedding-3-small`.  
  - Simple SQLite index + naive retrieval.

### Q3 2025
- **System foundations**  
  - Provisioned Proxmox hypervisor on Minisforum UM890 Pro.  
  - Stood up 8 Ubuntu VMs (db, embed, retrieval, storage, monitoring, etc.).  
  - CI/CD bootstrap: GitHub Actions → staging → prod for the ingestion service.  
  - Embedded **112,660 AI research papers** into PostgreSQL + Qdrant.  
  - First working UI (Streamlit) connected to retrieval service.  
  - **Version 0.1 released — “the plumbing milestone.”**

### Q4 2025 (planned)
- Kubernetes rebuild with Terraform + Ansible.  
- Reranking microservice (PyTorch cross-encoder).  
- Begin parallel Classics corpus.  

---

## 2026

### Q1 2026 (planned)
- SharePoint integration (ACL sync via Graph API).  
- Security services: threat modelling, PII redaction, audit logging.  

### Q2 2026 (planned)
- Metrics dashboards + golden set.  
- CI/CD hardening + experiment tracking.  

### Q3 2026 (planned)
- Domain LLM fine-tuning (LoRA/adapters).  
- Artefact versioning + lineage tracking.  

### Q4 2026 (planned)
- UI & prompt libraries (with citations).  
- RAG-specific observability + drift/shadow deploys.  

---

## How to Use This Log

- Treat it like a **lab diary**: every sprint gets a short summary, bulleting wins, blockers, and fixes.  
- Keep **planned vs achieved** side by side — transparency shows the real learning process.  
- Cross-link to issues, PRs, or design docs as needed (`[link](../folder/file.md)`).
