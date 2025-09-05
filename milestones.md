# Project Milestones & Progress Log

This lab notebook records the history of the RAG homelab project.  

| Date / Quarter     | Location | Milestones                                                                 | Status       |
|--------------------|----------|----------------------------------------------------------------------------|--------------|
| **25 May 2025**    | Prague   | ▸ Took delivery of the MINISFORUM UM890 Pro Mini PC                          | ✅ Done       |
| **June 2025**    | Prague   | ▸ Summer Sprint planning & preparation                        | ✅ Done       |
| **July & Aug 2025**        | Riga     | ▸ Summer Sprint: Provisioned Proxmox hypervisor on Minisforum UM890 Pro <br> ▸ Stood up 8 Ubuntu VMs (db, embed, retrieval, storage, monitoring, etc.) <br> ▸ CI/CD bootstrap: GitHub Actions → staging → prod for ingestion service <br> ▸ Embedded 112,660 AI research papers into PostgreSQL + Qdrant <br> ▸ First working UI (Streamlit) connected to retrieval service <br> ▸ Version 0.1 released | ✅ Done       |
| **Sept 2025**      | Sofia    | ▸ Planning & preparation for Q4 Sprint <br> ▸ Define Kubernetes/Ansible baseline <br> ▸ Reranker design notes | 🔄 In progress |
| **Q4 2025**        | Sofia    | ▸ Kubernetes rebuild with Terraform + Ansible <br> ▸ Reranking microservice (PyTorch cross-encoder) <br> ▸ Begin work on parallel Classics corpus | ⏳ Planned    |
| **Q1 2026**        | —        | ▸ SharePoint integration (ACL sync via Graph API) <br> ▸ Security services: threat modelling, PII redaction, audit logging | ⏳ Planned    |
| **Q2 2026**        | —        | ▸ Metrics dashboards + golden set <br> ▸ CI/CD hardening + experiment tracking | ⏳ Planned    |
| **Q3 2026**        | —        | ▸ Domain LLM fine-tuning (LoRA/adapters) <br> ▸ Artefact versioning + lineage tracking | ⏳ Planned    |
| **Q4 2026**        | —        | ▸ UI & prompt libraries (with citations) <br> ▸ RAG-specific observability + drift/shadow deploys | ⏳ Planned    |
