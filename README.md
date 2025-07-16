## Researching Agentic Excerpt Tagging, Section‑Aware Topic Augmentation & Recommendation for Scholarly Discovery

### 1 Executive Summary
Students and researchers often overlook pivotal literature because public search portals depend on rigid ontologies (e.g. MeSH) or semi‑dynamic graphs such as [OpenAlex Topics](https://docs.openalex.org/api-entities/topics). Librarians cannot anticipate every field‑specific nuance, while users frequently want to search *inside* articles—e.g. *“methods that use CRISPR in zebrafish”*—rather than just at the title/keyword level.

**Agentic Excerpt Tagging (AET)** bridges this gap: an offline large‑language‑model (LLM) pipeline extracts two complementary, transparent metadata layers from full‑text open‑access PDFs:

1. **Excerpt‑Tags** – concise, human‑readable snippets such as  
   *“Surgical excision reduced tumour volume by 45 % in a murine model.”*  
   *“Radiotherapy with cisplatin achieved a 20 % survival benefit.”*  
   *“Meta‑analysis of 320 000 tweets reveals vaccine‑hesitancy trends.”*
2. **Section‑Aware Topic Labels** – OpenAlex topic identifiers (*e.g. topic:V8/Immunology*), **scoped** to the structural section in which they occur (*Methods → Immunology* vs *Results → Oncology*).  
   This mirrors how researchers mentally parse papers and enables precise queries such as **Methods ∩ Machine‑Learning**.

Both layers are embedded, compared and pre‑computed into top‑k similarity lists. A lightweight Django portal offers two modes:

* **Baseline** – conventional faceted search on bibliographic & topic metadata.
* **Experimental** – baseline **plus** excerpt‑tags, section‑aware topics and “You‑might‑also‑read” recommendations (k‑nearest‑neighbour pre‑processing).

> **No machine‑learning inference runs at query time.** All heavy work is performed offline, ensuring a low‑cost, library‑friendly runtime.

Over 24 months we will prototype in two domains (medical & environmental sciences), process ≥ 20 000 open‑access PDFs, refine prompts & models, and release all code, metadata and evaluation artefacts under open licences.

### 2 Alignment with the Call (Criterion 1 — Scope, 10 %)
* **Aim fit** – enriches open‑science infrastructure with AI‑driven metadata and fine‑grained topic augmentation.  
* **Priority topics** – AI & data‑driven innovation for open science; user‑centred scholarly information systems.

### 3 Objectives & Success Metrics (Criterion 2 — Scientific Quality, 40 %)

| ID | Objective | Target (headline metric TBD with researcher) |
|----|-----------|----------------------------------------------|
| **O1** | Design an *agentic*, section‑aware LLM pipeline that extracts excerpt‑tags **and** scoped OpenAlex topic labels. | Macro‑F1 ≥ 0.80 on a 1 000‑record gold set (match if ROUGE‑L ≥ 0.70 **or** cosine ≥ 0.85) |
| **O2** | Process ≥ 20 000 PDFs; generate vectors & top‑k recommendations. | ≥ 90 % of tags/labels rated “useful” (Likert ≥ 4) by two experts, κ ≥ 0.75 |
| **O3** | Deploy a Django + NGINX portal with baseline & experimental modes. | ≥ 95 % uptime; P95 latency < 150 ms |
| **O4** | Conduct two user studies (Y1 & Y2, ≥ 200 participants each). | Metrics TBD (task‑time, nDCG@10, NASA‑TLX) |
| **O5** | Release code, data & evaluation scripts under MIT/CC‑BY. | Zenodo DOI, GitHub repo, OSF preregistration |

### 4 Methodology
#### 4.1 Agentic Excerpt & Topic Tagging (Months 1–8)
* Local LLM actors **Extractor → Tagger → Verifier** using open‑source models (Mixtral‑8×22B, Llama‑3‑8B‑Instruct, Phi‑3‑medium).  
* Extractor splits papers by IMRaD/XML cues; Tagger processes each section independently.  
* Prompt/model sweep on a 1 000‑record validation set; best configuration frozen.

#### 4.2 Similarity & Recommendation (Months 4–12)
* Embed each tag **and** scoped‑topic label; store vectors in FAISS/Qdrant.  
* Pre‑compute top‑k neighbours; persist IDs & ranks in PostgreSQL.

#### 4.3 Runtime Delivery (Months 6–18)
* Containerised Django + NGINX + PostgreSQL stack, cloud‑ or on‑prem‑deployable (incl. air‑gapped).  
* User toggle between baseline and experimental modes.

#### 4.4 Evaluation (Months 9–12 & 18–22)
* Controlled studies with multidisciplinary student cohorts (n ≥ 200 each).  
* Metrics & questionnaires finalised with the Scientific Researcher and ethics board.

#### 4.5 Expansion & Dissemination (Months 16–24)
* Scale corpus to ≥ 20 000 PDFs with expert validation.  
* Publish all artefacts; host a virtual workshop; produce an adoption guide for libraries.

### 5 Work Packages, Milestones & Timeline

| WP | Months | Lead | Key Outputs | Milestone |
|----|--------|------|-------------|-----------|
| **WP1** | 1–4 | Felix | 1 000‑record gold set; system specification | **M1** – validated gold set |
| **WP2** | 3–8 | Adam | Dockerised LLM pipeline; evaluation report | **M2** – pipeline meets O1 |
| **WP3** | 6–10 | Adam / Felix | Enriched metadata v1 (≥ 20 k PDFs) incl. scoped topics | **M3** – metadata v1 released |
| **WP4** | 7–14 | Adam / UU Research Engineer | Public portal (baseline + experimental) | **M4** – portal online |
| **WP5** | 9–12 | Scientific Researcher | Dataset; interim analysis; evaluation criteria | **M5** – Y1 user study complete |
| **WP6** | 9–14 | UU Research Engineers | Performance optimisation; CI/CD; integration tests | **M6** – production‑ready image |
| **WP7** | 12–18 | Adam | Enriched metadata v2; recommendation table | **M7** – recommender passes QA |
| **WP8** | 18–22 | Scientific Researcher | Final analysis; journal submission | **M8** – manuscript submitted |
| **WP9** | 20–24 | Felix | Zenodo deposit; workshop; adoption guide | **M9** – project closed |

### 6 Team & Roles (Criterion 3 — Feasibility, 30 %)

| Role | Name | FTE | Responsibilities |
|------|------|-----|------------------|
| Data Solution Architect | **Adam** | 0.5 | Pipeline design, infra, fine‑tuning, validation, GDPR compliance |
| University Information Officer | **Felix** | 0.3 | Requirements, metadata curation, stakeholder liaison |
| UU Research Engineer | **Roel** | 0.2 | Performance tuning, CI/CD, deployment support |
| Scientific Researcher | **TBD** | 0.5 | Study design, metrics, publications, ethics lead |
| CS Advisor (IR Prof.) | **Prof. TBD** | 0.05 | Quarterly methods audit, gate approvals |
| Student Assistants | — | 0.3 | Annotation, recruitment, testing |

### 7 Budget (EUR, 24 months)

| Item | Amount | Rationale |
|------|--------|-----------|
| Personnel (1.85 FTE core + 0.05 FTE advisor) | ~200 000 | Adam, Felix, Research Engineer, Researcher, Students, Advisor |
| GPU hardware (CapEx) + Cloud Compute (OpEx)| ~50 000 OR NWO Large Compute Grant | 2 × A100‑class GPU Workstations (5‑year life) |


