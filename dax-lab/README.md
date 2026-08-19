# dax-lab — teaching scenarios (synthetic)

Three tiny semantic-model scenarios, each built to demonstrate **one** DAX behaviour that a
realistic model cannot show. They are the refreshable data source for the `lab/` scenarios of a
DAX reference library, alongside [`contoso-retail/`](../contoso-retail/), which is the base model
those same scenarios sit next to.

**100% synthetic.** No real person, company, store or transaction. Every value is either written
by hand (`blancos`, `claves-huerfanas`) or produced by a deterministic formula (`rendimiento`) —
no randomness, so regenerating the files reproduces them byte for byte.

Hosting them here, in a public repo, is what lets those models refresh on **anybody's** machine
over `raw.githubusercontent.com` — no authentication, no SQL Server, no local paths — while the
reference repo that ships the `.pbip` files stays private.

## The scenarios

### `blancos/` — blanks in a numeric column

`Tiendas.parquet`, 5 rows. Three stores have `Metros`, **two are null**. The values are chosen so
the denominator is readable at a glance: `100 + 200 + 300 = 600` is **200** over three rows and
**120** over five. That is the whole point — `AVERAGE` skips blanks, and a `+ 0` written "just in
case" silently changes which denominator you get.

### `claves-huerfanas/` — orphan keys and the blank row

`DimProducto.parquet` (3 rows: products 1, 2, 3) and `Ventas.parquet` (4 rows). The last sale
points at **product 99, which does not exist**. That single row is the scenario: the engine has to
hang its 50 units somewhere, and that somewhere is the blank row it adds to the dimension.

### `rendimiento/` — two million rows

`Ventas.parquet`, **2,000,000 rows**, three `int64` columns:

| column | values |
|---|---|
| `VentaKey` | 1..2,000,000, unique |
| `Importe` | 1..1000, spread by a prime step (7919) so no ordering can be exploited |
| `CategoriaKey` | 20 distinct values — cardinality 1 in 100,000 |

Volume is the point here: below a few million rows a good query plan and a bad one cost the same
and there is nothing to compare.

**The file is 385 KB, not 8.9 MB.** `VentaKey` is two million consecutive distinct values, so a
dictionary buys nothing and plain snappy leaves it at 8.9 MB — four times the whole Contoso
dataset. Encoded as differences (`DELTA_BINARY_PACKED`) it drops 23×, because the gap between one
row and the next is always 1. The other two columns keep their dictionary.

`Tiempos.parquet`, **4 rows**, carries the measured medians the scenario publishes — cold runs,
`ClearCache` before each one, median of three. It exists so the report can chart a **versioned
measurement** instead of a text box nobody can audit: a Power BI visual cannot time itself.

Four rows and not nine: group A was timed with its six measures **together**, so splitting those
5 ms across six rows would invent six numbers nobody measured. What the scenario publishes is the
ratio (~290x), not the milliseconds — an absolute number ages with the hardware.

## Example raw URL

```
https://raw.githubusercontent.com/CSalcedoDataBI/SampleDataSets/main/dax-lab/blancos/Tiendas.parquet
```

The models take the folder URL as an M parameter and append the file name, so pointing a model at
a fork, a branch or a local mirror is a one-value change.
