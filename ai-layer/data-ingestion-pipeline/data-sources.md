# Data Sources

Primary data sources include agricultural universities (e.g. MPKV, Rahuri and others), state departments, and programme teams such as NDKSP (Nananji Deshmukh Krushi Sanjeevani Prakalpa). Documents are typically shared in the form of PDFs, reports, advisories, or circulars. Once received, these documents are reviewed for relevance and completeness before being passed through the ingestion pipeline.

For example, an agricultural university may share a set of PDF advisories related to crop nutrition. These documents are collected by the programme team and provided as inputs to the document ingestion pipeline for processing and indexing.

The process of sourcing documents from agricultural universities is structured and governed to ensure both quality and authorisation. The OpenAgriNet (OAN) team conducted an onboarding session with designated university representatives, guiding them on how to prepare and structure advisory content for digital use. Since much of the legacy material was originally available in scanned or image-based formats, universities were requested to convert their content into searchable PDF documents. The OAN team also shared best-practice guidelines on content formatting, clarity, and consistency to improve downstream processing.

As part of the governance process, universities are required to complete a standard information form that captures who makes and owns the content and how frequently the content is reviewed, verified, and updated. Submission of this form serves as an explicit authorisation to use the content on the MahaVISTAAR platform. Finally, the respective government department reviews and approves the material before it is ingested into the MahaVISTAAR document pipeline. This ensures that only vetted, high-quality, and officially approved content is made available to farmers.

Once documents are received and validated, they are converted into a machine-readable format as part of the ingestion setup. Each PDF is first processed through an OCR pipeline that extracts both text and tables accurately. The extracted content is then transformed into Markdown format, which preserves headings, tables, and document structure in a standardised text form.

This Markdown file becomes the base working format for all downstream steps such as cleaning, token-based chunking, and validation. It also allows technical and domain teams to visually inspect the extracted content before it is indexed into the vector database

<br>
