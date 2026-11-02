# 32 – Mejoras de medidor, KPI y tabla, y tooltips de página

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones), pulido de formato. Va más allá de lo que pide la guía.

## Punto de partida
Se leyó el estado guardado por Desktop antes de editar y se respetaron los ajustes del usuario: medidor con título centrado, etiquetas y objetivo ocultos; KPI con indicador de 30 pt; los íconos de la tabla (X, !, ✔), que ya estaban hechos en el changelog 22 y no se tocaron.

## Qué se hizo
**Medidor** (Análisis General)
- Arco con color semántico según el `% Cumplimiento metas`: menor a 60 % rojo `#C00000`, de 60 % a menos de 80 % amarillo `#E6A800`, 80 % o más verde `#2E8B57`. Son los mismos colores `good`/`neutral`/`bad` del tema.
- Valor central en millones con 2 decimales (por ejemplo `C$201.69 mill.`).

**KPI** (Análisis de Tendencia)
- Indicador en millones con 2 decimales.
- Objetivo y distancia al objetivo visibles; la distancia se muestra en porcentaje con 1 decimal, y "más alto es mejor".
- Colores de estado explícitos: bueno verde, neutral amarillo, malo rojo.

**Tabla** (Análisis General)
- `Total Facturado` y `Meta` en millones con 2 decimales.
- `% Cumplimiento metas` con 1 decimal.
- Íconos de cumplimiento: se mantienen los del changelog 22.
- Formato numérico: el separador de miles y el símbolo de moneda (`C$`) ya vienen del formato de las medidas (`"C$"#,0.00`, `0.0%`), así que no se repiten en el visual.

**Tooltips de página personalizados** (dos páginas nuevas, ocultas, de 400 × 280 px y tamaño real)
| Página | Se activa al pasar el cursor por | Contenido |
|---|---|---|
| `TooltipMapa` | una burbuja del mapa | Tarjetas de `Total Facturado`, `Costo Total` y `Utilidad Total` del departamento, y barras de los mejores clientes de ese departamento |
| `TooltipBarras` | una barra del Top 5 | Las mismas tres tarjetas del cliente y columnas de `Total Facturado` por año |

Los visuales de los tooltips no llevan borde, sombra ni fondo propios, y se activan en el mapa y en el gráfico de barras con la propiedad de tooltip de cada visual.

## Archivos creados/modificados
- Medidor: `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/a7112fe163d6dacd2aef/visual.json`
- KPI: `.../AnalisisTendencia/visuals/2413073d751b166ce799/visual.json`
- Tabla: `.../AnalisisGeneral/visuals/00ede76c2e04fc432cd8/visual.json`
- Mapa y barras (tooltip activado): `.../AnalisisGeneral/visuals/b5982da9636de7dfe6ac/visual.json` y `.../826d3d0b43c276cb6538/visual.json`
- Páginas nuevas: `pbip/Ventas.Report/definition/pages/TooltipMapa/` y `TooltipBarras/` (cada una con `page.json` y 4 visuales)
- `pbip/Ventas.Report/definition/pages/pages.json`
- `docs/changelog/32-medidor-kpi-tabla-tooltips.md`, `docs/CHANGELOG.md`, `prompts/32-medidor-kpi-tabla-tooltips.md`

Fuentes del formato: la referencia pública de PBIR [data-goblin/power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development) (páginas de tooltip y `visualTooltip`) y el esquema de temas de [microsoft/powerbi-desktop-samples](https://github.com/microsoft/powerbi-desktop-samples) (nombres de propiedades del KPI).

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. Medidor: el arco debe tener el color del tramo de cumplimiento (con datos de todos los años, 74.8 %, queda amarillo).
3. KPI: indicador en millones y debajo la distancia al objetivo en porcentaje.
4. Tabla: columnas de dinero en millones, `% Cumplimiento` con 1 decimal y los íconos de siempre.
5. Mapa: pasar el cursor por una burbuja debe abrir el tooltip con tres tarjetas y las barras. En el Top 5, el tooltip con las tarjetas y las columnas por año.
6. Las páginas `Tooltip Mapa` y `Tooltip Barras` no aparecen en las pestañas (están ocultas); se ven en Desktop en modo edición al buscar sus páginas.

## Pendientes o riesgos
- No se probó en Desktop.
- El color condicional del medidor usa la medida `Total Facturado` como campo del selector. Si el arco no cambia de color, se configura a mano: Formato del medidor > Colores > Relleno > fx > Reglas por `% Cumplimiento metas`.
- Los tooltips usan el contexto de la burbuja: con el mapa, el tooltip filtra por departamento. Al no estar `Dim_Metas` relacionada con el departamento, no se muestra `% Cumplimiento` en esos tooltips.
- Si algún tooltip no aparece, comprobar en el visual: Formato > General > Tooltips > Tipo "Página del informe" y la página elegida.
- Unidades en millones para toda la tabla: con filtros por un mes, las cifras serán pequeñas (decimales de millón). Se puede cambiar a miles ajustando `labelDisplayUnits` a `1000D`.