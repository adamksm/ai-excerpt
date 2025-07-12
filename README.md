## Researching Agentic Excerpt Tagging & Recommendation for Scholarly Discovery

### 1  Executive Summary
Students and researchers often overlook pivotal literature because public search portals depend on rigid ontologies (e.g. MeSH, OpenAlex). Librarians cannot anticipate every field‑specific nuance. **Agentic Excerpt Tagging (AET)** bridges this gap: an offline large‑language‑model (LLM) pipeline extracts *excerpt‑tags*—short, transparent nuggets such as:

* “Surgical excision reduced tumour volume by 45 % in a murine model.”
* “Radiotherapy with cisplatin achieved a 20 % survival benefit.”
* “Meta‑analysis of 320 000 tweets reveals vaccine‑hesitancy trends.”

Each tag is embedded, compared, and pre‑computed into top‑k similarity lists. A lightweight Django portal offers two modes:

* **Baseline** – conventional filter search.
* **Experimental** – filter search **plus** excerpt‑tags and “You‑might‑also‑read” recommendations (k‑nearest‑neighbour pre‑processing).

> **No machine‑learning inference runs at query time.**

Over 24 months we will prototype in two domains (likely medical & environmental sciences), process ≥ 20 000 open‑access PDFs, refine prompts & models, and release all code, metadata, and evaluation artefacts under open licences.

### 2  Alignment with the Call (Criterion 1 — Scope, 10 %)
* **Aim fit** – enhances open‑science infrastructure through AI‑driven metadata enrichment.
* **Priority topics** – AI & data‑driven innovation for open science; user‑centred scholarly information systems.

### 3  Objectives & Success Metrics (Criterion 2 — Scientific Quality, 40 %)

| ID | Objective | Quantitative Target (headline metric **TBD** by researcher) |
|----|-----------|-------------------------------------------------------------|
| **O1** | Design an agentic LLM pipeline that extracts excerpt‑tags. | *Macro‑averaged F1 ≥ 0.80* on a 1 000‑record gold set, where a “match” is counted if ROUGE‑L ≥ 0.70 **or** cosine similarity of sentence embeddings ≥ 0.85 (evaluation protocol TBD). |
| **O2** | Apply pipeline to ≥ 20 000 PDFs in two domains; generate vectors & top‑k recommendations. | ≥ 90 % of tags judged “useful” (Likert ≥ 4) by two domain experts, κ ≥ 0.75 (assessment rubric TBD). |
| **O3** | Deploy a Django + NGINX search portal with baseline & experimental modes. | ≥ 95 % uptime; p95 latency < 150 ms (monitoring stack TBD). |
| **O4** | Conduct two iterative user studies (Year 1 & Year 2) with ≥ 200 participants each. | Headline metrics TBD (candidates: task‑completion time, nDCG@10, NASA‑TLX workload). |
| **O5** | Release code, enriched dataset, prompts & evaluation scripts under MIT/CC‑BY. | Zenodo DOI, GitHub repo, OSF preregistration published (impact tracking TBD). |

### 4  Methodology
#### 4.1  Agentic Excerpt Tagging  (Months 1–8)
* Local LLM actors **Extractor → Tagger → Verifier** running on open‑source models (e.g. Mixtral‑8×22B, Llama‑3‑8B‑Instruct, Phi‑3‑medium).
* Prompt‑ & model‑sweep on a 1 000‑record validation set; best configuration locked for production.

#### 4.2  Similarity & Recommendation (offline, Months 4–12)
* Embed each tag with a lightweight semantic model; store vectors in FAISS/Qdrant (final choice TBD).
* Pre‑compute top‑k neighbours; persist IDs & ranks in PostgreSQL.

#### 4.3  Runtime Delivery (Months 6–18)
* Containerised Django + NGINX + Postgres stack, suitable for cloud or on‑prem.
* Session toggle between baseline and experimental modes.

