# 20 – Chiclet Slicer con el identificador correcto

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones), segmentaciones de Año y Mes_Corto.

## Qué pasó
El Chiclet Slicer no se veía porque el identificador del visual que puse en los changelogs 18 y 19 era incorrecto: `ChicletSlicer1448562802762`. Era de memoria y no coincide con el que instala Power BI.

Al instalar el Chiclet, Desktop guardó el identificador real en `report.json` (`publicCustomVisuals`): `ChicletSlicer1448559807354`.

## Qué se hizo
- Las dos segmentaciones de la fila superior (Año y Mes_Corto) pasaron de segmentación nativa a `ChicletSlicer1448559807354`, con el campo en el rol `Category`.
- Distribución: Año en 4 columnas y Mes en 12 columnas, ambos en 1 fila (propiedades `general.columns` y `general.rows`).
- El orden de los meses sale de `Mes_Num` (la columna ya se ordena por ella en el modelo).
- `report.json` no se tocó: ya lista el visual.

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/{3321249c81bd226877bd,dc3a8f8200499969ebf4}/visual.json`
- `docs/checklist-requerimiento.md`, `docs/changelog/20-chiclet-slicer.md`, `docs/CHANGELOG.md`, `prompts/20-chiclet-slicer.md`

Este commit incluye también cambios que Desktop dejó guardados (esquemas en `Ventas.pbip`, `definition.pbir`, `definition.pbism`, `report.json` y reformateo de visuales).

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`; abrir Análisis General.
2. En la fila superior deben verse los años (2020 a 2023) y los meses (ENE a DIC) como botones.
3. Al elegir un año o un mes, todos los visuales de la página deben filtrarse.
4. Los meses deben aparecer en orden de calendario, no alfabético.

## Pendientes o riesgos
- No se probó en Desktop.
- Las propiedades `columns` y `rows` se escribieron de memoria; si no se aplican, el aspecto se ajusta a mano (Formato > General > Filas y columnas).
- El Chiclet Slicer aún debe reutilizarse en la página de Análisis de Tendencia.