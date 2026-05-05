# Dataset Schema

All ingested datasets must follow a strict schema and column order to ensure consistent ingestion into the vector database and reliable retrieval during search.

| Column name | Description                                                                              | Example                                                              |
| ----------- | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| text        | The cleaned and processed content chunk that will be indexed and retrieved during search | “Apply nitrogen fertiliser at the tillering stage for better yield.” |
| type        | Indicates the type of source content                                                     | document, video                                                      |
| source      | The originating institution or platform from which the content is sourced                | MPKV Rahuri                                                          |
| name        | Human-readable name of the document or video                                             | “Wheat Fertiliser Advisory – MPKV”                                   |
| doc\_id     | Custom metadata field that stores the identifier of the original source document         | 9c7e2b14-acde-4f91-a3b2-71d9f8e4c2aa                                 |
| \_id        | A unique ID generated for the Marqo search engine for each indexed chunk or record       | 3a9e7c2b-4f81-4d9c-a6b5-2e1f8c7d9b30                                 |

All columns must be present and populated. Rows containing null or missing values are not ingested. The order of columns is fixed and must be maintained to match the ingestion schema defined in the vector database.

<br>
