# 30 – Variantes del tema (claro, oscuro, corporativo)

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (formato del informe). Variantes del tema del changelog 29.

## Qué se hizo
Se generaron tres variantes a partir de `assets/themes/tema-ventas.json`, con la misma estructura (mismos visuales y propiedades) y distinta paleta y acabado. Las tres validan contra el esquema oficial de temas (`reportThemeSchema-2.159.json`) con 0 errores.

| Variante | Archivo | Carácter |
|---|---|---|
| Claro | `assets/themes/tema-ventas-claro.json` | Idéntico al tema base (solo cambia el nombre): fondo de página gris muy claro, tarjetas blancas, bordes redondeados (radio 8) |
| Oscuro | `assets/themes/tema-ventas-oscuro.json` | Página `#12161C`, tarjetas `#1E252E`, texto claro, sombra más marcada |
| Corporativo | `assets/themes/tema-ventas-corporativo.json` | Página gris azulado `#EEF1F5`, azul marino `#0B2545`, esquinas casi rectas (radio 2) y sombra mínima |

## Paletas de datos (8 colores)
- **Claro:** `#1F3864`, `#E69F00`, `#56B4E9`, `#009E73`, `#CC79A7`, `#D55E00`, `#8C6D1F`, `#6C757D` (Okabe-Ito, apta para daltonismo).
- **Oscuro:** `#56B4E9`, `#E69F00`, `#009E73`, `#F0E442`, `#CC79A7`, `#D55E00`, `#B39DDB`, `#9AA5B1`. Se invirtió la lógica: el azul marino desaparecería sobre fondo oscuro y el amarillo, que era malo sobre blanco, aquí es el de más contraste.
- **Corporativo:** `#0B2545`, `#2A9D8F`, `#E9A23B`, `#8DA9C4`, `#9B2C5B`, `#5C6B7A`, `#C8553D`, `#A8B3BF`. Es más sobria; el verde azulado y el ámbar siguen distinguibles entre sí para daltonismo, pero los dos tonos azul/gris claros del final son más parecidos entre sí.

Colores semánticos (cumplimiento): claro `#2E8B57` / `#E6A800` / `#C00000`; oscuro `#4CC38A` / `#F2C94C` / `#FF6B6B`; corporativo `#2E7D32` / `#F9A825` / `#B71C1C`.

## Contraste (relación WCAG)
| | Texto sobre tarjeta | Texto secundario | Encabezado de tabla |
|---|---|---|---|
| Claro | 15.5 | 6.5 | 11.6 |
| Oscuro | 12.6 | 7.1 | 11.4 |
| Corporativo | 16.6 | 5.5 | 15.4 |

Todos pasan el 4.5 de WCAG AA para texto. En los tres temas el rojo de "malo" y el verde de "bueno" tienen contraste de 4.2 o más sobre la tarjeta, pero el amarillo de "neutral" queda en 2.0 a 2.1 en claro y corporativo, por eso en la tabla el ícono ! va junto al número y el estado no depende solo del color. En oscuro todos los colores de datos superan 4.0.

## Cómo se generaron
Con un script que parte del tema base y sustituye los colores por rol (acento, texto, texto secundario, cuadrícula, borde, fondo de página, fondo de tarjeta, fila alterna, total), más ajustes explícitos de encabezado de tabla, colores de estado del KPI, radio de bordes y sombra. La variante clara se comprobó idéntica al tema base salvo el nombre.

## Cómo aplicar una variante
El informe sigue usando el tema registrado (`tema-ventas.json`, que es la variante clara). Para probar otra:
1. En Desktop: Vista > Temas > Examinar temas, y elegir el archivo de `assets/themes/`. Es la forma rápida y reversible.
2. Para dejarla fija en el proyecto: copiar el archivo elegido sobre `pbip/Ventas.Report/StaticResources/RegisteredResources/tema-ventas.json` (mantiene el nombre del archivo, así `report.json` no cambia).

## Archivos creados/modificados
- `assets/themes/tema-ventas-claro.json`, `tema-ventas-oscuro.json`, `tema-ventas-corporativo.json` (nuevos)
- `docs/changelog/30-variantes-tema.md`, `docs/CHANGELOG.md`, `prompts/30-variantes-tema.md`

El tema registrado y los visuales del informe no se modificaron.

## Pendientes o riesgos
- No se probaron en Desktop. En la variante oscura, los visuales que ya tienen colores fijos en el informe (por ejemplo, los botones azules o el texto oscuro de la portada) no cambiarán con el tema; la portada se vería con texto oscuro sobre fondo oscuro. Si se adopta el oscuro, hay que revisar la portada.
- Si se adopta el tema oscuro, las imágenes de las tarjetas (iconos negros sobre fondo transparente) se verán poco: habría que usar versiones claras.