# Checklist del requerimiento

Fuente: `docs/requerimiento/GUIA DE TRABAJO FINAL.pdf`. Marcar `[x]` solo cuando esté hecho y validado en Power BI Desktop.

**Contexto:** informe de ventas nacionales 2020-2023 con control de cumplimiento de metas por vendedor, comportamiento por Año/Trimestre/Mes y comparativos con períodos anteriores.

## OE1 – Conexión a diferentes fuentes de datos
- [ ] Conectar al archivo xls `Fact_Ventas` (en `data/raw/` es `.xlsx`) — *consulta creada en pbip, falta validar en Desktop*
- [ ] Conectar al csv `Dim_Clientes` — *consulta creada en pbip, falta validar en Desktop*
- [ ] Conectar al csv `Dim_Productos` — *consulta creada en pbip, falta validar en Desktop*
- [ ] Conectar al archivo en línea G.Drive (Vendedores + Metas_Mensuales), importado como tipo **Web** — *consultas `Tabla 1` a `Tabla 5` creadas con `Web.Page`, falta validar en Desktop*
- [ ] Conectar al enlace Web de Wikipedia y extraer la tabla *Organización Territorial – Tabla 6* — *consulta `Tabla 6` creada eligiendo la tabla por contenido (`Table 5` daba error en Desktop), falta validar en Desktop*

## OE2 – Transformación de datos
### Fact_Ventas
- [ ] Eliminar filas superiores y colocar los encabezados correctos
- [ ] Eliminar columnas que no se usarán en el modelo
- [ ] Revisar el tipo de datos de cada columna

### Dim_Clientes
- [ ] Eliminar ID de clientes `null`
- [ ] Reemplazar Departamento "Región Autónoma de la Costa Caribe Norte" por "Costa Caribe Norte"
- [ ] Reemplazar Departamento "Región Autónoma de la Costa Caribe Sur" por "Costa Caribe Sur"
- [ ] Revisar el tipo de datos de cada columna

### Dim_Productos
- [ ] Eliminar ID `null` (el PDF dice "ID de clientes"; se interpreta como ID Producto)
- [ ] Revisar el tipo de datos de cada columna

### Tabla 1 → Dim_Vendedores
- [ ] Renombrar la consulta a `Dim_Vendedores`
- [ ] Eliminar filas superiores y colocar los encabezados correctos
- [ ] Eliminar columnas que no se usarán
- [ ] Revisar el tipo de datos de cada columna

### Tablas 2 a 5 → Metas_2020, Metas_2021, Metas_2022, Metas_2023
Para cada una de las cuatro consultas:
- [ ] `Metas_2020`: renombrar, quitar filas superiores y poner encabezados, quitar Columna 1, anular dinamización desde `ID_Vendedor`, renombrar columnas a `Fecha` y `Meta`, ajustar tipos
- [ ] `Metas_2021`: mismos pasos
- [ ] `Metas_2022`: mismos pasos
- [ ] `Metas_2023`: mismos pasos

### Consulta consolidada
- [ ] Anexar consultas como nueva a partir de `Metas_2020` para consolidar todas las metas
- [ ] Renombrar la consulta consolidada a `Dim_Metas`
- [ ] Deshabilitar "Habilitar carga" en `Metas_2020`, `Metas_2021`, `Metas_2022` y `Metas_2023`

### Tabla 6 → Dim_Territorios
- [ ] Renombrar la consulta a `Dim_Territorios`
- [ ] Eliminar filas superiores y colocar los encabezados correctos
- [ ] Eliminar filas inferiores sin datos útiles
- [ ] Eliminar columnas que no se usarán
- [ ] Revisar el tipo de datos de cada columna

### Cierre
- [ ] Cerrar y aplicar todos los cambios

## OE3 – Tabla Calendario + Modelado de datos
- [ ] Crear la tabla `Calendario` con "Nueva tabla" (DAX), del 2020/1/1 al 2023/12/31
- [ ] Columna `Año`
- [ ] Columna `Trimestre`
- [ ] Columna `Mes_Num`
- [ ] Columna `Mes_Corto` (con `FORMAT`)
- [ ] Columna `Mes_Largo` (con `FORMAT`)
- [ ] Verificar relaciones y modelado correctos entre todas las consultas, incluyendo `Calendario`

## OE4 – DAX
- [ ] Crear la consulta/tabla contenedora `Medidas`
- [ ] `Total Facturado` = Cantidad * Precio Unitario (`SUMX`)
- [ ] `Costo Total` = Cantidad * Costo Unitario (`SUMX`, `RELATED`)
- [ ] `Utilidad Total` = Total Facturado − Costo Total
- [ ] `% Cumplimiento metas` = Total Facturado / Meta (`DIVIDE`)
- [ ] `Meta` = `SumMetas`
- [ ] `Ventas del Año Anterior` (`SAMEPERIODLASTYEAR`)

## OE5 – Visualizaciones + Compartir informe
### Página de inicio
- [ ] Portada con nombre del autor y título del informe
- [ ] Botón de navegación hacia la primera página de Análisis General
- [ ] Logo de preferencia

### Página de Análisis General
- [ ] Tarjetas (nuevas): Facturación Total, Costo Total y Utilidad
- [ ] (Plus) Imágenes png en las tarjetas
- [ ] Mapa con Total Facturado por Departamento
- [ ] Tabla con el cumplimiento de ventas de cada vendedor
- [ ] Formato condicional en `% Cumplimiento`: menor a 60% rojo, 60% a 79% amarillo, mayor a 80% verde
- [ ] Medidor: Total Facturado vs Meta, con etiqueta personalizada en el valor que muestre el % de avance
- [ ] Gráfico de barras agrupadas: Top 5 clientes por facturación
- [ ] Chiclet Slicer (AppSource) por Año
- [ ] Chiclet Slicer (AppSource) por Mes Corto, con `Mes_Corto` ordenada por `Mes_Num`

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
