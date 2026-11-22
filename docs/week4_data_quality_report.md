# Individual Data Quality Audit Report

## 1. Project Context

**Project Name:** ScholarshipScan – AI-Based Scholarship Information Extraction and Comparison System

ScholarshipScan aims to help students retrieve, understand, and compare scholarship information from official documents. The system extracts important information such as citizenship requirements, age limits, minimum academic results, application deadlines, and required documents.

This data quality audit evaluates the scholarship PDF documents collected for the Week 3 data engineering pipeline. The audit focuses on whether the extracted data is suitable for further processing and retrieval.

Two relevant functional requirements are:

- **FR2:** Extract important scholarship information, including citizenship, age limits, minimum academic results, deadlines, and required documents.
- **FR3:** Provide source references and supporting passages for extracted information.

## 2. Dataset Overview

Three scholarship-related PDF documents were used in the pipeline.

| Document | Pages | Extracted Characters |
|---|---:|---:|
| MIS 2026 Guidelines (`mis.pdf`) | 7 | 9,006 |
| MTCP 2026 Guidelines (`mtcp.pdf`) | 4 | 7,459 |
| MyBrainSc 2026 FAQ (`mybrain.pdf`) | 10 | 10,559 |
| **Total** | **21** | **27,024** |

The documents were processed using PDF text extraction and whitespace cleaning. The extracted content was divided into chunks using fixed-size and paragraph-aware chunking methods.

## 3. Data Quality Assessment

### 3.1 Completeness

All three PDF documents produced extractable text, with a total of 27,024 extracted characters. This indicates that the documents contain machine-readable text that can be processed by the pipeline.

However, successful text extraction does not guarantee that all required scholarship fields are available. Field-level completeness for citizenship, age limits, academic requirements, deadlines, and required documents has not yet been fully evaluated.

**Assessment:** Partially satisfactory. The documents are extractable, but completeness of individual scholarship fields requires further verification.

### 3.2 Consistency

The documents were processed using the same extraction and preparation workflow. Source filenames, page references, and chunk identifiers were retained to support traceability.

Two chunking approaches were compared:

| Measurement | Fixed-size | Paragraph-aware |
|---|---:|---:|
| Total chunks | 63 | 84 |
| Average chunk length | 421.84 | 312.67 |
| Shortest chunk | 60 | 1 |
| Longest chunk | 500 | 500 |
| Chunks above 500 characters | 0 | 0 |
| Empty chunks | 0 | 0 |
| Exact duplicate occurrences | 0 | 2 |

Paragraph-aware chunking produced 21 more chunks and a lower average chunk length. It may preserve paragraph context better, but the short chunks and repeated headings require attention.

**Assessment:** Satisfactory for basic structural consistency, with minor issues identified in paragraph-aware chunking.

### 3.3 Accuracy

A targeted manual verification was performed by comparing suspicious extracted chunks against the original PDF pages.

The inspection confirmed:

- Five short chunks from the MIS document (`1`, `3`, `4`, `5`, and `6`) were page numbers rather than meaningful scholarship content.
- The MyBrainSc heading, `SOALAN LAZIM BIASISWA MYBRAINSC TAHUN 2026`, appeared repeatedly on pages 2, 3, and 4.

These findings indicate that the extraction process captured some page numbers and repeated headings as separate chunks.

However, this verification only covered the eight identified suspicious records. The accuracy of all extracted scholarship information, including eligibility criteria and deadlines, has not yet been fully checked against the original PDFs.

**Assessment:** Partially satisfactory. Targeted verification was completed, but broader content accuracy checks are still required.

### 3.4 Timeliness

All three documents relate to scholarship information for the 2026 academic year. Their information is relevant to the intended project dataset.

However, the current validity of the information and whether newer versions have been published have not yet been independently confirmed.

**Assessment:** Partially satisfactory. The documents are labelled for the 2026 intake, but their latest-version status needs verification.

### 3.5 Uniqueness

The chunking comparison identified different duplicate patterns:

