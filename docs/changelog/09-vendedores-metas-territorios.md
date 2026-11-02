# 09 – Dim_Vendedores, Dim_Metas y Dim_Territorios

**Fecha:** 2026-11-01
**Objetivo atendido:** OE2 (transformación de datos), segunda parte.

## Punto de partida
Antes de editar leí el estado actual de `pbip/`. Desktop había cambiado las consultas de G.Drive:
- `Tabla 1` a `Tabla 5` ya no usaban `Web.Page`, sino el CSV exportado de cada hoja con `Csv.Document(Web.Contents(".../export?format=csv&gid=..."))`. Sigue siendo origen Web.
- `GDrive_Fuente` y `fnBuscarTabla` ya no existían.

Descargué los CSV de las 5 hojas para escribir las transformaciones sobre datos reales.

## Qué se hizo
**Dim_Vendedores** (antes `Tabla 1`)
- Filas superiores: el CSV no las trae, el encabezado ya es la primera fila, así que no hay nada que quitar.
- Se promueven los encabezados, se quita `Primer Nombre`, se descartan IDs vacíos y se tipa.

**Metas_2020 a Metas_2023** (antes `Tabla 2` a `Tabla 5`)
- Ya no se cargan al modelo: pasaron de tabla a consulta intermedia (`expression`).
- Comparten la función `fnMetasMensuales`, que:
  - promueve encabezados;
  - renombra `ID_vendedor` a `ID_Vendedor`;
  - anula la dinamización de todas las columnas menos `ID_Vendedor`, con salida `Fecha` y `Meta`;
  - tipa las tres columnas.
- "Columna 1" no existe: el CSV no trae la columna de numeración que sí traía la vista web.

**Dim_Metas**
- Tabla nueva que se carga al modelo: `Table.Combine` de las cuatro consultas. Se espera 912 filas (19 vendedores × 12 meses × 4 años).

**Dim_Territorios** (antes `Tabla 6`)
- Se localiza la fila de encabezados (la que contiene "Departamento") y se quita lo de arriba.
- Se quitan la nota de fuente de abajo y las filas vacías.
- Quedan 4 columnas y se tipan.

## Cómo se convierten los meses de texto a fecha
Los encabezados llegan como texto `d/m/aaaa`: `1/1/2020` a `1/12/2020`, día 1 de cada mes.
```m
Date.FromText(Text.Trim(_), [Format="d/M/yyyy", Culture="en-US"])
```
Con formato y cultura fijos, el resultado no depende de la configuración regional del equipo. Con una regional `m/d/aaaa`, `1/10/2020` se leería como 10 de enero.

## Conversión de las metas
Las metas llegan como texto con formato de moneda: `251,625.83` en 2020 y `C$277,701.30` de 2021 a 2023.
```m
Number.FromText(Text.Select(Text.From(_), {"0".."9", "."}), "en-US")
```
Se dejan solo dígitos y punto decimal. El CSV las trae redondeadas a 2 decimales; el xlsx original traía más decimales (por ejemplo 251625.82499999995). La diferencia es de centavos. Los CSV se leen con `QuoteStyle.Csv` porque los valores con coma de miles vienen entre comillas.

## Columnas eliminadas y por qué
| Tabla | Columna | Motivo |
|---|---|---|
| Dim_Vendedores | `Primer Nombre` | Repite `Nombre Completo` y tiene apodos inconsistentes (Glenn/Glen, Leonor/Leo). |
| Dim_Territorios | `ISO:NI` | El código ISO no se usa en ninguna visual; el mapa usa el nombre del departamento. |
| Dim_Territorios | `Densidad` | Es un cálculo (población / superficie) que se puede hacer con DAX si hace falta. |
| Dim_Territorios | Columnas extra de la vista web (`Column1` a `Column7` guardaban 7, la tabla tiene 6) | Se seleccionan por nombre solo las 4 útiles, así que lo demás se descarta. |
| Metas_AAAA | Ninguna | Solo existen `ID_Vendedor` y los 12 meses, que se convierten en filas. |

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/expressions.tmdl` (nuevo): `fnMetasMensuales`, `Metas_2020` a `Metas_2023`
- `pbip/Ventas.SemanticModel/definition/model.tmdl`: refs, orden de consultas
- `pbip/Ventas.SemanticModel/definition/tables/{Dim_Vendedores,Dim_Metas,Dim_Territorios}.tmdl` (nuevos)
- Eliminados: `tables/Tabla 1.tmdl` a `Tabla 6.tmdl`
- `docs/checklist-requerimiento.md`, `docs/modelo/diccionario.md`
- `docs/changelog/09-vendedores-metas-territorios.md`, `docs/CHANGELOG.md`, `prompts/09-vendedores-metas-territorios.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. En Transformar datos:
   - `Dim_Vendedores`: 19 filas y 2 columnas.
   - `Dim_Metas`: 912 filas, `Fecha` entre 2020-01-01 y 2023-12-01 y `Meta` con valores como 251625.83.
   - `Metas_2020` a `Metas_2023`: en cursiva, sin carga.
   - `Dim_Territorios`: 17 departamentos, sin la fila de fuente.
3. Cerrar y aplicar; el modelo debe mostrar `Dim_Vendedores`, `Dim_Metas` y `Dim_Territorios`, sin las `Tabla N`.

## Pendientes o riesgos
- No se pudo ejecutar M aquí; nada está probado.
- `Dim_Territorios` es lo más frágil: la estructura real de la tabla en Desktop (7 columnas) no se pudo ver. Si falla, pega el error o el paso `Origen` de la consulta.
- Si Desktop vuelve a regenerar `.tmdl` (columnas, `PBI_QueryOrder`), revisa que no reaparezcan `Tabla 1` a `Tabla 6`.
- El `Departamento` de Wikipedia ("Costa Caribe Norte", "Costa Caribe Sur") debe coincidir con `Dim_Clientes` para el mapa; se comprueba en OE3 al relacionar.