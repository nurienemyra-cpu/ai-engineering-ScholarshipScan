# ScholarshipScan — Week 2 Requirements Specification

## Section 0 — Project Framing & Application Scope Checkpoint

| **Framing Element** | **ScholarshipScan** |
|---|---|
| **Problem Evidence** | Students often need to review multiple scholarship documents, including PDF documents and web pages, to identify important eligibility information. These documents may present information differently, making it time-consuming to compare scholarship requirements. Important details such as citizenship requirements, age limits, minimum academic results, application deadlines and required documents may be difficult to locate quickly. AI-based information extraction can help organise these details, but extracted information must be supported by the original scholarship sources to avoid misleading students. |
| **Existing Approaches** | **1. Manual document review:** Students read scholarship documents and manually identify eligibility requirements and application details. **2. Keyword-based or rule-based extraction:** Uses predefined keywords and patterns to locate information, but may struggle with different document structures and wording. **3. General-purpose AI assistants:** Can summarise and extract information from documents, but may generate unsupported information or fail to provide reliable source references. |
| **Project / Engineering Gap** | Existing approaches can make scholarship information retrieval difficult because students must manually compare multiple documents or rely on automated systems that may produce unsupported information. The remaining engineering gap is the lack of a structured scholarship information extraction system that retrieves important eligibility fields, provides supporting passages and source references, standardises extracted information into a comparable format, and identifies information that is not stated in the source documents. This is a practical engineering gap rather than a claim of scientific novelty. |
| **Objective O1** | To design a scholarship information extraction system that processes and organises publicly accessible scholarship documents. |
| **Objective O2** | To develop an AI-based information extraction system that identifies five core scholarship fields using document retrieval and language model processing. |
| **Objective O3** | To evaluate the system based on extraction accuracy, retrieval relevance, groundedness, source verification, response time and responsible AI requirements. |
| **Intended Users** | University students who need to identify, review and compare scholarship eligibility requirements and application information. |
| **Supported Tasks** | (1) Add publicly accessible scholarship PDF documents and web pages. (2) Record scholarship metadata, including title, sponsor, URL, intake year and download date. (3) Extract five core fields: citizenship, age limit, minimum results, application deadline and required documents. (4) Display supporting passages and source references for extracted information. (5) Present scholarship information in a clear and comparable table. (6) Convert extracted information into a fixed JSON schema. (7) Indicate "not stated" when information is unavailable in the source document. |
| **Information / Knowledge Domain** | Publicly accessible scholarship information, including scholarship PDF documents and web pages. The system focuses on five core fields: citizenship, age limit, minimum results, application deadline and required documents. Only information from the collected and registered scholarship sources is within scope. |
| **Operating Context** | A scholarship information extraction application used by students to review and compare information from registered scholarship documents. The system acts as an information retrieval and organisation tool rather than an official scholarship eligibility decision platform. |
| **Important Constraints** | **Data:** Scholarship documents must be publicly accessible and their source information must be recorded. **Time:** The system must be developed and evaluated within the project schedule. **Budget:** Model and API usage should remain within the allocated project budget. **Technical:** The system depends on document processing, information retrieval and language model extraction. PDF parsing may have limitations when handling complex layouts, tables and scanned documents. Extraction quality also depends on the availability and clarity of information in the source documents. |
| **Explicit Out-of-Scope Tasks** | Making final scholarship eligibility decisions for students. Providing personalised scholarship application advice. Searching for scholarships outside the registered collection. Completing scholarship applications on behalf of students. Guessing missing eligibility requirements or application details. Storing personal identifiers beyond the active session. |
| **Evaluation Focus** | Does ScholarshipScan accurately extract the five required scholarship fields, retrieve relevant supporting passages, provide verifiable source references, organise information into a consistent format and indicate "not stated" when evidence is unavailable? The system will also be evaluated against a regex-based baseline, performance thresholds and responsible AI requirements. |

---

### Traceability Check (Objectives → Scope → Evaluation)

| **Objective** | **Scope Decisions It Drives** | **Evaluation Focus** | **Requirements** |
|---|---|---|---|
| **O1** — Design a scholarship information extraction system | Scholarship document collection, document processing, metadata registration and structured output | Document addition time, metadata completeness and JSON schema validity | FR1, FR5; NFR5; RAI6 |
| **O2** — Develop an AI-based information extraction system | Five core scholarship fields, document retrieval, supporting passages and evidence sufficiency checking | Field-level extraction accuracy, retrieval relevance and correct handling of missing information | FR2, FR3, FR6; NFR2, NFR3; RAI1, RAI2, RAI3 |
| **O3** — Evaluate accuracy, groundedness and system performance | Evaluation dataset, regex baseline, source verification, output comparison and scope control | Extraction accuracy, faithfulness, citation correctness, table clarity, refusal performance, latency and cost | FR3, FR4, FR7; NFR1, NFR2, NFR4, NFR6; RAI1–RAI7 |

### Notes to Resolve Before Implementation

- Confirm the availability and accessibility of the scholarship PDF documents and web pages to be included in the system.
- Prepare a ground-truth dataset containing the correct values for all five required scholarship fields.
- Ensure that each ground-truth value has a corresponding source passage and document reference.
- Identify scholarship documents containing fields that are not explicitly stated to evaluate the system's abstention capability.
- Confirm the language model and document retrieval tools that will be used while keeping API costs within the project budget.

---

## Section 1 — Functional Requirements

# Requirements Specification — ScholarshipScan

