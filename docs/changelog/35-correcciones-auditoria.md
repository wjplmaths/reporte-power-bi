# 35 – Correcciones de la auditoría y nueva calificación

**Fecha:** 2026-11-01
**Objetivo atendido:** OE2, OE4 y OE5 (cierre de los pendientes de `docs/CHECKLIST_FINAL.md` que se pueden resolver por archivo).

## Criterio
Se corrigió todo lo que no requiere una revisión manual y no cambia el diseño ni la estructura del informe. Quedó fuera, por indicación del usuario, la publicación y el enlace compartido. No se movió ni redimensionó ningún visual, no se cambiaron páginas, relaciones ni tema.

## Qué se corrigió
1. **`SumMetas` (OE4, p.6 «% Cumplimiento metas = Total Facturado / SumMetas», «Meta = SumMetas»).** Nueva medida oculta `SumMetas = SUM(Dim_Metas[Meta])`; `Meta = [SumMetas]` y `% Cumplimiento metas = DIVIDE([Total Facturado], [SumMetas])`. Mismos valores que antes.
2. **Pasos visibles en las metas (OE2, p.2-3).** Cada `Metas_2020…2023` muestra ahora sus 8 pasos nombrados y comentados, tal como los lista la guía: origen, eliminar filas superiores, promover encabezados, eliminar “Columna 1”, renombrar `ID_vendedor`, anular dinamización a partir de `ID_Vendedor`, renombrar a `Fecha` y `Meta`, y tipos. La función compartida `fnMetasMensuales` se eliminó.
   - “Eliminar filas superiores” se hace buscando la fila de encabezados y quitando lo que haya encima (0 filas en el CSV actual).
   - “Eliminar Columna 1” quita cualquier columna sobrante sin encabezado (ninguna en el CSV actual).
3. **`Dim_Vendedores` (OE2, p.2).** Se añadió el paso visible de eliminar filas superiores antes de promover encabezados.
4. **Etiqueta personalizada del medidor (OE5, p.8).** El valor del globo del medidor usa ahora la etiqueta dinámica con la medida `Etiqueta % Avance` (“Avance: 74.8 %”). Se escribió con el formato de etiquetas dinámicas de Power BI documentado para otros visuales, así que **falta confirmarla en Desktop** (paso 14).
5. **Portada.** Se eliminó la referencia residual del logo a `logo.jpeg`, un recurso que ya no existe. No cambia nada visible.

## Comprobación
- Simulación en Python de los nuevos pasos con los 4 CSV reales de G.Drive: la fila de encabezados está en la posición 0, no se quita ninguna columna, y salen **912 filas** con suma de metas **269,764,999.68**, igual que antes y que Desktop. `Dim_Vendedores`: encabezado en posición 0 y 19 filas.
- Todos los JSON del informe siguen siendo válidos.
- No se pudo ejecutar Power Query ni abrir Desktop, así que los cambios de M y de DAX se confirman al abrir el proyecto (pasos 3 y 5 de `docs/PASOS_MANUALES_DESKTOP.md`).

## Lo que no se tocó, y por qué
| Pendiente | Motivo |
|---|---|
| Imágenes en las tarjetas (el “plus” de OE5.8) | Se quitaron a propósito en Desktop; reponerlas cambia el diseño |
| Tema `tema-ventas` aplicado (H1) | Decisión de diseño; la guía no lo exige |
| Rejilla de tarjetas, segmentaciones y botones (H6) | Ajustes de diseño hechos a mano |
| Publicar y compartir el enlace (OE5.27) | Excluido por indicación del usuario |
| `.pbix` en `dist/` y Classroom (OE5.28) | Requiere Desktop |
| `Dim_Territorios`, regla roja de los íconos, KPI, “Cerrar y aplicar” | Solo se pueden validar en Desktop |

## Nueva calificación
La publicación y el enlace (OE5.27) se excluyen del cálculo.

| Objetivo | Requisitos evaluados | ✅ | 🟡 | 🔍 | ❌ | % cumplido |
|---|---:|---:|---:|---:|---:|---:|
| OE1 | 5 | 5 | 0 | 0 | 0 | 100 % |
| OE2 | 49 | 44 | 0 | 5 | 0 | 90 % |
| OE3 | 14 | 14 | 0 | 0 | 0 | 100 % |
| OE4 | 8 | 8 | 0 | 0 | 0 | 100 % |
| OE5 | 27 | 22 | 0 | 3 | 2 | 81 % |
| **Total** | **103** | **93** | **0** | **8** | **2** | **90 %** |

| | Antes (changelog 34) | Después |
|---|---:|---:|
| Requisitos evaluados | 104 | 103 |
| ✅ cumplidos | 82 | 93 |
| 🟡 parciales | 12 | 0 |
| 🔍 por validar en Desktop | 7 | 8 |
| ❌ pendientes | 3 | 2 |
| % cumplido | 79 % | **90 %** |

Por qué sube 11 puntos: los 12 parciales (🟡) se cerraron como ✅ (11, entre `SumMetas`, los pasos de `Dim_Vendedores` y las 4 consultas de metas × 2 pasos) o pasaron a 🔍 (1, la etiqueta del medidor, hecha pero sin ver en Desktop); además salió del cálculo el requisito de publicar. Los 🔍 suben a 8 por esa etiqueta y los ❌ bajan a 2 (el “plus” de imágenes en tarjetas y la entrega a Classroom).

Si todas las validaciones en Desktop salen bien (los 8 🔍), el cumplimiento sube al 98 %; el 2 % restante es el “plus” de imágenes y la entrega a Classroom.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/Medidas.tmdl` (`SumMetas`, `Meta`, `% Cumplimiento metas`)
- `pbip/Ventas.SemanticModel/definition/expressions.tmdl` (8 pasos por consulta de metas; sin `fnMetasMensuales`)
- `pbip/Ventas.SemanticModel/definition/tables/Dim_Vendedores.tmdl` (paso de filas superiores)
- `pbip/Ventas.SemanticModel/definition/model.tmdl` (orden de consultas)
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/a7112fe163d6dacd2aef/visual.json` (etiqueta del medidor)
- `pbip/Ventas.Report/definition/pages/Inicio/visuals/b9bba74a5ea34ab52103/visual.json` (referencia residual)
- `docs/CHECKLIST_FINAL.md`, `docs/checklist-requerimiento.md`, `docs/PASOS_MANUALES_DESKTOP.md`, `docs/modelo/diccionario.md`
- `docs/changelog/35-correcciones-auditoria.md`, `docs/CHANGELOG.md`, `prompts/35-correcciones-auditoria.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`. Si aparece un aviso de problemas, copiar el detalle.
2. Editor de Power Query: `Metas_2020…2023` con 8 pasos cada una y 228 filas; `Dim_Metas` con 912; `Dim_Vendedores` con el paso `FilasSuperioresEliminadas` y 19 filas.
3. Tarjeta con `Meta` = C$269,764,999.68 y `% Cumplimiento metas` = 74.8 % (igual que antes); `SumMetas` no debe verse en el panel de campos (está oculta).
4. Medidor: el valor del globo debe decir “Avance: 74.8 %”.

## Pendientes o riesgos
- Nada se probó en Desktop. El riesgo mayor es el M de las metas, que es el que alimenta toda la comparación de cumplimiento; el cálculo equivalente en Python coincide con el valor esperado.
- La etiqueta del medidor puede no mostrarse si el formato no coincide con el de tu versión de Desktop; en ese caso se configura a mano (paso 14).