- Fixed-size chunking contained zero exact duplicate chunks.
- Paragraph-aware chunking contained one exact duplicate group with three occurrences of the same MyBrainSc heading. This represents two additional duplicate occurrences.
- Five short MIS chunks were identified as page numbers.

The repeated heading is likely caused by the document layout rather than duplicated scholarship information.

These records should be considered when preparing the retrieval-ready dataset. However, the original extracted records should be retained to preserve data provenance.

**Assessment:** Minor duplication and irrelevant chunks were identified. They can be addressed during retrieval data preparation.

## 4. Data Validation Rules

Two validation rules were applied to `prepared_text.jsonl`.

| Validation Rule | Description | Result |
|---|---|---|
| Rule 1: Source Reference and Non-empty Text | Every record must contain a source reference and non-empty text. | PASS |
| Rule 2: Maximum Chunk Length | Every chunk must contain no more than 500 characters. | PASS |

**Validation results:**

- Total records checked: 84
- Rule 1 failed records: 0
- Rule 2 failed records: 0
- Both validation rules passed.

These results demonstrate that all 84 records passed the two basic structural validation checks. However, these checks do not establish the semantic accuracy or completeness of the extracted scholarship information.

## 5. Data Coverage and Potential Bias

The dataset covers three scholarship programmes: MIS, MTCP, and MyBrainSc. The documents contain scholarship-related information such as eligibility requirements, application guidelines, and frequently asked questions.

However, the dataset is limited to three programmes and does not represent all scholarships available to students.

Potential limitations and biases include:

- **Programme coverage bias:** The dataset contains only three selected scholarship programmes.
- **Document format bias:** The extraction process may perform differently depending on PDF layouts, tables, and formatting.
- **Information availability bias:** Some scholarship criteria may not be explicitly stated in the collected documents.
- **Temporal bias:** Scholarship requirements may change between intake years.

Therefore, the system should avoid making assumptions when information is missing and should return "not stated" when a required field cannot be supported by the source document.

## 6. Responsible Data Practices

The following practices are important for ScholarshipScan:

1. **Data provenance:** Retain source filenames, page references, source URLs, and download dates to support traceability.
2. **Source verification:** Ensure that extracted information can be checked against the original scholarship documents.
3. **Copyright and usage permissions:** Public accessibility does not automatically mean that documents can be freely redistributed. Their copyright notices and usage terms must be reviewed.
4. **Privacy protection:** Avoid storing unnecessary personal information. The system should not retain applicants' personal identifiers beyond the intended session.
5. **Transparency:** Inform users that extracted scholarship information should be verified against the original sponsor documents.
6. **Missing information handling:** Return "not stated" rather than generating unsupported scholarship requirements.

## 7. Engineering Decision

**Decision: GO WITH CONDITIONS**

The dataset is suitable for continuing the development of the ScholarshipScan pipeline because all three documents produced extractable text, and all 84 prepared records passed the two structural validation rules.

Targeted manual verification also identified the causes of the suspicious short chunks and repeated headings.

However, the dataset is not yet fully ready for reliable scholarship information retrieval because field-level accuracy, document version validity, and copyright or usage permissions still require further checks.

### Priority Improvements

**1. Improve retrieval-ready data quality**

Exclude irrelevant page-number chunks and repeated document headings from the retrieval-ready dataset after verification. Preserve the original prepared records to maintain provenance and traceability.

**2. Perform broader content and source verification**

Manually verify important scholarship fields, especially eligibility criteria, academic requirements, deadlines, and required documents. Confirm that the collected PDFs are the appropriate and latest available versions, and review their usage permissions.

## 8. Conclusion

The audit shows that the current ScholarshipScan dataset is structurally suitable for further pipeline development. Both validation rules passed for all 84 records, while chunk inspection and targeted manual verification identified page numbers and repeated headings that may affect retrieval quality.

Nevertheless, further content accuracy checks, source verification, and copyright reviews are necessary before the dataset can be considered reliable for the final scholarship information extraction and comparison system.

Therefore, the recommended engineering decision is **GO WITH CONDITIONS**.
