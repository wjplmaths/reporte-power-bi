# 33 – Corrección de los tooltips de página

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5. Corrige el changelog 32.

## Qué pasó
Al pasar el cursor por las burbujas del mapa y por las barras del Top 5, Desktop mostraba el tooltip normal (nombre y valor) en vez de las páginas `TooltipMapa` y `TooltipBarras`.

## Qué se comprobó antes de cambiar nada
- Desktop reabrió el proyecto y conservó las dos páginas (`type: Tooltip`, `visibility: HiddenInViewMode`, tamaño real) y el vínculo del mapa y de las barras con ellas (`visualTooltip` con `type: ReportPage` y `section` igual al nombre de la página). Solo reformateó el JSON.
- Es decir, la configuración que había escrito era correcta en forma, pero Desktop no la tomaba como tooltip.

## Causa
En la documentación de Microsoft sobre la carpeta de informe de un proyecto de Power BI se indica que *"el objeto `pageBinding` es necesario para admitir drillthrough y tooltips de página"*. Mis dos páginas no lo tenían; yo había seguido la referencia de terceros, que lo da por opcional. Sin `pageBinding`, Desktop no registra la página como destino de tooltip y cae al tooltip predeterminado, sin ningún aviso.

## Qué se hizo
Se añadió a cada página de tooltip la propiedad `pageBinding` con un nombre único (GUID) y `type: "Tooltip"`:
```json
"pageBinding": { "name": "<guid>", "type": "Tooltip" }
```
Los visuales de las páginas, el vínculo del mapa y de las barras, y el resto del informe no cambian.

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/TooltipMapa/page.json`
- `pbip/Ventas.Report/definition/pages/TooltipBarras/page.json`
- Este commit incluye también el reformateo de JSON que Desktop dejó guardado en los visuales de las páginas de análisis y de tooltip (sin cambios de contenido).
- `docs/changelog/33-corregir-tooltips.md`, `docs/CHANGELOG.md`, `prompts/33-corregir-tooltips.md`

Fuente: [Power BI Desktop project report folder](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-report) (Microsoft Learn).

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. En Análisis General, pasar el cursor por una burbuja del mapa: debe aparecer la página con tres tarjetas y las barras de clientes del departamento.
3. Pasar el cursor por una barra del Top 5: tres tarjetas del cliente y las columnas de facturado por año.
4. En las páginas `Tooltip Mapa` y `Tooltip Barras`, Formato de página debe mostrar activa la opción "Permitir el uso como información sobre herramientas".

## Pendientes o riesgos
- No se probó en Desktop.
- Si sigue saliendo el tooltip normal, comprobar en cada visual (Formato > General > Tooltips) que el tipo sea "Página del informe" y que la página elegida sea la correcta. Si el selector aparece vacío, dímelo: puede ser una limitación del visual de mapa (Azure Maps).
- Si el tooltip aparece, pero con el contenido distinto al esperado, cambia solo los visuales de la página de tooltip.