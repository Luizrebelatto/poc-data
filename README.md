# poc-data

## Mermaid Code: Same data different source

```mermaid
flowchart TD

Q[Query SQL] --> A[Database]
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

## Examples

### Database
```sql
id | name            | email                | signup_date                | active | purchases
---+-----------------+----------------------+----------------------------+--------+---------
1  | Luiz Gabriel    | luiz@email.com       | 2023-05-10 14:25:00        | true   | 1530.75
2  | Joao            | joao@email.com       | 2023-06-21 09:10:00        | false  | 245.00
3  | Gabriel         | gabriel@email.com    | 2023-07-02 18:40:00        | true   | 999.90

```

### csv
```
id;name;email;signup_date;active;purchases
1;Luiz Gabriel;luiz@email.com;05/10/2023 14:25;TRUE;1530,75
2; João ;joao@email.com;06/21/2023 09:10;FALSE;245
3;Gabriel; gabriel@email.com ;07/02/2023 18:40;na;999.90
```

### json
```json
[
  {
    "id": 1,
    "name": "Luiz Gabriel",
    "email": "luiz@email.com",
    "signup_date": "2023-10-05 14:25",
    "active": true,
    "purchases": 1530.75
  },
  {
    "id": 2,
    "name": "João",
    "email": "joao@email.com",
    "signup_date": "2023-06-21 09:10",
    "active": false,
    "purchases": 245
  },
  {
    "id": 3,
    "name": "Gabriel",
    "email": "gabriel@email.com",
    "signup_date": "2023-07-02 18:40",
    "active": null,
    "purchases": 999.90
  }
]

```

### MERMAI

```mermaid
flowchart TD

subgraph Sources
    A1[(DB Users Core)]
    A2[(API Emails)]
    A3[(CSV Purchases 1TB)]
    A4[(DB Signup)]
    A5[(API Status)]
end

subgraph RequestManager
    B1[Batch Requests]
    B2[Parallel Workers]
    B3[Rate Limiter]
    B4[Retry Handler]
end

%% DB sources go directly
A1 --> E1[Extract ID and Name]
A4 --> E4[Extract Signup Date]

%% API sources through Request Manager
A2 --> B1 --> B2 --> B3 --> B4 --> E2[Extract Email]
A5 --> B1 --> B2 --> B3 --> B4 --> E5[Extract Active]

%% CSV 1TB distributed flow
A3 --> R1[Distributed CSV Reader]
R1 --> R2[Parallel Chunk Parser]
R2 --> R3[Convert to Parquet Partitions]
R3 --> E3[Load Purchases Table]

subgraph Transform
    T1[Normalize Names Fix Types Dedup]
    T2[Validate Email Normalize Domain]
    T3[Clean Purchases Fix Types Normalize]
    T4[Standardize Dates Fix Timezone]
    T5[Convert Boolean Handle Missing]
end

E1 --> T1
E2 --> T2
E3 --> T3
E4 --> T4
E5 --> T5

T1 --> U[Join by User ID]
T2 --> U
T3 --> U
T4 --> U
T5 --> U

U --> A[Aggregate Purchases]

A --> F[Save Final Table Parquet]

```