#### 4.4  Evaluation (Months 9–12 & 18–22)
* Controlled studies with interdisciplinary student cohorts (n ≥ 200 each).
* Metrics and questionnaires TBD, in consultation with the researcher and ethics board.

#### 4.5  Expansion & Dissemination (Months 16–24)
* Scale corpus to ≥ 20 000 PDFs with expert validation.
* Publish metadata, code, prompts, evaluation sets.
* Host virtual workshop and publish an adoption guide for libraries.

### 5  Work Packages, Milestones & Timeline
| WP | Months | Lead | Key Outputs | Milestone |
|----|--------|------|-------------|-----------|
| **WP1** | 1–4 | Felix | 1 000‑record gold set; system specification | **M1** – validated gold set |
| **WP2** | 3–8 | Adam | Dockerised LLM pipeline; evaluation report | **M2** – pipeline meets O1 target |
| **WP3** | 6–10 | Adam | Enriched metadata v1 (≥ 20 k PDFs) | **M3** – metadata release v1 |
| **WP4** | 7–14 | Adam | Public portal (baseline & experimental) | **M4** – portal online |
| **WP5** | 9–12 | Researcher | Dataset; analysis report; evaluation criteria | **M5** – Y1 user‑study complete |
| **WP6** | 12–18 | Adam | Enriched metadata v2; recommendations table | **M6** – recommendation engine pass |
| **WP7** | 18–22 | Researcher | Final analysis; journal submission | **M7** – manuscript submitted |
| **WP8** | 20–24 | Felix | Zenodo deposit; workshop; adoption guide | **M8** – project close‑out |

### 6  Team & Roles (Criterion 3 — Feasibility, 30 %)
| Role | Name | FTE | Responsibilities |
|------|------|-----|------------------|
| Data Solution Architect | **Adam** | 0.5 | Pipeline design, infra, fine‑tuning, validation, GDPR compliance |
| University Information Officer | **Felix** | 0.3 | Requirements, metadata curation, stakeholder liaison |
| Scientific Researcher | **TBD** | 0.5 | Study design, metrics definition (TBD), publications, ethics lead |
| CS Advisor (IR Professor) | **Prof. TBD** | 0.05 | Quarterly methods audit, gate approvals |
| Student Assistants | — | 0.3 | Annotation, recruitment, testing |

### 7  Budget (EUR, 24 months)
| Item | Amount | Rationale |
|------|--------|-----------|
| **Personnel** (1.65 FTE core + 0.05 FTE advisor) | 184 000 | Adam, Felix, Researcher, Students, Advisor |
| **GPU hardware** (on‑prem CapEx) | 20 000 | 2× A100 equivalents with 5‑year life |
| **Cloud compute** (OpEx) | 20 000 | Burst capacity for model sweeps |
| **Participant incentives** | 10 000 | ~ €25 × 400 participants |
| **Dissemination & workshops** | 6 000 | Venue, travel, materials |
| **Contingency & licences** | 10 000 | Reserve for inference pricing changes |
| **Total** | **250 000** | Matches call ceiling |

### 8  Risk & Mitigation
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| GPU supply constraints | Medium | Medium | Early reservation; hybrid on‑prem/cloud strategy |
| Accuracy variance across domains | Medium | High | Domain‑specific prompt tuning; human validation loop |
| Recruitment shortfall | Low | High | Multi‑faculty outreach; course‑credit incentives |
| Staff turnover | Medium | Medium | Containerised code; CS‑advisor back‑up |
| Model licensing changes | Medium | Medium | Maintain plan‑B open‑weights; licence audit at each release |

### 9  Expected Impact (Criterion 4 — 20 %)
* **Scientific** – first open, cross‑disciplinary benchmark for excerpt‑tagging; longitudinal evidence on LLM‑assisted discovery.
* **Societal** – faster, more transparent access to evidence; improved information literacy.
* **Library practice** – replicable, open‑source enhancement of discovery layers without live ML infrastructure.

> **KPI** – at least three external library pilots adopt the code within 12 months post‑project (tracking plan TBD).

---
**Contact:** Adam, Felix

