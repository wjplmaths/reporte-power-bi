# 11 – Tabla calculada Calendario

**Fecha:** 2026-11-01
**Objetivo atendido:** OE3 (tabla Calendario), primera parte. Las relaciones y el modelado quedan para el siguiente paso.

## Qué se hizo
- Se creó `Calendario` como tabla calculada en TMDL, del 2020-01-01 al 2023-12-31, con 1 461 filas.
- Columnas: `Date`, `Año`, `Trimestre` (formato "1 Trim"), `Mes_Num`, `Mes_Corto` (ENE) y `Mes_Largo` (ENERO).
- Marcada como tabla de fechas: `dataCategory: Time` en la tabla y `isKey` en `Date`.
- `Mes_Corto` y `Mes_Largo` se ordenan por `Mes_Num` (`sortByColumn`).
- Se añadió `ref table Calendario` en `model.tmdl`.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/Calendario.tmdl` (nuevo)
- `pbip/Ventas.SemanticModel/definition/model.tmdl`
- `docs/checklist-requerimiento.md`, `docs/modelo/diccionario.md`
- `docs/changelog/11-calendario.md`, `docs/CHANGELOG.md`, `prompts/11-calendario.md`

Este commit incluye también 3 cambios mínimos que Desktop dejó guardados en `Dim_Metas`, `Dim_Territorios` y `Dim_Vendedores` (una anotación `PBI_NavigationStepName`) y un cambio en `visual.json` de la portada.

## Código clave
```dax
ADDCOLUMNS(
    CALENDAR(DATE(2020, 1, 1), DATE(2023, 12, 31)),
    "Año", YEAR([Date]),
    "Trimestre", QUARTER([Date]) & " Trim",
    "Mes_Num", MONTH([Date]),
    "Mes_Corto", UPPER(FORMAT([Date], "MMM", "es-ES")),
    "Mes_Largo", UPPER(FORMAT([Date], "MMMM", "es-ES"))
)
```
`FORMAT` lleva la cultura `es-ES` como tercer argumento para que los meses salgan en español aunque Desktop esté en otro idioma.

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. En la vista de tabla, `Calendario` debe tener 1 461 filas, de 01/01/2020 a 31/12/2023.
3. `Mes_Corto` debe mostrar ENE, FEB... y `Mes_Largo` ENERO, FEBRERO... en mayúsculas.
4. Al ordenar una visual por `Mes_Corto`, el orden debe ser calendario (ENE, FEB, MAR...), no alfabético.
5. En el menú de la tabla debe aparecer "Marcada como tabla de fechas" con `Date`.

## Pendientes o riesgos
- No se probó en Desktop.
- El mes de septiembre puede salir como `SEP` o `SEPT` según el sistema (`es-ES` usa "sept" en versiones recientes). Si sale `SEPT`, se puede dejar o forzar a 3 letras con `LEFT(..., 3)`.
- Aún no hay relaciones: falta relacionar `Fact_Ventas[Fecha de Venta]`, `Dim_Metas[Fecha]` y el resto (siguiente paso de OE3).
- Power BI tiene activada la fecha automática (existen `LocalDateTable_*` y `DateTableTemplate_*`). Con `Calendario` conviene desactivarla en Opciones del archivo; se recomienda hacerlo en el paso de relaciones.