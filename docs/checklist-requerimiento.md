# Checklist del requerimiento

Fuente: `docs/requerimiento/GUIA DE TRABAJO FINAL.pdf`. Marcar `[x]` solo cuando esté hecho y validado en Power BI Desktop.

**Contexto:** informe de ventas nacionales 2020-2023 con control de cumplimiento de metas por vendedor, comportamiento por Año/Trimestre/Mes y comparativos con períodos anteriores.

## OE1 – Conexión a diferentes fuentes de datos
- [ ] Conectar al archivo xls `Fact_Ventas` (en `data/raw/` es `.xlsx`) — *consulta creada en pbip, falta validar en Desktop*
- [ ] Conectar al csv `Dim_Clientes` — *consulta creada en pbip, falta validar en Desktop*
- [ ] Conectar al csv `Dim_Productos` — *consulta creada en pbip, falta validar en Desktop*
- [ ] Conectar al archivo en línea G.Drive (Vendedores + Metas_Mensuales), importado como tipo **Web** — *consultas `Tabla 1` a `Tabla 5` creadas con `Web.Page`; `Table N` daba error en Desktop, ahora se elige cada tabla por contenido; falta validar en Desktop*
- [ ] Conectar al enlace Web de Wikipedia y extraer la tabla *Organización Territorial – Tabla 6* — *consulta `Tabla 6` creada eligiendo la tabla por contenido (`Table 5` daba error en Desktop), falta validar en Desktop*

## OE2 – Transformación de datos
### Fact_Ventas
- [ ] Eliminar filas superiores y colocar los encabezados correctos — *aplicado en M (changelog 07), falta validar en Desktop*
- [ ] Eliminar columnas que no se usarán en el modelo — *aplicado en M (changelog 07), falta validar en Desktop*
- [ ] Revisar el tipo de datos de cada columna — *aplicado en M (changelog 07), falta validar en Desktop*

### Dim_Clientes
- [ ] Eliminar ID de clientes `null` — *aplicado en M (changelog 07), falta validar en Desktop*
- [ ] Reemplazar Departamento "Región Autónoma de la Costa Caribe Norte" por "Costa Caribe Norte" — *aplicado en M (changelog 07), falta validar en Desktop*
- [ ] Reemplazar Departamento "Región Autónoma de la Costa Caribe Sur" por "Costa Caribe Sur" — *aplicado en M (changelog 07), falta validar en Desktop*
- [ ] Revisar el tipo de datos de cada columna — *aplicado en M (changelog 07), falta validar en Desktop*

### Dim_Productos
- [ ] Eliminar ID `null` (el PDF dice "ID de clientes"; se interpreta como ID Producto) — *aplicado en M (changelog 07), falta validar en Desktop*
- [ ] Revisar el tipo de datos de cada columna — *aplicado en M (changelog 07), falta validar en Desktop*

### Tabla 1 → Dim_Vendedores
- [ ] Renombrar la consulta a `Dim_Vendedores` — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] Eliminar filas superiores y colocar los encabezados correctos — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] Eliminar columnas que no se usarán — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] Revisar el tipo de datos de cada columna — *aplicado en M (changelog 09), falta validar en Desktop*

### Tablas 2 a 5 → Metas_2020, Metas_2021, Metas_2022, Metas_2023
Para cada una de las cuatro consultas:
- [ ] `Metas_2020`: renombrar, quitar filas superiores y poner encabezados, quitar Columna 1, anular dinamización desde `ID_Vendedor`, renombrar columnas a `Fecha` y `Meta`, ajustar tipos — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] `Metas_2021`: mismos pasos — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] `Metas_2022`: mismos pasos — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] `Metas_2023`: mismos pasos — *aplicado en M (changelog 09), falta validar en Desktop*

### Consulta consolidada
- [ ] Anexar consultas como nueva a partir de `Metas_2020` para consolidar todas las metas — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] Renombrar la consulta consolidada a `Dim_Metas` — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] Deshabilitar "Habilitar carga" en `Metas_2020`, `Metas_2021`, `Metas_2022` y `Metas_2023` — *aplicado en M (changelog 09), falta validar en Desktop*

### Tabla 6 → Dim_Territorios
- [ ] Renombrar la consulta a `Dim_Territorios` — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] Eliminar filas superiores y colocar los encabezados correctos — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] Eliminar filas inferiores sin datos útiles — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] Eliminar columnas que no se usarán — *aplicado en M (changelog 09), falta validar en Desktop*
- [ ] Revisar el tipo de datos de cada columna — *aplicado en M (changelog 09), falta validar en Desktop*

### Cierre
- [ ] Cerrar y aplicar todos los cambios

