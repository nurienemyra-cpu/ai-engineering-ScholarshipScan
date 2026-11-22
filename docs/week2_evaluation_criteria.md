# Evaluation Criteria — ScholarshipScan

## Step B1 — Evaluation Criteria Table

| Objective / Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User / Business Outcome |
|---|---|---|---|---|---|---|
| O2 / NFR2, FR2 | Field-Level Accuracy | Domain-specific capability | Compare extracted scholarship information against ground-truth values for at least 8 scholarship records covering 5 required fields each. Compare the results with a regex baseline. | Precision, Recall and F1-score | F1 ≥ 0.80 overall and higher than the regex baseline | Students receive accurate scholarship information, including citizenship, age limits, minimum results, deadlines and required documents. |
| O1 / FR5 | JSON Schema Validity | Instruction-following capability | Validate the extracted scholarship information against the predefined JSON schema across the evaluation set. | Schema validity rate (%) | ≥ 95% | Students receive consistently structured scholarship information that can be compared easily. |
| O1 / FR1, NFR5, RAI6 | Document Addition and Metadata Completeness | Data management capability | Add new scholarship documents and inspect whether the required metadata (title, sponsor, URL, intake year and download date) is recorded correctly. | Document addition time; metadata completeness rate | < 15 minutes per document; 100% metadata completeness | New scholarship documents can be added and maintained without rebuilding unrelated system components. |
| O2 / NFR3, FR2 | Retrieval Relevance | Domain-specific capability | For each question or extraction task, check whether the top 5 retrieved passages include the reference source location for the required scholarship field. | Hit rate @ top 5 passages | ≥ 85% | The system retrieves relevant information from scholarship documents, improving the reliability of extracted values. |
| O3 / RAI1, FR2 | Faithfulness | Generation capability | Manually review at least 40 extracted values and check whether each value is supported by its corresponding retrieved source passage. | Faithfulness rate (% of extracted values supported by evidence) | ≥ 90% | Students are protected from incorrect or invented scholarship information that is not supported by the original documents. |
| O3 / FR3, RAI2 | Source Citation and Support | Instruction-following capability | Inspect all extracted fields to ensure each value includes a source reference, then verify whether the cited page or section supports the extracted information. | Citation presence rate; citation–source match rate | 100% presence; ≥ 95% match | Students can verify scholarship requirements and deadlines using the original source documents. |
| O2 / FR6, RAI3 | "Not Stated" Handling | Instruction-following capability | Test fields that are absent from the scholarship documents and check whether the system correctly returns "not stated". Also test fields containing available information to identify false abstentions. | Correct "not stated" rate; false "not stated" rate | ≥ 90%; ≤ 15% | Students are informed when scholarship information is unavailable instead of receiving guessed or misleading information. |
| O3 / FR7, RAI4 | Out-of-Scope Refusal | Instruction-following capability | Run a test set of at least 20 out-of-scope queries, including eligibility decisions, scholarship application advice and scholarships outside the document collection. | Refusal-and-redirect success rate | ≥ 95% | The system remains focused on comparing scholarship information rather than providing unsupported eligibility decisions or unrelated advice. |
| O3 / FR4, NFR6 | Table Clarity | Generation capability | Ask 3–5 student testers to evaluate the readability and clarity of scholarship comparison tables using a 5-point rating rubric. | Mean clarity score (1–5) | ≥ 4.0 | Students can easily understand and compare scholarship requirements across different sponsors. |
| O3 / NFR1 | Extraction Latency | Cost and latency | Measure the end-to-end processing time required to extract scholarship information from a document over the evaluation workload. | p90 extraction latency | < 60 seconds per document | Students receive scholarship comparisons within a reasonable processing time. |
| O3 / NFR4 | Inference Cost | Cost and latency | Record token usage and API costs when processing scholarship documents throughout the evaluation workload. | Cost per document | < US$0.05 per document | The prototype can be operated and evaluated within the available project budget. |
| O3 / RAI7 | Personal Data Protection | Responsible AI capability | Audit evaluation logs to ensure that no personal identifiers are stored beyond the user session. | Number of personal identifiers found in persistent logs | 0 found | Students' personal information is protected during system evaluation and operation. |

## Shared Evaluation Datasets

| Dataset | Size | Used for |
|---|---|---|
| Scholarship documents with ground-truth labels for the five required fields | ≥ 8 scholarship records, with 5 fields each | Field-level accuracy, precision, recall, F1-score and comparison with the regex baseline |
| Labelled document passages with reference source locations | Based on the scholarship evaluation documents | Retrieval relevance |
| Extracted scholarship values with their supporting passages | Approximately 40 or more extracted values | Faithfulness and source citation |
| Fields with information absent from the documents | Test cases covering missing scholarship information | "Not stated" handling and false abstention |
| Out-of-scope queries | ≥ 20 | Out-of-scope refusal |
| Scholarship comparison tables | Evaluated by 3–5 student testers | Table clarity |
| Document processing workload | Selected scholarship PDF and web documents | Extraction latency, inference cost and document addition time |

The evaluation datasets will consist of selected public scholarship documents in PDF and web page formats. Each document will be labelled with the five required fields: citizenship, age limit, minimum results, deadline and required documents.

The same scholarship documents will be reused across the evaluation process to ensure consistent and comparable results. A regex-based extraction approach will also be used as a baseline to determine whether the proposed system improves field-level extraction accuracy.

## Optional Rubric — Table Clarity (1–5)

| Score | Description |
|---|---|
| 5 | Very clear and easy to compare; all scholarship information is accurately presented with consistent formatting. |
| 4 | Mostly clear and readable; minor formatting or wording issues do not affect understanding. |
| 3 | Understandable but requires some effort to compare scholarship requirements. |
| 2 | Difficult to read or compare; some information is confusing or poorly organised. |
| 1 | Unclear, poorly structured or difficult to understand. |

## Reflection

**Which evaluation criterion is most important for determining whether ScholarshipScan will be useful to its intended users?**

Faithfulness (RAI1) is the most important evaluation criterion. ScholarshipScan is designed to help students compare scholarship requirements by extracting information directly from selected scholarship documents. Therefore, the extracted information must be supported by the original sources rather than generated based on assumptions or general knowledge. If the system provides incorrect deadlines, eligibility requirements or minimum academic results, students may make decisions based on misleading information.

Faithfulness also supports the other evaluation criteria. Accurate extraction alone does not guarantee that the information is supported by the original document. Similarly, source citations are only useful when the cited passages actually support the extracted values. Retrieval relevance and evidence sufficiency are also important because the system needs to find the correct information and identify when a particular field is not stated. Although processing time, cost and table clarity affect the user experience, these factors are only valuable when students can trust the extracted scholarship information.
