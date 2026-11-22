# Individual Data Quality Audit Report

## A. Project Context

**Project Title:** ScholarshipScan – AI-Based Scholarship Information Extraction and Comparison System

**Project Purpose:**

ScholarshipScan is an AI application designed to help students retrieve, understand, and compare scholarship information from official scholarship documents. It extracts important criteria and provides supporting source references so that users can verify the information against the original documents.

**Application Requirements:**

1. **FR2 – Scholarship Information Extraction:** Extract citizenship requirements, age limits, minimum academic results, application deadlines, and required documents.
2. **FR3 – Source Reference and Evidence:** Provide a source reference and supporting passage for every extracted field.

**Data Sources and Types:**

The current dataset consists of three publicly accessible scholarship PDF documents:

| Document | Pages | Extracted Characters |
|---|---:|---:|
| MIS 2026 Guidelines | 7 | 9,006 |
| MTCP Scholarship 2026 Guidelines | 4 | 7,459 |
| MyBrainSc Scholarship FAQ 2026 | 10 | 10,559 |
| **Total** | **21** | **27,024** |

The data consists of unstructured textual information extracted from PDF documents using PyPDF. The text is cleaned, divided into paragraph-aware chunks with a maximum configured size of 500 characters, and stored in JSONL format.

**Sample Inspected:**

All three available PDF documents were included in the initial document-level inspection. A further detailed inspection of 10–20 extracted text chunks is required to evaluate field-level extraction quality.

## B. Data Quality Assessment

| Dimension | Finding and Evidence | Impact |
|---|---|---|
| Completeness | All three PDFs produced extractable text. However, the presence of extracted text does not confirm that all required scholarship fields are available or correctly extracted. | Missing criteria may produce incomplete scholarship comparisons. |
| Consistency | The documents describe different scholarship programmes and formats. Cross-document consistency cannot be fully established without comparing equivalent fields. | Differences in date and eligibility representations may affect comparisons. |
| Accuracy | Text extraction was successful, but extracted passages have not yet been fully compared with their original PDF pages. | Extraction errors may produce incorrect scholarship information. |
| Timeliness | All selected documents relate to the 2026 scholarship cycle. Their current validity and application deadlines still require verification with the providers. | Outdated information may mislead students. |
| Uniqueness | Three different scholarship documents were selected. Exact duplicate chunks have not yet been systematically checked. | Duplicate content may affect retrieval quality. |

## C. Two Validation Rules

**Rule 1: Source Reference and Non-empty Text**

- **Problem detected:** Missing source information or empty extracted text.
- **Condition:** Every chunk must contain a source filename and non-empty text.
- **Validation result:** Pending execution of the Python validation script.
- **Action if failed:** Reprocess the affected PDF and verify its source metadata. Do not automatically delete the record.

**Rule 2: Maximum Chunk Length**

- **Problem detected:** Chunks exceeding the configured processing limit.
- **Condition:** Every chunk must contain no more than 500 characters.
- **Validation result:** Pending execution of the Python validation script.
- **Action if failed:** Reprocess the affected text using the paragraph-aware chunking method and validate it again.

A failed validation should require human review because the affected data may contain important scholarship information. For example, an empty extracted page may be caused by a scanned PDF rather than irrelevant content. Reprocessing or OCR may recover the information without losing valuable data.

## D. Coverage and Potential Bias

**Coverage Findings:**

Two important categories for ScholarshipScan are:

1. **Eligibility Requirements:** Citizenship, age limits, and minimum academic results.
2. **Application Information:** Application deadlines and required supporting documents.

The three selected documents provide an initial basis for examining these categories. However, the dataset covers only three scholarship programmes and cannot yet represent all scholarship providers, study levels, or application situations.

**Potential Bias and Its Effect:**

A potential risk is selection bias because the dataset contains only three selected scholarship programmes. This may limit the system's ability to compare scholarships outside the selected collection.

Pipeline-induced bias is also possible because PDF extraction may handle ordinary paragraphs more effectively than complex tables, multi-column layouts, or scanned pages.

These issues could cause ScholarshipScan to produce more complete results for clearly structured documents than for documents containing complex layouts. Further investigation should include additional scholarship sources and manual comparisons between extracted chunks and original PDF pages.

## E. Responsible Data Practices

The project uses publicly accessible scholarship documents published by the relevant scholarship providers. Source URLs, filenames, page references, and SHA-256 fingerprints are recorded to maintain traceability and provenance.

The documents mainly contain general scholarship information rather than personal student application records. Nevertheless, they should be checked for accidentally included personal information.

Public accessibility does not automatically grant unrestricted reuse or redistribution rights. Copyright notices, source terms, and applicable permissions must be checked before redistributing the documents.

Safeguards include preserving original PDFs, recording source information and download dates, maintaining document fingerprints, checking for personal information, and verifying extracted information against the original provider documents.

**Data Governance Statement:**

ScholarshipScan maintains data provenance through source references, page information, download records, and SHA-256 fingerprints. The project minimises personal data collection and requires verification of document usage permissions. Extracted information must remain traceable to its original sources to support responsible and reliable AI development.

## F. Engineering Decision

**Selected Decision: GO WITH CONDITIONS**

The current dataset is suitable for continuing the next development stage with additional validation and improvements.

The initial inspection confirmed that all three scholarship PDFs produced extractable text, with a combined total of 27,024 extracted characters across 21 pages. The documents can therefore support the initial development of the document processing and information extraction components.

However, field-level extraction accuracy has not been fully verified. The current dataset also has limited scholarship coverage, while document validity and reuse permissions require further investigation.

**Two Priority Improvements:**

1. **Strengthen Data Quality Validation:** Compare selected extracted text chunks with their original PDF pages. Verify important fields such as citizenship, academic requirements, deadlines, and required documents. Apply OCR or improve extraction where necessary.

2. **Improve Data Coverage and Provenance:** Include additional scholarship documents from different providers and study levels. Verify publication dates, source URLs, document versions, and usage permissions. Maintain source references and SHA-256 fingerprints for each document.

**Final Justification:**

Proceeding with conditions allows ScholarshipScan development to continue while recognising the limitations of the current dataset. The proposed improvements should be completed before relying on the extracted information for final scholarship comparisons.
