# 19 – Corrección de Análisis General (segmentaciones, íconos y tamaños)

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones). Corrige el changelog 18 a partir de la captura del usuario.

## Qué pasó
En Desktop, la página Análisis General cargó bien las tarjetas, el mapa, la tabla, el medidor y las barras Top 5. Fallaron cuatro cosas:
- **Las dos segmentaciones Chiclet** (fila superior) no se veían. Desktop mostraba "Hay un problema con el objeto visual". Los campos `Año` y `Mes_Corto` sí estaban enlazados (aparecen en el panel de filtros), así que el fallo es del visual personalizado, no de los datos.
- **Los dos íconos** (junto a Total Facturado y Utilidad Total) se veían como imagen rota. Fue un error mío: al llamar a la función que crea los íconos pasé mal los parámetros, y el nombre del recurso (`ItemName`) quedó vacío.
- **Las tarjetas** cortaban las cifras (`C$201....`).
- **La tabla** dejaba la columna `% Cumplimiento` cortada a la derecha.

## Qué se hizo
- **Íconos:** se escribió el `ItemName` correcto en cada imagen y se volvió a registrar `icono-ventas.jpeg` en `report.json` (Desktop lo había quitado por no tener referencia válida).
- **Segmentaciones:** se sustituyeron los Chiclet por segmentaciones nativas en horizontal, para que la página funcione ya. No se pudo averiguar por qué falla el Chiclet; se deja como paso manual (abajo). Se quitó `publicCustomVisuals` de `report.json`.
- **Tarjetas:** letra de 26 pt y valores en millones (por ejemplo `C$201.69 mill.`).
- **Mapa y tabla:** el mapa se estrechó a 380 px y la tabla se ensanchó a 530 px (empieza en x = 410).
- Lo que Desktop ya había guardado bien no se tocó: formato condicional por color de la tabla, Top 5, medidor.

## Paso manual: pasar las segmentaciones a Chiclet Slicer
La guía pide "Chiclet Slicer". En Desktop:
1. Seleccionar una segmentación (Año o Mes) y, en el panel Visualizaciones, hacer clic en el icono de **Chiclet Slicer**. Los campos se conservan.
2. Repetir con la otra. Guardar el proyecto.
3. Avisarme: leeré el JSON que Desktop guarde y lo usaré como plantilla (así se resuelve por qué falló el mío).
4. Comprobar que los meses salen ENE, FEB, MAR... Si salen alfabéticos: vista de datos > `Mes_Corto` > Ordenar por columna > `Mes_Num`.

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/*/visual.json` (íconos, segmentaciones, tarjetas, mapa, tabla)
- `pbip/Ventas.Report/definition/report.json`, `pages/pages.json` (cambios que dejó Desktop)
- `docs/checklist-requerimiento.md`, `docs/changelog/19-analisis-general-correcciones.md`, `docs/CHANGELOG.md`, `prompts/19-analisis-general-correcciones.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`; abrir Análisis General.
2. Los íconos de facturación y ventas deben verse junto a sus tarjetas.
3. Las tarjetas deben mostrar cifras en millones sin cortarse.
4. La tabla debe mostrar la columna `% Cumplimiento metas` completa, con los colores por rango.
5. En la fila superior deben verse las segmentaciones de año y mes. Al elegir un año, todos los visuales deben filtrarse.

## Pendientes o riesgos
- No se probó en Desktop.
- El formato de las tarjetas (millones, tamaño) y la orientación horizontal de las segmentaciones se escribieron sin plantilla; si Desktop no los aplica, se ajustan a mano en Formato.
- El Chiclet Slicer sigue pendiente (paso manual de arriba).
- La etiqueta personalizada del medidor y los íconos de la tabla siguen siendo pasos manuales (changelog 18).