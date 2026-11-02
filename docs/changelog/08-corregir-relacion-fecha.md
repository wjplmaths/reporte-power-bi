# 08 – Corrección del error de relación al abrir el proyecto

**Fecha:** 2026-11-01
**Objetivo atendido:** OE2 (transformación de datos). Corrige el cambio del paso 07.

## Qué pasó
Al abrir `Ventas.pbip`, Desktop mostró: `Property FromColumn of object "relationship 0495765e-..." refers to an object which cannot be found`.

La causa fue un error mío en el paso 07:
- Desktop había guardado las tablas con sus columnas (`column ...`) y una relación automática de fecha, `Dim_Clientes[Fecha de Alta]` hacia `LocalDateTable`, en `relationships.tmdl`.
- Al aplicar las transformaciones, reescribí `Dim_Clientes.tmdl`, `Dim_Productos.tmdl` y `Fact_Ventas.tmdl` desde cero y borré esas columnas.
- La relación quedó apuntando a `Dim_Clientes.'Fecha de Alta'`, que ya no existía.
- No leí los archivos antes de reescribirlos, que es lo que pide `CLAUDE.md`.

## Qué se hizo
- `Dim_Clientes.tmdl` y `Dim_Productos.tmdl`: se restauraron las columnas, la variación de fecha y las anotaciones que había guardado Desktop. Solo se reemplazó la consulta M de la partición por la del paso 07.
- `Fact_Ventas.tmdl`: Desktop tenía 11 columnas genéricas (`Column1` a `Column11`) que ya no existen tras la transformación. Se declararon las 7 columnas reales con sus tipos: 4 IDs enteros, `Fecha de Venta` como fecha, `Cantidad` entero y `Precio Unitario` decimal.
- `relationships.tmdl` y las tablas `LocalDateTable` no se tocaron.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/{Fact_Ventas,Dim_Clientes,Dim_Productos}.tmdl`
- `docs/changelog/08-corregir-relacion-fecha.md`, `docs/CHANGELOG.md`, `prompts/08-corregir-relacion-fecha.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`: ya no debe salir el error.
2. En Transformar datos, `Fact_Ventas` debe mostrar solo las 7 columnas, `Dim_Clientes` 5 y `Dim_Productos` 9.
3. Cerrar y aplicar, y revisar que no haya errores de actualización.

## Pendientes o riesgos
- No se probó en Desktop. Las columnas de `Fact_Ventas` se escribieron a mano; si Desktop encuentra una diferencia de tipo, la corrige al actualizar.
- `Fact_Ventas[Fecha de Venta]` no tiene jerarquía automática de fecha. No importa: en OE3 se relaciona con `Calendario`.
- Lección para próximos pasos: antes de editar un `.tmdl`, leer su contenido actual y conservar sus columnas.