# 27 – Mapa: León de Nicaragua en lugar de León (España)

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5, página Análisis General (mapa de ventas por departamento).

## Qué pasó
El mapa ubicaba "León" en España, porque el nombre de departamento solo no basta para que el servicio de mapas sepa de qué país es. El mapa quedaba en vista mundial y las burbujas de Nicaragua se amontonaban en una esquina.

## Qué se hizo (sin tocar el modelo de datos)
Solo se cambió el visual del mapa:
- **Ubicación:** se añadió `Dim_Clientes[País]` (siempre "Nicaragua") como primer campo del eje de ubicación, delante de `Departamento`. Con país y departamento, el servicio de mapas busca "León, Nicaragua". Es una columna que ya existe y que ya estaba en el modelo.
- **Cámara:** el mapa tenía un zoom mundial guardado (zoom 1.41, centro en el Atlántico). Se centró en Nicaragua: zoom 6, latitud 12.9, longitud -85.2.
- Tablas, relaciones, medidas y datos no cambian.

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/b5982da9636de7dfe6ac/visual.json` (mapa)
- `docs/changelog/27-mapa-leon-nicaragua.md`, `docs/CHANGELOG.md`, `prompts/27-mapa-leon-nicaragua.md`

Este commit incluye también cambios que Desktop dejó guardados en otros visuales.

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`; abrir Análisis General.
2. El mapa debe mostrar Nicaragua con una burbuja por departamento, y ninguna en España.
3. Pasar el cursor por la burbuja de León: debe mostrar las ventas de León (Nicaragua).

## Pendientes o riesgos
- No se probó en Desktop.
- Si León sigue saliendo en España: seleccionar `País` en la vista de datos > Herramientas de columna > Categoría de datos > País o región. Es un cambio de metadatos del modelo, que no se hizo para respetar la petición de no afectarlo.
- Si Desktop vuelve a guardar el zoom antiguo al mover el mapa, se vuelve a centrar a mano.