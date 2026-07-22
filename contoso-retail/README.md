# Contoso Retail — sample Parquet (synthetic)

Retail star-schema sample data (Contoso). **100% synthetic** — generated with the
[Contoso Universe Generator](https://github.com/CSalcedoDataBI/contoso-universe-gen) (Faker), on top
of SQLBI's Contoso Data Generator V2. No real person or company.

These files are the **refreshable data source** for the Contoso Retail teaching semantic model in the
`fabric-data-agents` reference. Hosting them here (a public repo) lets the model refresh for anyone
over `raw.githubusercontent.com`, while the reference repo stays private.

| File | Grain |
|---|---|
| `FactSales.parquet` | one order line (126,524 rows) |
| `DimProduct` · `DimCustomer` · `DimStore` · `DimDate` · `DimCurrency` · `DimCurrencyExchange` | dimensions |

Example raw URL:
`https://raw.githubusercontent.com/CSalcedoDataBI/SampleDataSets/main/contoso-retail/FactSales.parquet`
