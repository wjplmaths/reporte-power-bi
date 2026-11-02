# 22 – Íconos nativos de formato condicional en la tabla

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones), tabla de cumplimiento por vendedor.

## Qué pasó
En el changelog 18 dejé los íconos como paso manual y la tabla solo usaba color de fuente. Power BI sí guarda los íconos en el JSON del informe, y la guía los muestra (cuadro "Iconos - CUMPLIMIENTO" con reglas). Ahora se implementan en el propio informe.

## Cómo averigüé el formato
No había ejemplos en el proyecto. Usé:
- La estructura de formato condicional por íconos de la referencia pública de formato PBIR de [data-goblin/power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development): propiedad `icon` con `kind: "Icon"`, `layout` y `value` con una expresión `Conditional`.
- Los nombres de los íconos integrados de Power BI, de [RADACAD](https://radacad.com/power-bi-icon-names-for-conditional-formatting-using-dax). El conjunto sin círculo (X, !, ✔) es `SymbolLow`, `SymbolMedium` y `SymbolHigh`. Esa fuente no indica qué ícono es cuál; por el orden de las reglas, `SymbolLow` es la X roja, `SymbolMedium` el ! amarillo y `SymbolHigh` el ✔ verde.

## Qué se hizo
En la tabla de Análisis General, columna `% Cumplimiento metas`, se reemplazó el color de fuente por íconos con las reglas de la imagen de la guía:

| Regla | Ícono |
|---|---|
| valor ≥ 0 y < 0.6 | X rojo (`SymbolLow`) |
| valor ≥ 0.6 y < 0.8 | ! amarillo (`SymbolMedium`) |
| valor ≥ 0.8 | ✔ verde (`SymbolHigh`) |

- Diseño: a la izquierda de los datos (`Before`), igual que en la imagen.
- Se compara con el valor sin redondear, así que 79.96 % cae en amarillo aunque se muestre como 80.0 %.
- Se eliminó el formato de color de fuente que había (los íconos ya llevan color).

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/00ede76c2e04fc432cd8/visual.json` (tabla)
- `docs/checklist-requerimiento.md`, `docs/changelog/22-iconos-cumplimiento.md`, `docs/CHANGELOG.md`, `prompts/22-iconos-cumplimiento.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`; abrir Análisis General.
2. En la tabla, cada vendedor debe mostrar un ícono a la izquierda de su `% Cumplimiento metas`: X, ! o ✔ según el valor.
3. Seleccionar la tabla > Formato > Celdas > `% Cumplimiento metas` > Íconos: debe mostrar las 3 reglas de la guía (0 a 0.6, 0.6 a 0.8, 0.8 a Máx.).

## Pendientes o riesgos
- No se probó en Desktop.
- Si los íconos salen como imagen rota, el nombre del ícono no coincide con el de Power BI. Pasar el conjunto que prefieras por la ventana de Íconos de Desktop, guardar, y se lee el nombre real del JSON.
- Si salen en orden contrario (✔ donde debería haber X), hay que intercambiar `SymbolLow` y `SymbolHigh`.
- Sigue pendiente, como paso manual opcional, la etiqueta dentro del globo del medidor.