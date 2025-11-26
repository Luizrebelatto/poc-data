# poc-data

## Mermaid Code

```mermaid
flowchart TD

Q[Query SQL] --> A[Databse]
A --> B[Extract Database]

C[API] --> D[Extract API]
E[CSV] --> F[Extract CSV]

B --> G
D --> G
F --> G

subgraph G[Transform / Clean]
    G1[Standardize types, Remove duplicates, Handle nulls, Normalize texts, Correct invalid values, Adjust timezone]
    G2[Parsing JSON, Standardizing schema, Converting types, Handling missing fields, Deduplication, Normalizing lists, Standardizing dates]
    G3[Adjust encoding, Correct delimiters, Clean headers, Handle NA, Correct types, Remove duplicates, Normalize formats]
end

G --> H[Save as Parquet / Data Lake]

B --> G1
D --> G2
F --> G3

G1 --> G
G2 --> G
G3 --> G

```

