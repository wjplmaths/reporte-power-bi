# 10 – Revisión de Dim_Territorios con la tabla real de Wikipedia

**Fecha:** 2026-11-01
**Objetivo atendido:** OE2 (transformación de datos), `Dim_Territorios`.

## Qué se revisó
El usuario mostró dos imágenes: la tabla de Wikipedia y el requisito de la guía para la consulta Tabla 6 (renombrar a `Dim_Territorios`, quitar filas superiores e inferiores, quitar columnas no usadas y revisar tipos).

La tabla real tiene 6 columnas: `Departamento` (con la bandera), `ISO:NI`, `Cabecera`, `Población (2023)`, `Superficie (km²)` y `Densidad`. Tiene 17 filas, de Boaco a Rivas. Es lo mismo que se esperaba, y el resultado final debe quedar así:

| Departamento | Cabecera | Población (2023) | Superficie (km²) |
|---|---|---|---|
| Boaco | Boaco | 188809 | 4177 |
| ... | ... | ... | ... |
| Rivas | Rivas | 185514 | 2162 |

## Qué se corrigió
Desktop había guardado la tabla con 7 columnas y no 6, así que probablemente la bandera es una columna aparte. La versión anterior habría elegido la primera columna que empezara por "Departamento", que podía ser la de la bandera, vacía. Entonces habría filtrado todas las filas y la tabla habría quedado vacía.

Cambios en la consulta:
- Si hay varias columnas con el mismo prefijo, se elige la que tiene más celdas con texto.
- Si no se encuentra una fila de encabezados, no se quita nada ni se promueve la primera fila.
- Se quitan las notas entre corchetes (por ejemplo `[3]`) en el texto y en las cifras.
- La fila inferior de nota se quita de dos formas: porque `Departamento` empieza por `*` y porque no tiene población.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/Dim_Territorios.tmdl`
- `docs/changelog/10-revisar-dim-territorios.md`, `docs/CHANGELOG.md`, `prompts/10-revisar-dim-territorios.md`

## Cómo validar en Power BI Desktop
1. Reabrir `pbip/Ventas.pbip` (o actualizar `Dim_Territorios`).
2. Debe mostrar 17 filas y 4 columnas: `Departamento`, `Cabecera`, `Población (2023)` (entero) y `Superficie (km²)` (decimal).
3. Managua debe tener población 1585800 y Masaya superficie 610.8. No debe quedar la fila "* Fuente...".
4. "Costa Caribe Norte" y "Costa Caribe Sur" deben aparecer así, igual que en `Dim_Clientes`.

## Pendientes o riesgos
- Sigue sin poder ejecutarse M desde aquí. Si la tabla queda vacía o da error, abre el paso `Datos` de la consulta y pega una captura de las columnas y sus primeras filas.
- Si `Departamento` incluye texto de la bandera (por ejemplo un texto alternativo), no coincidiría con `Dim_Clientes`. Se comprueba al relacionar en OE3.