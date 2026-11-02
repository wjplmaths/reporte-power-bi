# 07 – Transformación de Fact_Ventas, Dim_Clientes y Dim_Productos

**Fecha:** 2026-11-01
**Objetivo atendido:** OE2 (transformación de datos), primera parte: las tres tablas locales.

## Qué se hizo
Cada consulta tiene pasos con nombre y un comentario por paso.

**Fact_Ventas**
- Se leyó la hoja sin promover encabezados y se eliminaron las 3 filas superiores (título y filas vacías). La fila 4 pasó a ser el encabezado.
- Se conservaron solo 7 columnas, seleccionadas por nombre.
- Se eliminaron las filas sin `ID Transacción`. En los datos actuales no hay ninguna.
- Tipos: los IDs y `Cantidad` como entero, `Fecha de Venta` como fecha y `Precio Unitario` como decimal.

**Dim_Clientes**
- Se promovieron los encabezados y se quitaron los clientes con ID null o vacío (hoy no hay ninguno).
- "Región Autónoma de la Costa Caribe Norte" pasó a "Costa Caribe Norte", y "Región Autónoma de la Costa Caribe Sur" a "Costa Caribe Sur".
- Tipos: `ID Cliente` como entero, `Fecha de Alta` como fecha (leída como `d/m/aaaa` con `es-ES`) y el resto como texto.

**Dim_Productos**
- Se promovieron los encabezados y se quitaron los productos con ID null o vacío (hoy no hay ninguno).
- Tipos: `ID Producto` y `Tiempo de Espera (dias)` como entero, `Costo Unitario` como decimal (punto decimal, leído con `en-US`) y el resto como texto.

## Columnas eliminadas y por qué
| Tabla | Columna | Motivo |
|---|---|---|
| Fact_Ventas | Columna A (vacía) | No tiene datos; los datos empiezan en B. |
| Fact_Ventas | Columna L | Solo tiene formato, sin datos. |
| Fact_Ventas | `Tasa de Impuesto` | Vale 15 en las 266 128 filas y ninguna medida del requerimiento la usa. |
| Fact_Ventas | `Fecha de Envío` | Ninguna medida la usa y llega a enero de 2024, fuera del `Calendario` 2020-2023. Dejarla invitaría a una relación inactiva sin sentido. |
| Fact_Ventas | `ID Factura` | El requerimiento no pide ningún cálculo por factura. Se puede recuperar si se necesita contar facturas. |
| Dim_Clientes | ninguna | `País` vale siempre "Nicaragua", pero no se quitó porque el prompt no lo pide y no estorba. |
| Dim_Productos | ninguna | Todas las columnas son atributos útiles para segmentar. |

Se conservó `ID Transacción` como llave de la fila.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/{Fact_Ventas,Dim_Clientes,Dim_Productos}.tmdl`
- `docs/checklist-requerimiento.md`, `docs/modelo/diccionario.md`
- `docs/changelog/07-transformacion-tablas-locales.md`, `docs/CHANGELOG.md`, `prompts/07-transformacion-tablas-locales.md`

## Código clave
```m
// Fact_Ventas
FilasSuperioresEliminadas = Table.Skip(Hoja, 3),
EncabezadosPromovidos = Table.PromoteHeaders(FilasSuperioresEliminadas, [PromoteAllScalars=true]),
ColumnasUtiles = Table.SelectColumns(EncabezadosPromovidos, {"ID Transacción", "ID Vendedor", "ID Cliente", "ID Producto", "Fecha de Venta", "Cantidad", "Precio Unitario"}),

// Dim_Clientes
SinIdNulo = Table.SelectRows(EncabezadosPromovidos, each [ID Cliente] <> null and [ID Cliente] <> ""),
CaribeNorte = Table.ReplaceValue(SinIdNulo, "Región Autónoma de la Costa Caribe Norte", "Costa Caribe Norte", Replacer.ReplaceValue, {"Departamento"}),
CaribeSur = Table.ReplaceValue(CaribeNorte, "Región Autónoma de la Costa Caribe Sur", "Costa Caribe Sur", Replacer.ReplaceValue, {"Departamento"}),
```
Los pasos completos están en cada archivo `.tmdl`.

## Cómo validar en Power BI Desktop
1. Reabrir `pbip/Ventas.pbip` y abrir Transformar datos.
2. `Fact_Ventas`: 266 128 filas, 7 columnas con los encabezados correctos y `Fecha de Venta` entre 2020-01-02 y 2023-12-30.
3. `Dim_Clientes`: 442 filas; en `Departamento` aparecen "Costa Caribe Norte" y "Costa Caribe Sur", y ya no los nombres largos.
4. `Dim_Productos`: 227 filas; `Costo Unitario` con valores como 20.94 (no 2094).
5. Ninguna columna debe mostrar errores en la barra de calidad.

## Pendientes o riesgos
- No se pudo ejecutar M desde aquí; el código no está probado.
- `Table.Skip(Hoja, 3)` supone que Power Query conserva las filas 2 y 3, vacías, al leer la hoja. Si la primera fila de datos aparece como encabezado o falta una, cambiar el 3.
- Si algún `ID Cliente` de `Fact_Ventas` no existe en `Dim_Clientes` (el máximo es 713 frente a 442 clientes), saldrá al crear las relaciones en OE3.
- Las casillas del checklist siguen sin marcar hasta validar en Desktop.