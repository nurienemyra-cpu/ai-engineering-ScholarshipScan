# ScholarshipScan — Week 3 Pipeline Decisions

## 1. Selected Data Source

The project uses three publicly accessible scholarship PDF documents:

1. Malaysia International Scholarship (MIS) 2026 Guidelines.
2. Malaysian Technical Cooperation Programme (MTCP) Scholarship 2026 Guidelines.
3. MyBrainSc Scholarship FAQ 2026.

These documents contain scholarship information relevant to the project's extraction requirements, including citizenship, age limits, academic qualifications, application deadlines and required documents.

## 2. Data Preparation Comparison

Two chunking configurations were evaluated using the same PDF documents.

| Configuration | Description |
|---|---|
| Fixed-size chunking | Divides extracted text into consecutive chunks of 500 characters. |
| Paragraph-aware chunking | Groups paragraphs while keeping each chunk within 500 characters. |

The paragraph-aware approach was selected provisionally because it attempts to preserve paragraph boundaries and contextual information. However, long paragraphs may still be split into smaller chunks.

The actual chunk counts and output statistics are recorded in the experiment results.

## 3. Selected Preparation Configuration

- Input format: PDF
- Text extraction: PyPDF
- Text cleaning: whitespace normalization
- Chunking method: Paragraph-aware
- Maximum chunk size: 500 characters
- Output format: JSONL
- Metadata: source filename, chunk ID and PDF page number

## 4. ETL / ELT Decision

An ETL approach was selected because the documents are extracted, cleaned and transformed into structured text chunks before being stored as prepared JSONL records.

## 5. Batch / Streaming Decision

Batch processing was selected because scholarship guidelines are static documents rather than continuous data streams. Documents can be processed when new scholarship information becomes available.

## 6. Pipeline Output

The prepared outputs are stored under:

- `data/processed/`
- `data/pipeline_v1/documents_mis/`
- `data/pipeline_v1/documents_mtcp/`
- `data/pipeline_v1/documents_mybrain/`

Lineage information is recorded in `data/week3_manifest.json` and individual pipeline manifests.

## 7. Limitations and Provenance

PDF extraction may produce formatting issues, particularly in tables, multi-column layouts and scanned pages. Paragraph-aware chunking may also split long paragraphs.

The source versions and access rights have not yet been independently verified. The original documents should remain available for checking extracted information.

## 8. Failure Points and Responses

| Stage | Possible Failure | Response |
|---|---|---|
| Ingest | PDF file is missing | Stop processing and check the source path. |
| Inspect | PDF contains no extractable text | Flag the document for OCR or manual review. |
| Prepare | Text extraction or chunking fails | Stop and inspect the affected page. |
| Validate | Prepared output is missing or empty | Stop the pipeline and regenerate the output. |
| Record lineage | Source or output checksum does not match | Report the mismatch and investigate the changed file. |
