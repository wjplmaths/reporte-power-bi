# 29 – Tema personalizado del informe

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones), formato del informe. El tema es opcional según la guía ("no es necesario realizar el informe exactamente con los formatos del ejemplo").

## Qué se hizo
Se creó `assets/themes/tema-ventas.json` y se registró en el informe como tema personalizado.

**Paleta de datos (8 colores)**, basada en la paleta de Okabe-Ito, pensada para daltonismo, más un azul marino (el color de los títulos y botones del informe) y un gris:

| # | Color | Hex |
|---|---|---|
| 1 | Azul marino | `#1F3864` |
| 2 | Naranja | `#E69F00` |
| 3 | Celeste | `#56B4E9` |
| 4 | Verde azulado | `#009E73` |
| 5 | Rosa | `#CC79A7` |
| 6 | Bermellón | `#D55E00` |
| 7 | Oro oscuro | `#8C6D1F` |
| 8 | Gris | `#6C757D` |

Colores semánticos para el cumplimiento de metas: `good` verde `#2E8B57`, `neutral` amarillo `#E6A800`, `bad` rojo `#C00000`. Degradado: mínimo `#E6F0FA`, centro `#56B4E9`, máximo `#1F3864`.

**Tipografía:** Segoe UI en todos los textos y Segoe UI Semibold en títulos, encabezados y cifras (14 clases de texto definidas).

**Estilos por defecto (`visualStyles`)**
- Todos los visuales: fondo blanco, borde gris claro con esquinas redondeadas (radio 8), sombra sutil y título alineado a la izquierda.
- Página: fondo gris muy claro `#F3F4F6`, para que los visuales resalten.
- Tarjetas (`cardVisual`): cifra en azul marino semibold y etiqueta gris.
- Tabla (`tableEx`): encabezado azul marino con texto blanco, filas alternas, líneas horizontales tenues y fila de total resaltada.
- Barras y combinado (`clusteredBarChart`, `lineClusteredColumnComboChart`): cuadrícula horizontal tenue, sin cuadrícula en el eje de categorías, etiquetas legibles y leyenda arriba. En el combinado, línea de 3 pt con marcadores.
- Medidor (`gauge`), KPI (`kpi`, con colores de estado del tema), mapa (`azureMap`) y segmentación nativa (`slicer`).

## Validación
El archivo se validó con el esquema oficial de temas de Power BI (`reportThemeSchema-2.159.json`, del repositorio [microsoft/powerbi-desktop-samples](https://github.com/microsoft/powerbi-desktop-samples)): **0 errores**. La primera versión dio 3 errores (colores de estado del KPI escritos como texto en vez de relleno) y se corrigió.

Contraste de la paleta frente al blanco (relación WCAG): `#1F3864` 11.6, `#6C757D` 4.7, `#8C6D1F` 4.9, `#D55E00` 3.9, `#009E73` 3.4, `#CC79A7` 3.1, `#56B4E9` 2.3, `#E69F00` 2.3. Los dos últimos tienen poco contraste con el fondo blanco, pero son rellenos de barras y columnas junto a colores oscuros, no texto. Se descartó el amarillo `#F0E442` de Okabe-Ito (1.3) y se sustituyó por oro oscuro.

## Cómo se registró
- Copia del tema en `pbip/Ventas.Report/StaticResources/RegisteredResources/tema-ventas.json`.
- `report.json`: nuevo elemento `tema-ventas.json` de tipo `CustomTheme` en `RegisteredResources` y `themeCollection.customTheme` apuntando a él. El tema base `CY24SU10` se mantiene.

## Archivos creados/modificados
- `assets/themes/tema-ventas.json` (nuevo; es la fuente del tema)
- `pbip/Ventas.Report/StaticResources/RegisteredResources/tema-ventas.json` (copia registrada)
- `pbip/Ventas.Report/definition/report.json`
- `docs/changelog/29-tema.md`, `docs/CHANGELOG.md`, `prompts/29-tema.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. Vista > Temas: debe aparecer "Tema Ventas" como tema aplicado.
3. El fondo de las páginas debe ser gris claro y los visuales deben tener borde redondeado y sombra suave.
4. Los visuales que ya tienen un formato propio (por ejemplo, el fondo azul de los botones o el color del texto de la portada) conservan el suyo. El tema solo cambia los valores por defecto.

## Pendientes o riesgos
- No se probó en Desktop. El esquema valida el formato del tema, pero no garantiza que todos los nombres de propiedades de cada visual se apliquen como se espera. Si algún visual no cambia, se ajusta ese objeto.
- La segmentación Chiclet no tiene estilo en el tema (los visuales personalizados no están en el esquema de temas); sigue con su formato propio.
- Si alguna cifra de las tarjetas o de la tabla queda con otro tamaño, es porque ese visual ya tenía un tamaño fijado en el informe.