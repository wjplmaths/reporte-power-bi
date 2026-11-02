# 17 – Página Inicio (portada)

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones), página de inicio.

## Qué se hizo
- La página `Portada` (que solo tenía un cuadro de texto de prueba con "qew") pasó a ser **Inicio**. Se reutilizó la estructura JSON de ese cuadro de texto para los textos nuevos.
- Se creó la página **Análisis General**, vacía, que es el destino del botón. Se llenará en los siguientes pasos de OE5.
- Visuales de `Inicio`, en una página de 1280 × 720:
  - **Logo:** imagen `assets/icono-ventas.jpeg`, copiada como recurso del informe.
  - **Título:** "Informe de Ventas Nacionales 2020-2023" (36 pt, negrita).
  - **Autor:** "Wilmer José Palacios López" (22 pt).
  - **Botón** "Ir al Análisis General": navega a la página Análisis General.
- `pages.json`: orden Inicio, Análisis General; página activa Inicio.

## Decisiones
- Logo: en `assets/` solo hay dos íconos (`icono-ventas.jpeg` e `icono-facturacion.jpeg`), pensados para las tarjetas. Se usó `icono-ventas.jpeg`; si prefieres otro logo, se cambia el archivo `StaticResources/RegisteredResources/logo.jpeg`.
- El título del informe es de mi cosecha ("Informe de Ventas Nacionales 2020-2023"), a partir del contexto de la guía. Se puede cambiar.
- Se eliminó la carpeta `Portada`, incluido el cuadro de texto de prueba "qew".

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/pages.json`
- `pbip/Ventas.Report/definition/pages/Inicio/page.json` y 4 `visuals/*/visual.json` (logo, título, autor, botón)
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/page.json`
- `pbip/Ventas.Report/definition/report.json` (recurso registrado `logo.jpeg`)
- `pbip/Ventas.Report/StaticResources/RegisteredResources/logo.jpeg`
- Eliminado: `definition/pages/Portada/`
- `docs/checklist-requerimiento.md`, `docs/changelog/17-pagina-inicio.md`, `docs/CHANGELOG.md`, `prompts/17-pagina-inicio.md`

Este commit incluye también cambios que Desktop dejó guardados en `Medidas.tmdl` y `diagramLayout.json`.

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. El informe debe tener 2 pestañas: Inicio y Análisis General, y abrir en Inicio.
3. En Inicio: logo arriba al centro, título, tu nombre y el botón azul oscuro.
4. Ctrl + clic en el botón: debe llevar a Análisis General.

## Pendientes o riesgos
- No se probó en Desktop. Es lo más delicado hasta ahora: el formato del informe (PBIR) se escribió de memoria.
  - El recurso de imagen (`ResourcePackageItem`) y el botón de navegación (`visualLink` con `PageNavigation`) son los puntos con más riesgo.
  - Si Desktop da error al abrir el informe, o el logo o el botón no aparecen, pégame el mensaje y lo corrijo.
- Plan B si el logo no carga: insertarlo a mano en Desktop (Insertar, Imagen), tarda un minuto.
- Los estilos (colores, tamaños) son sencillos; el tema final se puede ajustar en Desktop.