| **#** | **Requirement — The System SHALL** | **Scope Boundary — SHALL NOT (where relevant)** |
|---|---|---|
| FR1 | Accept publicly accessible scholarship PDF documents and web pages and record their metadata, including title, sponsor, URL, intake year and download date. | Process documents that have not been selected or registered as scholarship sources. |
| FR2 | Extract five core scholarship fields: citizenship, age limit, minimum results, application deadline and required documents. | Invent or assume scholarship requirements that are not supported by the source documents. |
| FR3 | Provide a source reference and supporting passage for every extracted field so that users can verify the information against the original scholarship document. | Provide references or supporting passages that do not correspond to the extracted information. |
| FR4 | Present extracted scholarship information in a clear and comparable table to help students review different scholarship opportunities. | Present information in a misleading or inconsistent format that makes scholarship comparison difficult. |
| FR5 | Normalise extracted scholarship information into a predefined and consistent JSON schema. | Return extracted information in an inconsistent structure that does not follow the required schema. |
| FR6 | Return "not stated" when a required scholarship field is not explicitly available or supported by the source document. | Guess, infer or generate missing scholarship requirements without supporting evidence. |
| FR7 | Decline requests outside the defined scholarship information extraction scope, including final eligibility decisions, personalised application advice and information about scholarships outside the registered collection. | Make final scholarship eligibility decisions or provide unsupported scholarship information beyond the collected sources. |

---

## Section 2 — Non-Functional Requirements

| **#** | **Quality Attribute** | **Requirement** | **Measurement Method** | **Metric** | **Threshold** |
|---|---|---|---|---|---|
| NFR1 | Performance | The system shall process scholarship documents and return extracted information within an acceptable response time. | Measure end-to-end document processing latency using the evaluation workload. | p90 latency per document | < 60 seconds |
| NFR2 | Accuracy | The system shall accurately extract the five required scholarship fields and perform better than a regex-based baseline. | Evaluate field-level precision, recall and F1-score using at least 8 scholarship records with ground-truth labels for all five fields. | Overall field-level F1-score | ≥ 0.80 and higher than the regex baseline |
| NFR3 | Retrieval Relevance | The retrieved passages shall contain the information required to extract the five scholarship fields. | Compare the retrieved top-five passages against labelled supporting passages in the evaluation dataset. | Retrieval hit rate @ top 5 | ≥ 85% |
| NFR4 | Cost | Model and API usage shall remain within the project budget. | Record model and API usage during document processing and calculate the cost per document. | Cost per document | < US$0.05 |
| NFR5 | Maintainability | A new scholarship document shall be added and processed without rebuilding unrelated system components. | Perform a controlled document addition test and measure the time required to make the document available. | Time per document addition | < 15 minutes |
| NFR6 | Usability | The extracted scholarship information shall be presented clearly and consistently so that students can compare scholarship requirements. | Collect feedback from 3–5 student testers using a short evaluation questionnaire. | Mean table clarity rating (1–5) | ≥ 4.0 |

---

## Section 3 — Responsible AI Requirements

| **#** | **Responsible AI Risk** | **Requirement** | **Measurement Method** | **Metric** | **Threshold** |
|---|---|---|---|---|---|
| RAI1 | Hallucination / Groundedness | Every extracted scholarship value shall be supported by its corresponding retrieved passage. | Manually review at least 40 extracted values and compare each value against its supporting passage. | Groundedness rate (% of extracted values supported by evidence) | ≥ 90% |
| RAI2 | Transparency / Verifiability | The system shall provide a verifiable source reference and supporting passage for every extracted field. | Check the extracted fields for source reference availability and verify whether the references match the supporting information. | Citation presence rate; citation–source match rate | 100% presence; ≥ 95% match |
| RAI3 | Unsupported Information | The system shall return "not stated" when the source document does not provide sufficient evidence for a required field. | Test scholarship documents containing missing or unavailable fields and compare the system output against the ground-truth labels. | Correct "not stated" rate; false "not stated" rate | ≥ 90%; ≤ 15% |
| RAI4 | Scope / Misuse | The system shall decline requests outside the defined scholarship information extraction scope and redirect users appropriately. | Test at least 20 out-of-scope requests, including eligibility decisions and personalised application advice. | Out-of-scope refusal success rate | ≥ 95% |
| RAI5 | Over-reliance on AI | The system shall remind students to verify extracted scholarship information against the sponsor's original document. | Inspect system responses and extracted scholarship information for a verification reminder directing students to the original source. | Responses containing a verification reminder (%) | 100% |
| RAI6 | Incorrect or Outdated Source Information | The system shall record document provenance and version-related information, including the scholarship title, sponsor, source URL, intake year and download date. | Audit the document register against the indexed scholarship documents. | Indexed documents with complete metadata (%) | 100% |
| RAI7 | Privacy | The system shall not store personal identifiers from user interactions beyond the active session. | Conduct test sessions containing dummy personal identifiers and inspect persistent logs and stored data. | Number of persisted personal identifiers | 0 |

---

## Reflection

**Which Responsible AI requirement will be the most difficult to measure or enforce technically?**

RAI1 (Hallucination / Groundedness) will be the most difficult requirement to measure and enforce technically. ScholarshipScan must ensure that every extracted value, such as citizenship requirements, age limits, minimum academic results and application deadlines, is directly supported by the original scholarship document.

A language model may produce an answer that appears reasonable but contains information that is not explicitly stated in the source. This is particularly risky because students may rely on incorrect scholarship requirements when deciding whether to apply for a scholarship.

To evaluate this requirement, I plan to manually compare extracted values against their retrieved supporting passages. Although automated evaluation may help reduce evaluation time, it may not reliably identify every unsupported statement.

RAI3 (Unsupported Information) is also closely related because the system must recognise when information is missing and return "not stated" instead of guessing. However, setting the evidence threshold too strictly may cause the system to reject information that is actually available. Therefore, groundedness and correct abstention must be evaluated together to ensure that ScholarshipScan provides reliable and verifiable scholarship information.
