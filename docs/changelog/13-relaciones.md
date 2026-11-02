# 13 – Relaciones del modelo

**Fecha:** 2026-11-01
**Objetivo atendido:** OE3 (modelado de datos).

## Qué se hizo
- Se definieron 7 relaciones en `relationships.tmdl`, todas de muchos a uno, con filtro en una sola dirección y activas:
  - `Fact_Ventas` con `Dim_Clientes`, `Dim_Productos`, `Dim_Vendedores` y `Calendario`.
  - `Dim_Metas` con `Dim_Vendedores` y `Calendario`.
  - `Dim_Clientes` con `Dim_Territorios`, por `Departamento`.
- Se ocultaron las columnas de llaves en el panel de campos (lista en `docs/modelo/diagrama-relaciones.md`).
- `Departamento` en `Dim_Clientes` y `Dim_Territorios` tiene la categoría de datos "Estado o provincia", para el mapa.
- Se generó el diagrama del modelo en Mermaid: `docs/modelo/diagrama-relaciones.md`.

## Sobre la imagen de contexto
El diagrama que mostró el usuario tiene las mismas 7 relaciones. Se diferencia en columnas que el modelo actual ya no tiene: `Fecha de Envío`, `ID Factura` y `Tasa de Impuesto` en `Fact_Ventas` (quitadas en el changelog 07) y `Primer Nombre` en `Dim_Vendedores` (quitada en el 09). En el diagrama de la imagen, `Dim_Metas` es el único con `ID_vendedor` en minúscula; aquí se estandarizó a `ID_Vendedor`.

## Cardinalidad y dirección del filtro
En TMDL, una relación sin propiedades extra es de muchos a uno, con filtro unidireccional y activa. Por eso las 7 se escribieron sin propiedades adicionales. Todas tienen la columna "muchos" en `fromColumn` y la columna "uno" en `toColumn`.
- Los lados "uno" tienen valores únicos: 442 clientes, 227 productos, 19 vendedores, 1 461 fechas y 17 departamentos.
- `Dim_Metas` tiene una fila por vendedor y mes (912 filas esperadas), así que sus dos relaciones son de muchos a uno.

## Ambigüedad de fecha en Fact_Ventas
No hay ambigüedad: `Fecha de Envío` se eliminó de `Fact_Ventas` en la transformación (changelog 07), así que `Fecha de Venta` es la única relación con `Calendario`. Si más adelante se recupera `Fecha de Envío`, tendría que ir con una relación inactiva.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/relationships.tmdl`
- `pbip/Ventas.SemanticModel/definition/tables/{Fact_Ventas,Dim_Clientes,Dim_Productos,Dim_Vendedores,Dim_Metas,Dim_Territorios}.tmdl` (columnas ocultas y categoría de datos)
- `docs/modelo/diagrama-relaciones.md` (nuevo), `docs/modelo/diccionario.md`, `docs/checklist-requerimiento.md`
- `docs/changelog/13-relaciones.md`, `docs/CHANGELOG.md`, `prompts/13-relaciones.md`

Este commit incluye también los cambios que Desktop dejó guardados en `Calendario.tmdl`, `model.tmdl` y `diagramLayout.json`, y un cambio en `visual.json` de la portada.

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`.
2. En la vista de modelo deben verse las 7 relaciones (`*` hacia `1`), todas con línea continua. El resultado debe parecerse a la imagen de contexto.
3. Si alguna sale con línea punteada o con aviso, revisar:
   - IDs de `Fact_Ventas[ID Cliente]` que no existen en `Dim_Clientes`: máximo 713 frente a 442 clientes.
   - Nombres de departamento de `Dim_Clientes` que no coinciden con `Dim_Territorios`.
4. En el panel de campos no deben verse los IDs, y `Departamento` debe tener el icono de ubicación.

## Pendientes o riesgos
- No se probó en Desktop.
- **Huérfanos:** si hay clientes en `Fact_Ventas` que no están en `Dim_Clientes` (no se verificó), las ventas de esos clientes saldrán con el departamento en blanco. Es un problema de los datos, no de la relación.
- **Departamento:** los nombres de `Dim_Clientes` y `Dim_Territorios` deben coincidir letra por letra (acentos incluidos) para que el filtro funcione.
- La relación automática de `Fecha de Alta` y las tablas `LocalDateTable_*` siguen ahí; se pueden quitar desactivando "Fecha/hora automática" en Opciones, un paso manual.