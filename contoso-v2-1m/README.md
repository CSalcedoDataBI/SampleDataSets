# Contoso V2 — 1M

Star schema de **Contoso Data Generator V2** (SQLBI — Marco Russo & Alberto
Ferrari), exportado a parquet y servible por `raw.githubusercontent` sin
autenticacion.

No confundir con [`contoso-retail/`](../contoso-retail), que es **sintetico**
(generado con [CUG](https://github.com/PesanteAnalytics/contoso-universe-gen)).
Este es el Contoso de SQLBI, y se nota: **2.517 productos y 74 tiendas** frente
a 137 y 25. Esa riqueza de catalogo es la que hace que un visual de tarjetas KPI
se vea como debe.

| Tabla | Filas |
|---|---|
| FactSales | 2.237.028 |
| DimCurrencyExchange | 202.100 |
| DimCustomer | 88.941 |
| DimDate | 4.018 |
| DimProduct | 2.517 |
| DimStore | 74 |

32,72 MB en total; el mayor es `FactSales.parquet` con 29,45 MB.

## Columnas tal cual, sin renombrar

Cada parquet sale de la **vista** homonima de la base (`Sales`, `Customer`,
`Product`, `Store`, `Currency Exchange`, `Date`), asi que conserva los nombres
originales con espacios: `Order Number`, `Net Price`, `Unit Cost`, `Product
Name`, `Square Meters`… Un modelo construido sobre Contoso V2 lo consume **sin
tocar una sola consulta**.

## Uso desde Power BI

```m
DataBaseUrl = "https://raw.githubusercontent.com/CSalcedoDataBI/SampleDataSets/main/contoso-v2-1m"

let
    Source = Parquet.Document(Binary.Buffer(Web.Contents(DataBaseUrl, [RelativePath="FactSales.parquet"])))
in
    Source
```

> **`Binary.Buffer` no es opcional.** Por encima de unos pocos MB, `Web.Contents`
> entrega el binario en *streaming* y `Parquet.Document` falla con
> `cannot be used with streamed binary values`. Con ficheros pequenios el error
> no aparece, asi que es una trampa que solo se manifiesta al crecer.

`Web.Contents` con `RelativePath` mantiene cada consulta *firewall-safe*: la URL
base es estatica y solo cambia la ruta relativa.

## Origen

Exportado de la base `Contoso 1M` con `SELECT * FROM [<vista>]` y escrito con
pyarrow (compresion zstd). Los datos son los del generador de SQLBI; el credito
del esquema y del realismo temporal es suyo.
