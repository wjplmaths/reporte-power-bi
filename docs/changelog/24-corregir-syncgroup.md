# 24 – Corrección de syncGroup (error al abrir el informe)

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5. Corrige el changelog 23.

## Qué pasó
Al abrir el informe, Desktop mostró "El informe tiene problemas que no se pudieron resolver" (4 problemas): `Se ha incluido una propiedad 'syncGroup' adicional en la propiedad root de visuals/.../visual.json`.

En el changelog 23 puse `syncGroup` en la raíz del archivo de cada Chiclet Slicer, y ahí no está permitido. Fue un error mío: no verifiqué dónde va esa propiedad.

## Qué se hizo
Se movió `syncGroup` dentro del bloque `visual` en los 4 Chiclet (2 de Análisis General y 2 de Análisis de Tendencia). Contenido sin cambios: `groupName` (`Año` o `Mes_Corto`), `fieldChanges` y `filterChanges` en `true`.

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/{3321249c81bd226877bd,dc3a8f8200499969ebf4}/visual.json`
- `pbip/Ventas.Report/definition/pages/AnalisisTendencia/visuals/{9d8786ab7b4ad3055b92,10d3e50a6525ddc4d538}/visual.json`
- `docs/changelog/24-corregir-syncgroup.md`, `docs/CHANGELOG.md`, `prompts/24-corregir-syncgroup.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`: ya no debe salir el aviso.
2. Elegir un año en una página y pasar a la otra: el año debe seguir seleccionado.

## Pendientes o riesgos
- No se probó en Desktop.
- Si Desktop vuelve a protestar, copiar el detalle del error: ya conoce el lugar correcto, pero las propiedades `fieldChanges` y `filterChanges` se escribieron de memoria.