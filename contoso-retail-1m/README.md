# Contoso Retail 1M

Star schema de retail sintetico, en parquet, servible por `raw.githubusercontent`
sin autenticacion. Hermano mayor de [`contoso-retail/`](../contoso-retail): mismo
generador, misma semilla y mismas categorias, pero **10x pedidos y 9 anios de
historico** en vez de 2.

Existe porque las tarjetas KPI con sparkline y comparativa interanual necesitan
esa densidad para verse bien; con 2 anios de datos el grafico queda pobre.

| Tabla | Filas |
|---|---|
| FactSales | 1.198.197 |
| DimCustomer | 200.000 |
| DimCurrencyExchange | 78.888 |
| DimDate | 3.287 |
| DimProduct | 137 |
| DimStore | 25 |
| DimCurrency | 25 |

22,66 MB en total; el mayor es `FactSales.parquet` con 14,9 MB.

## Uso desde Power BI

Un solo parametro M, y cada particion cuelga de el:

```m
DataBaseUrl = "https://raw.githubusercontent.com/CSalcedoDataBI/SampleDataSets/main/contoso-retail-1m"

let
    Source = Parquet.Document(Web.Contents(DataBaseUrl, [RelativePath="FactSales.parquet"]))
in
    Source
```

`Web.Contents` con `RelativePath` mantiene cada consulta *firewall-safe*. Para
usar un fork, una rama o una copia local se cambia ese unico valor.

## Reproducirlo

Generado con [Contoso Universe Generator](https://github.com/PesanteAnalytics/contoso-universe-gen)
v0.3.0. El config exacto esta aqui al lado, en `cug-config.toml`:

```bash
cug generate -c cug-config.toml --verify --strict
```

Determinista por semilla: misma config = mismos datos. La generacion paso el
chequeo de integridad referencial, asi que no hay filas huerfanas.

> Ojo al copiar configs antiguos: `contoso-retail-ref-es.toml` usa claves de una
> version anterior (`orders_count`, `path`) que la v0.3.0 **ignora en silencio**
> y sustituye por sus valores por defecto. Las claves buenas son
> `[output] target_orders` y `[output] output_path`.
