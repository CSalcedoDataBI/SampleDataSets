# SampleDataSets

Public datasets used in tutorials, blog posts, and worked examples at [csalcedodatabi.com](https://csalcedodatabi.com) — where the Power BI, Fabric, and data-agent techniques behind these datasets are explained in depth.

## Datasets

### contoso-retail/

Synthetic retail sales dataset, amounts in MXN. Seven Parquet tables: `DimCurrency`, `DimCurrencyExchange`, `DimCustomer`, `DimDate`, `DimProduct`, `DimStore`, `FactSales`.

Used in the **[Contoso Retail Agent](https://github.com/CSalcedoDataBI/fabric-data-agents/tree/main/examples/contoso-retail)** worked example — a complete Microsoft Fabric Data Agent over this model, illustrating additive vs. non-additive measures, breakdown defaults, and few-shot queries.

### dax-lab/

Three tiny teaching scenarios, each isolating one DAX behaviour a realistic model cannot show: blanks in a numeric column (`blancos/`), orphan foreign keys and the blank row the engine adds (`claves-huerfanas/`), and a 2,000,000-row table for query-plan cost comparisons (`rendimiento/`).

Used as the refreshable source for the executable `lab/` scenarios of a DAX reference library. See [`dax-lab/README.md`](dax-lab/README.md).

## License

MIT © 2026 Cristóbal Salcedo ([CSalcedoDataBI](https://github.com/CSalcedoDataBI)).
