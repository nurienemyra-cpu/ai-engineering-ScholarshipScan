# ScholarshipScan: Automatic Extraction and Comparison of Eligibility Criteria from Malaysian Scholarship Documents
AI Engineering — Topic 1 
 
> ScholarshipScan extracts eligibility criteria, deadlines and required documents from public scholarship documents into a structured, source-linked comparison table, for Malaysian students who are choosing where to apply.
 
## Problem Statement
Scholarship information is scattered across many sponsor websites and PDFs, and each sponsor writes its criteria in a different format and language (English or Malay). Students must read and compare these documents by hand, and may miss deadlines or apply to schemes they do not qualify for. Because the criteria are free text, often conditional ("and", "or", "except"), a keyword search or fixed rules cannot capture them reliably, which is why an AI-based extraction pipeline with measurable evaluation is a suitable engineering solution.
 
## Evidence of the Problem
- Times Higher Education notes that finding the right scholarship can be difficult because there are so many choices.
  Source: Times Higher Education (2022). *Scholarships for Malaysian students to study abroad.* https://www.timeshighereducation.com/student/advice/scholarships-malaysian-students-study-abroad
- A Malaysian study points out that students sometimes apply for financial aid only to be rejected because they did not meet the requirements.
  Source: Sulaiman, M. S., et al. (2024). *EduFinBuddy: A Web-based Education Financial Aid Recommendation System using Web Scraping.* JICTIE, UPSI. https://ejournal.upsi.edu.my/index.php/JICTIE/article/download/9280/5321/51225
## Initial Related Work / Existing Approaches
- **EduFinBuddy** is a web-based system that scrapes financial aid listings and matches them to student input using cosine similarity.
  Source: Sulaiman et al. (2024), link above.
  Key limitation: it matches on a small, fixed set of requirement fields (the paper mentions six), so it does not appear to handle free-text or conditional criteria written in varied styles.
- **EduScholar AI** is a recommendation platform that compares student data with scholarship schemes using machine learning, while administrators manage scholarship records and update eligibility criteria.
  Source: IJERT, paper ID IJERTV15IS070495. https://www.ijert.org/research/eduscholar-ai-ai-powered-scholarship-recommendation-system-IJERTV15IS070495.pdf
  Key limitation: eligibility criteria are maintained by administrators, so every new scholarship document needs manual data entry.
- **Fuzzy TOPSIS decision model** assesses the eligibility of scholarship recipients to support decision makers.
  Source: arXiv:1306.6489. https://arxiv.org/pdf/1306.6489
  Key limitation: it supports the committee's side (ranking applicants from already-structured scores) and does not read scholarship documents. It is also an older approach (2013).
**Engineering gap:** in the systems above, eligibility criteria are already structured (entered by administrators or scraped into a few predefined fields). They do not reliably extract criteria from heterogeneous, free-text, bilingual documents, and they do not provide traceable links back to the source sentence.
 
## Project Objectives
- O1: To extract five key fields (citizenship, age limit, minimum results, deadline, required documents) from at least 8 scholarship records in 3 public sources into a structured JSON schema.
- O2: To evaluate the extraction against a manually labelled ground truth (precision, recall and F1 per field) and compare it with a regex baseline.
- O3: To produce a source-linked comparison table and measure faithfulness, meaning how often extracted values actually appear in the source text.
## Target Users
Post-SPM, STPM and matriculation students in Malaysia, and their parents or school counsellors, who need to compare scholarships.
 
## Data Sources
- Source 1: MEXT Scholarship 2027 (KOSEN) guidelines, Embassy of Japan in Malaysia — https://www.my.emb-japan.go.jp/files/100990158.pdf
- Source 2: MEXT Scholarship 2027 (Undergraduate) guidelines, Embassy of Japan in Malaysia — https://www.my.emb-japan.go.jp/files/100990174.pdf
- Source 3 (to add): official scholarship page from a Malaysian sponsor (e.g. JPA or MARA) — URL: TODO (check terms of use first)
> Data handling: store the URL, download date and intake year for every document. Where redistribution rights are unclear, keep only URLs and the download script in this repository instead of the document text.
 
