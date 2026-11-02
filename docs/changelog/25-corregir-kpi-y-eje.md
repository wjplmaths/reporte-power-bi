# 25 – Corrección del KPI y del eje del gráfico combinado

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5, página Análisis de Tendencia. Corrige el changelog 23.

## Qué pasó
Con los Chiclet ya sincronizados, el usuario reportó dos cosas en Análisis de Tendencia:
- **El KPI "Total Facturado vs año anterior" aparecía vacío**, con o sin año seleccionado. Las tarjetas sí mostraban cifras, así que las medidas funcionan.
- **El gráfico combinado solo mostraba el nivel Año**, y la guía pide un comparativo por mes con jerarquía Año-Trimestre-Mes en el eje X.

## Qué se hizo
- **Gráfico combinado:** Desktop había guardado Trimestre y Mes_Corto como niveles no activos (`active: false`), así que solo se veía el año y había que bajar de nivel con las flechas. Ahora los tres niveles están activos (`active: true`), de modo que el eje muestra Año, Trimestre y Mes a la vez, con la jerarquía de la guía. Los meses siguen ordenados por `Mes_Num`.
- **KPI:** se quitó el eje de tendencia (`Calendario[Año]`). No pude confirmar por qué no pintaba con ese eje; sin él, el KPI compara directamente el indicador (`Total Facturado`) con su meta (`Ventas del Año Anterior`), que es lo que pide la guía ("comparativo entre las ventas actuales y las del período anterior"). También se quitó el filtro de `Año` que Desktop había añadido al visual.
- Lo demás no se tocó (tarjetas, segmentaciones sincronizadas, tamaños que ajustó el usuario).

## Revisión contra la guía (página de Análisis de Tendencia)
| Requisito de la guía | Estado |
|---|---|
| Tarjetas (nuevas): Facturación Total y Facturación Total del Año Anterior | Hecho: `Total Facturado` y `Ventas del Año Anterior` |
| KPI: ventas actuales frente a ventas del período anterior | Hecho (corregido en este cambio) |
| Columnas y líneas: meta frente a total facturado por mes | Hecho: columnas `Meta` y `Total Facturado` |
| La línea representa el % de cumplimiento | Hecho: `% Cumplimiento metas` en eje secundario |
| Jerarquía Año-Trimestre-Mes en el eje X | Hecho (corregido en este cambio) |
| Reutilizar las segmentaciones de la página anterior | Hecho: Chiclet de Año y Mes sincronizados |

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/AnalisisTendencia/visuals/2413073d751b166ce799/visual.json` (KPI)
- `pbip/Ventas.Report/definition/pages/AnalisisTendencia/visuals/bc9ea33c1b97d3ff6a61/visual.json` (combinado)
- Cambios de tamaño y posición que dejó Desktop en otros visuales
- `docs/changelog/25-corregir-kpi-y-eje.md`, `docs/CHANGELOG.md`, `prompts/25-corregir-kpi-y-eje.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`; abrir Análisis de Tendencia.
2. El KPI debe mostrar `Total Facturado` y, debajo, la comparación con `Ventas del Año Anterior`. Con 2021 y 2022 seleccionados se espera algo cercano a C$102.54 mill. frente a C$94.24 mill.
3. El gráfico debe mostrar un grupo de columnas por mes, agrupado por trimestre y año. Con todos los años son 48 meses; con un año, 12.
4. Con pocos años seleccionados el eje se lee mejor; si se ve apretado, se puede ensanchar o seleccionar un año.

## Pendientes o riesgos
- No se probó en Desktop.
- **KPI:** si sigue vacío sin el eje, la causa no es el eje. Abre el visual en Desktop, mira si muestra un icono de error en la esquina y pásame la captura.
- **Eje de tendencia:** si se quiere el fondo de área del KPI, se puede volver a arrastrar `Calendario[Año]` al campo "Eje de tendencia" en Desktop.
- **Eje combinado:** el valor `active: true` en los tres niveles es la forma en que Power BI guarda el "expandir todos los niveles". No lo pude contrastar con una plantilla.
- Falta, en OE5, publicar el informe y entregar el `.pbix` (pasos manuales del usuario).