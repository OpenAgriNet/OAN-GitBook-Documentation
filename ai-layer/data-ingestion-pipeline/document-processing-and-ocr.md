# Document Processing and OCR

Unstructured documents often include:

* Scanned PDFs
* Text-based PDFs
* Complex tables (e.g. fertiliser dosages, measurements)

Initial attempts using standard Python PDF extraction libraries resulted in complex table extraction and formatting errors, especially for tabular data. To address this, the pipeline was migrated to the Mistral OCR pipeline, which provides:

* Accurate extraction of tables
* Preservation of headers and document structure
* Cleaner downstream processing

Documents are converted into Markdown format after OCR.

For example, a scanned PDF containing fertiliser dosage tables is processed using the Mistral OCR pipeline. The OCR extracts both the textual content and tabular data, preserving headers and structure. The output is generated in Markdown format, which is then used for downstream cleaning and chunking.

The Markdown output generated after OCR serves as an intermediate artefact in the ingestion pipeline. This intermediate format allows teams to inspect, validate, and review extracted content before further processing.

Maintaining Markdown outputs as part of the setup process improves transparency and makes it easier to identify extraction or formatting issues prior to chunking and ingestion into the vector database.

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/unknown (4).png" alt=""><figcaption></figcaption></figure></div>