## Responsible AI Risks
- **Incorrect extraction (hallucination).** A wrong deadline or criterion could make a student miss an opportunity. Mitigation: every field links to its source sentence and a "last checked" date, a faithfulness check measures unsupported values, and the interface tells users to verify with the sponsor before applying.
- **Outdated information.** Scholarships change every year. Mitigation: store the intake year and download date, and flag documents older than one cycle.
- **Coverage bias.** If the dataset covers only large or English-language sponsors, students applying to smaller or Malay-only schemes are underserved. Mitigation: include Malay documents and report which sponsors are covered.
- **Copyright and terms of use.** Mitigation: check each site's terms and store URLs and scripts instead of full text where redistribution is unclear.
## Weekly Progress
| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + docs | ✅ |
| W2 | Data collection + field schema | Source table (3 sources), JSON schema (5 fields) | ⬜ |
| W3 | Ground-truth labelling | First 3 records labelled (target: 8 or more by W11) | ⬜ |
| W4–W6 | Baseline | Download/cleaning pipeline, regex baseline | ⬜ |
| W7–W10 | LLM extraction | JSON extraction pipeline, retrieval of relevant sections | ⬜ |
| W11–W13 | Evaluation + interface | F1 per field, faithfulness, error analysis, comparison table UI | ⬜ |
| W14 | Final | Report + demo | ⬜ |
 
## Repository Structure (planned)
```
scholarshipscan/
├── README.md
├── data/
│   ├── raw/                 # URLs and download scripts
│   ├── processed/
│   └── labels/ground_truth.csv
├── src/
│   ├── scraping/
│   ├── baseline/
│   ├── extraction/
│   └── eval/
├── app/
├── docs/
└── requirements.txt
```
 
## Week 3: Data Engineering – Pipelines, Lineage, and Workflows

This week focuses on developing the initial data preparation pipeline for ScholarshipScan using publicly available scholarship PDF documents.

### Selected Data Sources

Three scholarship documents were selected:
- Malaysia International Scholarship (MIS) 2026
- Malaysian Technical Cooperation Programme (MTCP) Scholarship 2026
- MyBrainSc Scholarship FAQ 2026

### Data Processing Workflow

The pipeline consists of the following stages:

1. **Source:** Collect scholarship PDF documents.
2. **Ingest:** Load the PDF files into the project.
3. **Inspect:** Check page counts and extracted text availability.
4. **Prepare / Transform:** Extract text using PyPDF, clean whitespace, and apply paragraph-aware chunking with a maximum chunk size of 500 characters.
5. **Validate Output:** Verify that the prepared JSONL files are not empty and that chunk sizes meet the configured limit.
6. **Record Lineage:** Generate SHA-256 fingerprints and pipeline manifests to track source files, processing configurations, and outputs.

### Project Structure

```text
week3_project/
├── data/
│   ├── raw/
│   ├── processed/
│   │   ├── prepared_text.jsonl
│   │   └── chunking_comparison/
│   ├── week3_manifest.json
│   └── pipeline_v1/
│       ├── documents_mis/
│       ├── documents_mtcp/
│       └── documents_mybrain/
```

### Pipeline Outputs

- Extracted and prepared scholarship text in JSONL format.
- Fixed-size and paragraph-aware chunking comparison.
- Data lineage manifest.
- Individual pipeline manifests for each scholarship document.

### Limitations

The current pipeline focuses on PDF text extraction. Complex tables, multi-column layouts, and scanned documents may require additional processing or OCR. Source version and access rights also require verification.

| Task | Status | Evidence / Required Action |
|---|---|---|
| Validation Rule 1 and Rule 2 | Completed – PASS | All 84 records passed both validation rules, with zero failed records. |
| Duplicate and Short Chunk Inspection | Completed | Identified five page-number chunks and one repeated-heading group containing three occurrences. |
| Targeted Manual Text Verification | Completed | Verified the five MIS page-number chunks and three repeated MyBrainSc headings against the original PDF pages. |
| Copyright and Usage Permissions | Pending | Review copyright notices, terms of use, and permissions of source documents. |
