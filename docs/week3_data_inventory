# Data Inventory — ScholarshipScan

## 1. Required Data (from Topic 2 Requirements and V1 Architecture)

| **Requirement ID** | **Needed Information / Feature** | **Candidate Source** | **Why Usable / Constraint** |
|---|---|---|---|
| FR1, FR2: Scholarship information extraction | Scholarship information containing citizenship, age limit, minimum results, application deadline and required documents. | Publicly accessible scholarship PDF documents and official scholarship websites. | These sources contain the eligibility requirements and application information needed by the system. Information may be presented in different formats and layouts, which can affect extraction accuracy. |
| FR3, RAI2: Source references | Supporting passages, document title, page number, section or webpage URL for every extracted field. | Original scholarship PDF documents and official scholarship webpages. | Source references allow students to verify extracted information. PDF page numbers and printed page numbers may differ, so both should be recorded where applicable. |
| FR4: Scholarship comparison | Normalised scholarship information for comparing different scholarship opportunities. | Extracted information from the registered scholarship documents. | A consistent table makes scholarship requirements easier to compare. Missing information must be displayed as "not stated" rather than left ambiguous. |
| FR5: Structured output | Five extracted fields converted into a fixed JSON schema. | System-generated extraction results. | A fixed schema ensures consistent output across different scholarship documents. All required fields must follow the predefined structure. |
| FR6, RAI3: Missing information handling | Examples of scholarship documents where one or more required fields are not explicitly stated. | Selected scholarship PDF documents and official webpages. | These examples are needed to evaluate whether the system correctly returns "not stated" instead of guessing missing information. |
| FR7, RAI4: Scope and refusal | At least 20 out-of-scope requests, such as personalised eligibility decisions and scholarship application advice. | Team-written evaluation dataset (D4). | Project-created test cases allow the system's refusal behaviour to be evaluated consistently. |
| NFR2, NFR3: Accuracy and retrieval | At least 8 scholarship records with ground-truth labels for all five required fields and supporting source passages. | Team-labelled scholarship dataset (D4). | Ground-truth values are needed to calculate precision, recall and F1-score, compare against a regex-based baseline and evaluate retrieval relevance. |
| RAI1: Groundedness | Extracted scholarship values and their corresponding supporting passages. | Original scholarship documents and labelled evidence passages. | Each extracted value must be checked against its original source to determine whether it is supported by evidence. |
| RAI6, NFR5: Provenance and maintainability | Scholarship title, sponsor, source URL, intake year, download date and document version information. | Document register (D3) and original scholarship documents. | Complete metadata helps identify the source and version of each document and supports future document updates. |

---

## 2. Source Records

| **Field** | **D1 — Scholarship Document 1** | **D2 — Scholarship Document 2** |
|---|---|---|
| **Source Identity** | To be confirmed: official scholarship provider or sponsor. | To be confirmed: official scholarship provider or sponsor. |
| **URL** | To be recorded from the official scholarship website or PDF source. | To be recorded from the official scholarship website or PDF source. |
| **Data Type** | PDF document or official scholarship webpage containing eligibility requirements and application information. | PDF document or official scholarship webpage containing eligibility requirements and application information. |
| **Purpose** | Primary source for extracting citizenship, age limit, minimum results, application deadline and required documents. | Additional source for extracting and comparing scholarship requirements. |
| **Version / Date** | To be confirmed from the document, including intake year or publication date. | To be confirmed from the document, including intake year or publication date. |
| **Access Rights** | To be checked. Confirm the document's licence, copyright conditions and permission for the intended processing and AI use. | To be checked. Confirm the document's licence, copyright conditions and permission for the intended processing and AI use. |
| **Inspected** | Pending document selection and inspection. | Pending document selection and inspection. |
| **Local File Details** | File name, size, page count and SHA-256: To confirm. | File name, size, page count and SHA-256: To confirm. |

Additional scholarship documents should be registered using the same structure.

**Important:** Only official or otherwise reliable scholarship sources should be used. Public accessibility does not automatically mean that a document can be freely ingested into an AI system. Copyright, licence and applicable usage restrictions should be checked before processing.

---

## 3. Inspection Evidence

### Inspection 1 — Scholarship PDF Documents

The following observations should be checked when inspecting the selected scholarship PDF documents.

| **#** | **Observation to Check** | **Evidence to Record** | **Implication** |
|---|---|---|---|
| 1 | Scholarship eligibility information may be distributed across different sections. | Record the sections containing citizenship, age limit, minimum results, application deadline and required documents. | The document retrieval process must identify relevant passages from different sections. |
| 2 | Different scholarship providers may use different wording for similar requirements. | For example, citizenship requirements may be expressed using terms such as "Malaysian citizens" or "Malaysian nationals". | The extraction model must recognise different expressions that refer to the same information. |
| 3 | Tables may contain important eligibility requirements. | Inspect tables containing academic qualifications, minimum grades, age limits and application details. | PDF table extraction must preserve the relationship between headings and values. |
| 4 | Application deadlines may be presented in different formats. | Record examples of deadlines written as specific dates, date ranges or intake periods. | The system should extract the deadline accurately without changing its meaning. |
| 5 | Some scholarship fields may not be explicitly stated. | Identify documents that do not contain one or more of the five required fields. | The system must return "not stated" when sufficient supporting evidence is unavailable. |
| 6 | Scholarship documents may contain different versions or intake years. | Record the publication date, intake year, document version and download date where available. | The document register must preserve source provenance to reduce the risk of using outdated information. |

### Inspection 2 — Official Scholarship Webpages

| **#** | **Observation to Check** | **Evidence to Record** | **Implication** |
|---|---|---|---|
| 1 | Scholarship information may be published directly on webpages rather than downloadable PDFs. | Record the official webpage URL and the sections containing scholarship requirements. | The system should support webpage content as well as PDF documents. |
| 2 | Information may be distributed across multiple webpage sections or pages. | Record the relevant headings, passages and URLs for each required field. | Retrieval must identify the correct supporting information and preserve its source reference. |
| 3 | Scholarship webpages may be updated between application intakes. | Record the date accessed, intake year and available publication or update date. | The system should retain document provenance and avoid treating older information as current. |
| 4 | Some webpages may link to separate application guidelines or downloadable documents. | Record related official PDF links and determine which source contains the authoritative requirement. | Related documents should be registered and linked appropriately to avoid incomplete extraction. |
| 5 | Website access and content extraction may be restricted. | Check website accessibility, usage terms and whether the content can be processed. | Only permitted and accessible content should be included in the system. |

---

## 4. Open Items

- [ ] Identify and select at least 8 suitable scholarship records from official or reliable sources.
- [ ] Download the selected scholarship PDF documents and record their file names, sizes, page counts and SHA-256 checksums.
- [ ] Record the title, sponsor, source URL, intake year and download date for every scholarship document.
- [ ] Check copyright, licensing and AI-processing permissions before using the selected documents.
- [ ] Inspect the selected documents to identify where the five required fields are located.
- [ ] Prepare ground-truth labels for citizenship, age limit, minimum results, application deadline and required documents.
- [ ] Record the supporting passage and source reference for every ground-truth field.
- [ ] Identify documents containing fields that are not explicitly stated to test the system's "not stated" functionality.
- [ ] Prepare at least 20 out-of-scope queries to evaluate the system's refusal behaviour.
- [ ] Prepare the evaluation dataset for field-level precision, recall, F1-score, retrieval hit rate and groundedness.
- [ ] Develop a document register to maintain scholarship source information and version history.