## OE3 – Tabla Calendario + Modelado de datos
- [ ] Crear la tabla `Calendario` con "Nueva tabla" (DAX), del 2020/1/1 al 2023/12/31 — *aplicado en TMDL (changelog 11), falta validar en Desktop*
- [ ] Columna `Año` — *aplicado en TMDL (changelog 11), falta validar en Desktop*
- [ ] Columna `Trimestre` — *aplicado en TMDL (changelog 11), falta validar en Desktop*
- [ ] Columna `Mes_Num` — *aplicado en TMDL (changelog 11), falta validar en Desktop*
- [ ] Columna `Mes_Corto` (con `FORMAT`) — *aplicado en TMDL (changelog 11), falta validar en Desktop*
- [ ] Columna `Mes_Largo` (con `FORMAT`) — *aplicado en TMDL (changelog 11), falta validar en Desktop*
- [ ] Verificar relaciones y modelado correctos entre todas las consultas, incluyendo `Calendario` — *7 relaciones definidas en TMDL (changelog 13), falta validar en Desktop*

## OE4 – DAX
- [ ] Crear la consulta/tabla contenedora `Medidas` — *consulta M en blanco (changelog 15), falta validar en Desktop*
- [ ] `Total Facturado` = Cantidad * Precio Unitario (`SUMX`) — *aplicado en TMDL (changelog 14), falta validar en Desktop*
- [ ] `Costo Total` = Cantidad * Costo Unitario (`SUMX`, `RELATED`) — *aplicado en TMDL (changelog 14), falta validar en Desktop*
- [ ] `Utilidad Total` = Total Facturado − Costo Total — *aplicado en TMDL (changelog 14), falta validar en Desktop*
- [ ] `% Cumplimiento metas` = Total Facturado / Meta (`DIVIDE`) — *aplicado en TMDL (changelog 14), falta validar en Desktop*
- [ ] `Meta` = `SumMetas` — *aplicado en TMDL (changelog 14), falta validar en Desktop*
- [ ] `Ventas del Año Anterior` (`SAMEPERIODLASTYEAR`) — *aplicado en TMDL (changelog 14), falta validar en Desktop*

## OE5 – Visualizaciones + Compartir informe
### Página de inicio
- [ ] Portada con nombre del autor y título del informe — *creado en PBIR (changelog 17), falta validar en Desktop*
- [ ] Botón de navegación hacia la primera página de Análisis General — *creado en PBIR (changelog 17), falta validar en Desktop*
- [ ] Logo de preferencia — *creado en PBIR (changelog 17), falta validar en Desktop*

### Página de Análisis General
- [ ] Tarjetas (nuevas): Facturación Total, Costo Total y Utilidad — *creado en PBIR (changelog 18), falta validar en Desktop*
- [ ] (Plus) Imágenes png en las tarjetas — *creado en PBIR (changelog 18), falta validar en Desktop*
- [ ] Mapa con Total Facturado por Departamento — *creado en PBIR (changelog 18), falta validar en Desktop*
- [ ] Tabla con el cumplimiento de ventas de cada vendedor — *creado en PBIR (changelog 18), falta validar en Desktop*
- [ ] Formato condicional en `% Cumplimiento`: menor a 60% rojo, 60% a 79% amarillo, mayor a 80% verde — *íconos nativos X / ! / ✔ en PBIR (changelog 22), falta validar en Desktop*
- [ ] Medidor: Total Facturado vs Meta, con etiqueta personalizada en el valor que muestre el % de avance — *medidor con medida `Etiqueta % Avance` en tarjeta bajo el medidor y en su tooltip (changelog 21); la etiqueta dentro del globo es un paso manual*
- [ ] Gráfico de barras agrupadas: Top 5 clientes por facturación — *creado en PBIR (changelog 18), falta validar en Desktop*
- [ ] Chiclet Slicer (AppSource) por Año — *creado en PBIR con el identificador real del visual (changelog 20), falta validar en Desktop*
- [ ] Chiclet Slicer (AppSource) por Mes Corto, con `Mes_Corto` ordenada por `Mes_Num` — *creado en PBIR con el identificador real del visual (changelog 20), falta validar en Desktop*

### Página de Análisis de Tendencia
- [ ] Tarjetas (nuevas): Facturación Total y Facturación Total del Año Anterior
- [ ] KPI: ventas actuales vs ventas del período anterior
- [ ] Gráfico de columnas agrupadas y líneas: Meta vs Total Facturado por mes, con línea de % de cumplimiento
- [ ] Eje X con jerarquía Año-Trimestre-Mes
- [ ] Reutilizar las segmentaciones de la página anterior

### Entrega
- [ ] Publicar y compartir el enlace del proyecto en el grupo de trabajo
- [ ] Subir el archivo .pbix a Classroom

## Notas
- El PDF define "mayor al 80%" en verde y "60% a 79%" en amarillo; el tramo 79%–80% queda sin regla. Decidir el borde al implementar el formato condicional.
- Los enlaces a las fuentes en línea están en `docs/requerimiento/*.txt`.
