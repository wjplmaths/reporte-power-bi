# 36 – Medidor sin la etiqueta “(En blanco)”

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (medidor). Corrige el punto 4 del changelog 35.

## Qué pasó
Con la etiqueta dinámica del changelog 35, el centro del medidor mostraba el texto “(En blanco)” en lugar del porcentaje de avance. El formato de esa etiqueta se había escrito por analogía con la de otros visuales, sin plantilla, y Desktop la leyó pero no obtuvo valor.

## Qué se hizo
Se retiró la etiqueta dinámica del medidor: se quitaron la propiedad `showDynamicLabels` y la entrada con `dynamicLabelValue` de `calloutValue`. El valor del globo vuelve a mostrar `Total Facturado` (C$201.69 mill., con el formato en millones de antes), como en la captura anterior al changelog 35. El resto del medidor no cambió: colores semánticos del arco, `Total Facturado` frente a `Meta` y la medida `Etiqueta % Avance` en el tooltip.

## Efecto en la calificación
La viñeta «Utilice una etiqueta personalizada en el valor del globo que represente el porcentaje de avance» (p.8) vuelve a 🟡: el % de avance está en el tooltip, pero no en el valor del globo. Es el único parcial que queda.

| Objetivo | Requisitos evaluados | ✅ | 🟡 | 🔍 | ❌ | % cumplido |
|---|---:|---:|---:|---:|---:|---:|
| OE1 | 5 | 5 | 0 | 0 | 0 | 100 % |
| OE2 | 49 | 44 | 0 | 5 | 0 | 90 % |
| OE3 | 14 | 14 | 0 | 0 | 0 | 100 % |
| OE4 | 8 | 8 | 0 | 0 | 0 | 100 % |
| OE5 | 27 | 22 | 1 | 2 | 2 | 81 % |
| **Total** | **103** | **93** | **1** | **7** | **2** | **90 %** (91 % ponderando el parcial) |

(La publicación y el enlace siguen excluidos del cálculo.) El % cumplido no cambia respecto al changelog 35 porque la etiqueta de entonces estaba contada como 🔍, no como ✅; lo que cambia es que ahora está como 🟡.

## Cómo conseguir la etiqueta en el globo
No se pudo por archivo. Se hace a mano en Desktop (paso 14 de `docs/PASOS_MANUALES_DESKTOP.md`): Formato del medidor > Valor de llamada > `fx` con la medida `Etiqueta % Avance`. Si lo haces, guarda el proyecto y avísame: copio el resultado al archivo para dejarlo en el repositorio.

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/a7112fe163d6dacd2aef/visual.json` (medidor)
- `docs/CHECKLIST_FINAL.md`, `docs/checklist-requerimiento.md`, `docs/PASOS_MANUALES_DESKTOP.md`
- `docs/changelog/36-medidor-sin-en-blanco.md`, `docs/CHANGELOG.md`, `prompts/36-medidor-sin-en-blanco.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. `Análisis General`: el centro del medidor debe mostrar `C$201.69 mill.` y no “(En blanco)”.
3. Pasar el cursor por el medidor: el tooltip debe mostrar `Etiqueta % Avance` (“Avance: 74.8 %”).

## Hallazgo aparte: tooltips de página eliminados
Al revisar el estado del proyecto se vio que las páginas `TooltipMapa` y `TooltipBarras` ya no existen en disco y `pages.json` ya no las lista: se eliminaron en Desktop después del changelog 35 (cambio del usuario, no de esta pasada, y todavía sin commit). El mapa y el gráfico de barras Top 5 siguen teniendo `visualTooltip` apuntando a esas páginas, así que son referencias colgantes y Desktop muestra el tooltip normal. No se tocó por no haberlo pedido; se anotó como H11 en `docs/CHECKLIST_FINAL.md` (opcional: quitar `visualTooltip` de esos dos visuales).

## Pendientes o riesgos
- No se probó en Desktop.
- La etiqueta del globo con el % queda como paso manual opcional.
