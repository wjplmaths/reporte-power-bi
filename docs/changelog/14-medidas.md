# 14 – Tabla Medidas y medidas DAX

**Fecha:** 2026-11-01
**Objetivo atendido:** OE4 (DAX).

## Qué se hizo
- Se creó la tabla contenedora `Medidas`: tabla calculada `ROW("Columna", 1)` con la columna oculta, solo para agrupar medidas.
- Se crearon 8 medidas, cada una con descripción (comentario `///` en TMDL), formato y carpeta de visualización.
- Los nombres de columna se contrastaron con el modelo real. Coinciden con los del prompt: `Fact_Ventas[Cantidad]`, `Fact_Ventas[Precio Unitario]`, `Dim_Productos[Costo Unitario]`, `Dim_Metas[Meta]` y `Calendario[Date]`.
- Se añadió `ref table Medidas` en `model.tmdl`.

## Medidas
| Carpeta | Medida | Descripción |
|---|---|---|
| Ventas | `Total Facturado` | Cantidad × Precio Unitario por línea, sumado |
| Ventas | `Costo Total` | Cantidad × costo unitario del producto (`RELATED`) |
| Ventas | `Utilidad Total` | Total Facturado − Costo Total |
| Metas | `SumMetas` (oculta) | Suma de `Dim_Metas[Meta]` |
| Metas | `Meta` | `[SumMetas]` |
| Metas | `% Cumplimiento metas` | `DIVIDE([Total Facturado], [SumMetas])` |
| Tiempo | `Ventas del Año Anterior` | `SAMEPERIODLASTYEAR` sobre `Calendario[Date]` |
| Etiquetas | `Etiqueta % Avance` | Texto "xx.x%" para el medidor |

## Decisiones
- `Meta` se define como `[SumMetas]`, siguiendo la guía ("Meta = SumMetas"). `SumMetas` queda oculta y `% Cumplimiento metas` la usa directamente, igual que la guía (Total Facturado / SumMetas).
- `Etiqueta % Avance` devuelve texto con `FORMAT(..., "0.0%")`. Usa `COALESCE(..., 0)` para que el medidor muestre `0.0%` y no vacío cuando no hay meta. Las medidas devuelven texto, así que el medidor la usa en la etiqueta personalizada del valor y no como valor numérico.
- El formato de moneda es `"C$"#,0.00` (córdoba).
- Los porcentajes usan `0.0%`, así que 79.96 % se muestra como 80.0 %. Para el formato condicional (menor a 60 %, 60 a 79 %, mayor a 80 %) conviene comparar con el valor sin redondear.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/Medidas.tmdl` (nuevo), `definition/model.tmdl`
- `docs/modelo/diccionario.md`, `docs/checklist-requerimiento.md`
- `docs/changelog/14-medidas.md`, `docs/CHANGELOG.md`, `prompts/14-medidas.md`

Este commit incluye también cambios que Desktop dejó guardados en `relationships.tmdl`, `Dim_Clientes.tmdl`, `Dim_Territorios.tmdl`, `diagramLayout.json` y un visual de la portada (solo reordenan líneas).

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. En el panel de campos debe verse la tabla `Medidas` con las carpetas Ventas, Metas, Tiempo y Etiquetas. `SumMetas` y la columna `Columna` no deben aparecer.
3. En una tarjeta, `Total Facturado` y `Costo Total` deben dar cifras positivas. `Utilidad Total` = diferencia de ambas.
4. En una tabla con `Año` y `Mes_Corto`: `Ventas del Año Anterior` está en blanco en 2020 y tiene valor desde 2021.
5. En una tabla por vendedor: `Meta` y `% Cumplimiento metas` tienen valor por vendedor.

## Pendientes o riesgos
- No se probó en Desktop.
- `SAMEPERIODLASTYEAR` exige que `Calendario[Date]` esté marcada como tabla de fechas y relacionada con `Fact_Ventas`. Ya lo está (changelogs 11 y 13).
- Si en los datos hay IDs de producto de `Fact_Ventas` que no están en `Dim_Productos`, `RELATED` devuelve en blanco y `Costo Total` queda subestimado. No se verificó.
- El formato `"C$"` en TMDL no se probó; si Desktop lo rechaza, usar `\C\$#,0.00`.