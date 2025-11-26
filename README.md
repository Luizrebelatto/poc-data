# poc-data

## Mermaid Code

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
```
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

