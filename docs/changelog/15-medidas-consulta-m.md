# 15 – Medidas como consulta de Power Query

**Fecha:** 2026-11-01
**Objetivo atendido:** OE4 (DAX). Corrige el changelog 14.

## Qué pasó
La guía pide "Crear una consulta como contenedor de medidas: cree una nueva consulta llamada Medidas". En el changelog 14 se hizo con una tabla calculada DAX (`ROW("Columna", 1)`), que no es una consulta de Power Query. El usuario lo detectó al ver el resultado en el panel de campos.

## Qué se hizo
- `Medidas` ahora es una consulta M en blanco, que aparece en Transformar datos y se carga al modelo como contenedor de las medidas.
- La columna `Columna` sigue oculta y su `sourceColumn` pasó de `[Columna]` (DAX) a `Columna` (M).
- Las 8 medidas, con su DAX, formato, carpeta y descripción, no cambiaron.
- `Medidas` se añadió al final de `PBI_QueryOrder`.

## Código clave
```m
let
    // Consulta en blanco: solo sirve de contenedor de las medidas del modelo (OE4)
    Origen = #table(type table [Columna = Int64.Type], {})
in
    Origen
```

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/Medidas.tmdl`, `definition/model.tmdl`
- `docs/modelo/diccionario.md`, `docs/checklist-requerimiento.md`
- `docs/changelog/15-medidas-consulta-m.md`, `docs/CHANGELOG.md`, `prompts/15-medidas-consulta-m.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. En Transformar datos debe aparecer la consulta `Medidas`, vacía, con una columna `Columna`.
3. En el panel de campos: la tabla `Medidas` con las carpetas Etiquetas, Metas, Tiempo y Ventas, sin columna visible.
4. Las 8 medidas deben dar los mismos valores que antes.

## Pendientes o riesgos
- No se probó en Desktop. Puede pedir "Cerrar y aplicar", porque la tabla pasa de calculada a importada.
- Una tabla sin filas puede mostrar el icono de tabla en vez del de medidas hasta refrescar. No afecta a los cálculos.