# 21 – Etiqueta personalizada de % de avance del medidor

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones), medidor. Recupera la medida del changelog 14, que se había quitado en el 16 para limitarse a las de la guía.

## Qué se hizo
- Se volvió a crear la medida `Etiqueta % Avance` en `Medidas`, sin carpeta y con descripción:
  ```dax
  "Avance: " & FORMAT(COALESCE([% Cumplimiento metas], 0), "0.0%")
  ```
  Devuelve texto como `Avance: 74.9%`, y `Avance: 0.0%` si no hay meta.
- Medidor (Análisis General):
  - La medida se añadió a su información sobre herramientas (rol `Tooltips`).
  - Se redujo su alto de 300 a 240 px.
- Se creó una tarjeta nueva con la medida justo debajo del medidor (x = 960, y = 344, 300 × 52). Así el porcentaje de avance se ve siempre, sin esperar al tooltip.

## Por qué una tarjeta y no el texto dentro del globo
El texto del centro del medidor muestra el valor del campo `Total Facturado` y no se puede cambiar por una medida de texto desde el JSON. Un medidor solo acepta números en su valor, y una medida de texto no cabe ahí. La tarjeta y el tooltip son la forma estable.

## Paso manual opcional: etiqueta dentro del globo
Si quieres el porcentaje en el centro del medidor: seleccionar el medidor > Formato > Valor de llamada (o Etiqueta de datos) > Valor > fx. Si Desktop ofrece "Valor del campo", elegir `Etiqueta % Avance` o `% Cumplimiento metas`.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/Medidas.tmdl`
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/a7112fe163d6dacd2aef/visual.json` (medidor)
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/<nuevo>/visual.json` (tarjeta)
- `docs/modelo/diccionario.md`, `docs/checklist-requerimiento.md`
- `docs/changelog/21-etiqueta-avance.md`, `docs/CHANGELOG.md`, `prompts/21-etiqueta-avance.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`; abrir Análisis General.
2. En el panel de campos, `Medidas` debe tener la medida nueva `Etiqueta % Avance`.
3. Debajo del medidor debe verse una tarjeta con `Avance: xx.x%`.
4. Al elegir un año en el Chiclet, el texto debe cambiar. Con el cursor sobre el medidor, el tooltip también debe mostrar el avance.

## Pendientes o riesgos
- No se probó en Desktop.
- Sin filtro de año, el avance suma ventas y metas de los cuatro años.