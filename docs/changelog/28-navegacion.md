# 28 – Navegación entre páginas

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones), navegación del informe.

## Qué se hizo
Se añadieron botones de navegación con la misma apariencia que el botón de la portada (azul oscuro `#1F3864`, texto blanco), copiando la estructura del botón que Desktop ya había guardado:

| Página | Botón | Destino | Posición |
|---|---|---|---|
| Inicio | Ir al Análisis de Tendencia | Análisis de Tendencia | debajo del botón existente (x 490, y 590, 300 × 56) |
| Análisis General | Inicio | Inicio | esquina inferior derecha (x 1005, y 702, 125 × 18) |
| Análisis General | Tendencia | Análisis de Tendencia | esquina inferior derecha (x 1135, y 702, 125 × 18) |
| Análisis de Tendencia | Inicio | Inicio | esquina inferior derecha (x 1005, y 702, 125 × 18) |
| Análisis de Tendencia | Análisis General | Análisis General | esquina inferior derecha (x 1135, y 702, 125 × 18) |

El botón de Inicio hacia Análisis General ya existía y no se tocó.

## Cómo se respetó el diseño
- Los botones de las dos páginas de análisis van en la franja inferior de la página (y 702 a 720), que estaba libre: los visuales llegan hasta y 700.
- Ningún visual existente se movió ni se redimensionó. Se comprobó por posición que los botones nuevos no se solapan con ninguno.
- Los botones tienen un `z` mayor que el de los demás visuales, para que siempre se puedan pulsar.

## Archivos creados/modificados
- Un archivo `visual.json` nuevo por botón, en `pbip/Ventas.Report/definition/pages/{Inicio,AnalisisGeneral,AnalisisTendencia}/visuals/`
- `docs/changelog/28-navegacion.md`, `docs/CHANGELOG.md`, `prompts/28-navegacion.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. En Inicio debe haber dos botones. Con Ctrl + clic, el segundo lleva a Análisis de Tendencia.
3. En Análisis General y en Análisis de Tendencia, abajo a la derecha, hay dos botones pequeños. Con Ctrl + clic llevan a Inicio y a la otra página de análisis.
4. En los botones pequeños el texto debe leerse completo (9 pt en 18 px de alto).

## Pendientes o riesgos
- No se probó en Desktop.
- La franja inferior solo da 18 px de alto, así que los botones son pequeños. Si el texto sale cortado, se puede subir el alto de los botones a costa de acortar visuales, o ajustar la fuente en Formato.
- En el informe publicado en el servicio, los botones funcionan con un clic simple (sin Ctrl).