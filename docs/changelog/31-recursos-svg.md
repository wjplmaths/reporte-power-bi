# 31 – Recursos gráficos en SVG

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones), recursos de diseño. La guía los menciona como un plus ("imágenes png para representar mejor las tarjetas").

## Qué se hizo
Se crearon 6 archivos SVG en `assets/svg/`, con la paleta del tema (`tema-ventas.json`): azul marino `#1F3864`, celeste `#56B4E9`, naranja `#E69F00`, verde `#009E73`, bermellón `#D55E00`, fondo gris `#F3F4F6` y borde `#E1E5EA`.

| Archivo | Tamaño | Contenido |
|---|---|---|
| `fondo-portada.svg` | 1280 × 720 | Degradado marino, círculos translúcidos en las esquinas, barras ascendentes abajo a la izquierda y franja de acento inferior. El centro queda libre para logo, título, autor y botones |
| `fondo-analisis.svg` | 1280 × 720 | Fondo gris claro con las zonas ya dibujadas como paneles blancos de bordes redondeados y sombra suave: 2 segmentaciones, 3 tarjetas (con franja de color celeste, naranja y verde), medidor, barras, mapa y tabla |
| `icono-facturado.svg` | 64 × 64 | Factura con una moneda con el símbolo de dinero |
| `icono-costo.svg` | 64 × 64 | Etiqueta de precio con una flecha hacia abajo |
| `icono-utilidad.svg` | 64 × 64 | Barras ascendentes con flecha de crecimiento |
| `logo-ventas.svg` | 256 × 256 | Cuadrado redondeado marino con cuatro barras ascendentes (sin texto, para que no dependa de ninguna fuente) |

## Cómo está pensado `fondo-analisis.svg`
Las zonas siguen la cuadrícula de la página Análisis General (márgenes de 20, separación de 10):
- Segmentaciones: (20, 16, 500 × 72) y (530, 16, 730 × 72)
- Tarjetas: (20, 98), (330, 98) y (640, 98), de 300 × 110
- Medidor: (950, 98, 310 × 185); barras Top 5: (950, 293, 310 × 407)
- Mapa: (20, 218, 380 × 482); tabla: (410, 218, 530 × 482)

Son aproximadas a las posiciones actuales de los visuales, que se ajustaron a mano en Desktop. Si se usa como fondo, se alinean los visuales sobre los paneles, o se retocan los números del SVG.

## Verificación
- Los 6 archivos son XML/SVG válidos.
- Se renderizaron con Microsoft Edge para revisarlos a ojo: se ven correctamente (colores de la paleta, iconos legibles, paneles alineados).

## Archivos creados/modificados
- `assets/svg/fondo-portada.svg`, `fondo-analisis.svg`, `icono-facturado.svg`, `icono-costo.svg`, `icono-utilidad.svg`, `logo-ventas.svg`
- `docs/changelog/31-recursos-svg.md`, `docs/CHANGELOG.md`, `prompts/31-recursos-svg.md`

El informe no se modificó: los recursos todavía no están conectados a ninguna página.

## Cómo usarlos en Power BI Desktop
- **Fondo de página:** Formato de página > Fondo de lienzo > Imagen > Examinar, elegir `fondo-portada.svg` (página Inicio) o `fondo-analisis.svg` (página de análisis); ajuste "Ajustar", transparencia 0 %.
- **Iconos y logo:** Insertar > Imagen, o reemplazar los de las tarjetas.
- Si tu versión de Desktop no admite SVG en alguna de estas opciones, se pueden convertir a PNG (por ejemplo, con Inkscape o abriendo el SVG en el navegador y capturando).

## Pendientes o riesgos
- No se probaron dentro de Power BI Desktop; solo en el navegador.
- Con el fondo de análisis aplicado, los visuales que tienen fondo blanco propio (por el tema) taparán los paneles dibujados; el efecto final es el mismo, y el SVG sirve sobre todo como guía de alineación.
- El logo y los iconos tienen fondo transparente: en el tema oscuro (changelog 30) los trazos azul marino se verían poco.