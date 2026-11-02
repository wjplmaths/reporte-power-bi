# 16 – Medidas: solo las de la guía y sin carpetas

**Fecha:** 2026-11-01
**Objetivo atendido:** OE4 (DAX). Ajusta los changelogs 14 y 15.

## Qué pasó
El usuario mostró el apartado "Cálculos" de la guía y pidió limitarse a esas medidas y quitar las carpetas de visualización.

## Qué se hizo
- Se quedaron las 6 medidas de la guía: `Total Facturado`, `Costo Total`, `Utilidad Total`, `% Cumplimiento metas`, `Meta` y `Ventas del Año Anterior`.
- Se eliminaron `Etiqueta % Avance` (no está en la guía) y `SumMetas`.
- `Meta` pasó a ser `SUM(Dim_Metas[Meta])` y `% Cumplimiento metas` a `DIVIDE([Total Facturado], [Meta])`. Es el mismo resultado que con `SumMetas` (la guía usa "SumMetas" como nombre del cálculo, no como una medida aparte).
- Se quitaron las carpetas: ya no hay `displayFolder`, así que las medidas se ven sueltas dentro de `Medidas`.
- Se conservaron las descripciones, los formatos y la consulta M de `Medidas`.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/Medidas.tmdl`
- `docs/modelo/diccionario.md`
- `docs/changelog/16-medidas-solo-guia.md`, `docs/CHANGELOG.md`, `prompts/16-medidas-solo-guia.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. `Medidas` debe mostrar solo 6 medidas, sin carpetas.
3. `Meta` y `% Cumplimiento metas` deben dar los mismos valores que antes. `Ventas del Año Anterior` está en blanco en 2020.

## Pendientes o riesgos
- No se probó en Desktop.
- Sin `Etiqueta % Avance`, la etiqueta personalizada del medidor (OE5) se hará con la medida `% Cumplimiento metas` o con una medida nueva cuando llegue ese paso.
- Los cambios 14 y 15 se mantienen en el historial; este reemplaza su lista de medidas.