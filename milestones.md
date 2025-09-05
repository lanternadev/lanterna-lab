# Project Milestones & Progress Log

This lab notebook records the history of the RAG homelab project.  

## 2025
### 25 May 2025 | Location: Prague
- Took delivery of the MINISFORUM UM890 Pro Mini PC

### Q3 2025 | Location: Riga
- **System foundations**  
  - Provisioned Proxmox hypervisor on Minisforum UM890 Pro.  
  - Stood up 8 Ubuntu VMs (db, embed, retrieval, storage, monitoring, etc.).  
  - CI/CD bootstrap: GitHub Actions → staging → prod for the ingestion service.  
  - Embedded **112,660 AI research papers** into PostgreSQL + Qdrant.  
  - First working UI (Streamlit) connected to retrieval service.  
  - **Version 0.1 released**

### Q4 2025 | Location: Sofia
- Kubernetes rebuild with Terraform + Ansible.  
- Reranking microservice (PyTorch cross-encoder).  
- Begin work on parallel Classics corpus.  


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
