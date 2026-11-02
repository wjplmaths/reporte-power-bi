# 23 – Página Análisis de Tendencia

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones), página de Análisis de Tendencia.

## Qué se hizo
Se creó la página `AnalisisTendencia` ("Análisis de Tendencia"), registrada en `pages.json` después de Análisis General, con 6 visuales en el mismo lienzo de 1280 × 720:

| Zona | Visual | Contenido |
|---|---|---|
| Fila superior | 2 × Chiclet Slicer | `Calendario[Año]` y `Calendario[Mes_Corto]`, en las mismas posiciones y con el mismo formato que en Análisis General |
| Fila 2, izquierda | 2 × Tarjeta (nueva) | `Total Facturado` y `Ventas del Año Anterior` (en millones) |
| Fila 2, derecha | KPI | Indicador `Total Facturado`, meta `Ventas del Año Anterior`, eje de tendencia `Calendario[Año]` |
| Abajo | Columnas agrupadas y líneas | Columnas: `Meta` y `Total Facturado`. Línea (eje secundario): `% Cumplimiento metas`. Eje X: `Año`, `Trimestre` y `Mes_Corto` |

## Segmentaciones sincronizadas
Los dos Chiclet de la página nueva y los dos de Análisis General comparten grupo de sincronización: `Año` y `Mes_Corto`. Elegir un año o mes en una página se refleja en la otra. Se añadió `syncGroup` (campos y filtros) a los 4 visuales.

## Decisiones
- **Eje del KPI:** `Calendario[Año]`. El KPI compara el último valor del eje con su meta, así que con el año se compara el año seleccionado (o 2023, sin filtro) con el año anterior. Con `Date` compararía solo el último día.
- **Jerarquía Año-Trimestre-Mes:** el eje X tiene las tres columnas en ese orden, que Power BI trata como niveles de desglose (flechas de drill-down). No se creó una jerarquía en el modelo para no alterar `Calendario.tmdl`; si se prefiere, se crea en Desktop (clic derecho en `Año` > Crear jerarquía).
- **Orden de los meses:** por `Mes_Num`, ya configurado en el modelo.

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/AnalisisTendencia/page.json` y 6 `visuals/*/visual.json`
- `pbip/Ventas.Report/definition/pages/pages.json`
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/{3321249c81bd226877bd,dc3a8f8200499969ebf4}/visual.json` (grupo de sincronización)
- `docs/checklist-requerimiento.md`, `docs/changelog/23-analisis-tendencia.md`, `docs/CHANGELOG.md`, `prompts/23-analisis-tendencia.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`; abrir Análisis de Tendencia.
2. Las tarjetas deben mostrar ventas y ventas del año anterior en millones. Sin filtro de año, `Ventas del Año Anterior` suma los años con año previo en el calendario (2020 queda en blanco).
3. El KPI debe mostrar `Total Facturado` frente a su meta, con indicador de color.
4. El gráfico debe mostrar columnas de meta y facturado y una línea de porcentaje, con el eje por año. Con las flechas de desglose se baja a trimestre y mes.
5. Elegir un año en esta página y volver a Análisis General: el mismo año debe estar seleccionado.

## Pendientes o riesgos
- No se probó en Desktop.
- Los roles `Indicator`, `TrendAxis`, `Goal` (KPI) y `Category`, `Y`, `Y2` (gráfico combinado) se escribieron de memoria. Si algún visual sale vacío o con error, abre el campo en Desktop y pásame la captura.
- El KPI puede mostrar el 2020 en blanco si no hay año anterior; no es un error.
- Falta, en OE5, publicar el informe y entregar el `.pbix` (pasos manuales del usuario).