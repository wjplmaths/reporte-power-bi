# Checklist del requerimiento

Fuente: `docs/requerimiento/GUIA DE TRABAJO FINAL.pdf`. Estados reales tras la reauditoría del 2026-11-01 tras las correcciones de los changelogs 35 y 36 (detalle, evidencias y citas en `docs/CHECKLIST_FINAL.md`).

**Leyenda:** `[x]` ✅ cumplido con evidencia en archivo · `[ ]` 🟡 parcial · ❌ pendiente · 🔍 por validar en Desktop · ➖ excluido por indicación del usuario.

**Contexto:** informe de ventas nacionales 2020-2023 con control de cumplimiento de metas por vendedor, comportamiento por Año/Trimestre/Mes y comparativos con períodos anteriores.

## OE1 – Conexión a diferentes fuentes de datos
- [x] ✅ OE1 · «Conectarse al archivo “xls” (Fact_Ventas)»
- [x] ✅ OE1 · «Conectarse a los archivos “csv” (Dim_Clientes)»
- [x] ✅ OE1 · «…y (Dim_Productos)»
- [x] ✅ OE1 · «Conectarse al archivo en línea “G.Drive” (Vendedores + Metas_Mensuales) ** Importar como tipo Web»
- [x] ✅ OE1 · «Conectarse al enlace Web “Wikipedia” (Extraer tabla: Organización Territorial – Tabla 6)»

## OE2 – Transformación de datos
- [x] ✅ OE2 · Consulta Fact_Ventas · «Elimine filas superiores y coloque los encabezados correctos»
- [x] ✅ OE2 · Fact_Ventas · «Elimine columnas que no usará en su modelo de datos»
- [x] ✅ OE2 · Fact_Ventas · «Revise que el tipo de datos sea el correcto en cada columna»
- [x] ✅ OE2 · Consulta Dim_Clientes · «Elimine ID de clientes “null”»
- [x] ✅ OE2 · Dim_Clientes · «Modifique el Departamento “Región Autónoma de la Costa Caribe Norte” por “Costa Caribe Norte”»
- [x] ✅ OE2 · Dim_Clientes · «…“Región Autónoma de la Costa Caribe Sur” por “Costa Caribe Sur”»
- [x] ✅ OE2 · Dim_Clientes · «Revise que el tipo de datos sea el correcto en cada columna»
- [x] ✅ OE2 · Consulta Dim_Productos · «Elimine ID de clientes “null”» (la guía dice “clientes”; se entiende ID Producto)
- [x] ✅ OE2 · Dim_Productos · «Revise que el tipo de datos sea el correcto en cada columna»
- [x] ✅ OE2 · Consulta Tabla 1 · «Cambie el nombre de la consulta por “Dim_Vendedores”»
- [x] ✅ OE2 · Tabla 1 · «Elimine filas superiores y coloque los encabezados correctos»
- [x] ✅ OE2 · Tabla 1 · «Elimine columnas que no usará en su modelo de datos»
- [x] ✅ OE2 · Tabla 1 · «Revise que el tipo de datos sea el correcto en cada columna»
- [x] ✅ OE2 · Consulta Tabla 2 · «Cambie el nombre de la consulta por “Metas_2020”»
- [x] ✅ OE2 · Consulta Tabla 2 · «Elimine filas superiores y coloque los encabezados correctos» (Metas_2020)
- [x] ✅ OE2 · Consulta Tabla 2 · «Elimine columnas que no usará (Columna 1)» (Metas_2020)
- [x] ✅ OE2 · Consulta Tabla 2 · «Anule la dinamización de otras columnas, a partir de la columna ID_Vendedor» (Metas_2020)
- [x] ✅ OE2 · Consulta Tabla 2 · «Cambie el nombre de las columnas por “Fecha” y “Meta”» (Metas_2020)
- [x] ✅ OE2 · Consulta Tabla 2 · «Cambie el tipo de datos de “Fecha” y “Meta”» (Metas_2020)
- [x] ✅ OE2 · Consulta Tabla 3 · «Cambie el nombre de la consulta por “Metas_2021”»
- [x] ✅ OE2 · Consulta Tabla 3 · «Elimine filas superiores y coloque los encabezados correctos» (Metas_2021)
- [x] ✅ OE2 · Consulta Tabla 3 · «Elimine columnas que no usará (Columna 1)» (Metas_2021)
- [x] ✅ OE2 · Consulta Tabla 3 · «Anule la dinamización de otras columnas, a partir de la columna ID_Vendedor» (Metas_2021)
- [x] ✅ OE2 · Consulta Tabla 3 · «Cambie el nombre de las columnas por “Fecha” y “Meta”» (Metas_2021)
- [x] ✅ OE2 · Consulta Tabla 3 · «Cambie el tipo de datos de “Fecha” y “Meta”» (Metas_2021)
- [x] ✅ OE2 · Consulta Tabla 4 · «Cambie el nombre de la consulta por “Metas_2022”»
- [x] ✅ OE2 · Consulta Tabla 4 · «Elimine filas superiores y coloque los encabezados correctos» (Metas_2022)
- [x] ✅ OE2 · Consulta Tabla 4 · «Elimine columnas que no usará (Columna 1)» (Metas_2022)
- [x] ✅ OE2 · Consulta Tabla 4 · «Anule la dinamización de otras columnas, a partir de la columna ID_Vendedor» (Metas_2022)
- [x] ✅ OE2 · Consulta Tabla 4 · «Cambie el nombre de las columnas por “Fecha” y “Meta”» (Metas_2022)
- [x] ✅ OE2 · Consulta Tabla 4 · «Cambie el tipo de datos de “Fecha” y “Meta”» (Metas_2022)
- [x] ✅ OE2 · Consulta Tabla 5 · «Cambie el nombre de la consulta por “Metas_2023”»
- [x] ✅ OE2 · Consulta Tabla 5 · «Elimine filas superiores y coloque los encabezados correctos» (Metas_2023)
- [x] ✅ OE2 · Consulta Tabla 5 · «Elimine columnas que no usará (Columna 1)» (Metas_2023)
- [x] ✅ OE2 · Consulta Tabla 5 · «Anule la dinamización de otras columnas, a partir de la columna ID_Vendedor» (Metas_2023)
- [x] ✅ OE2 · Consulta Tabla 5 · «Cambie el nombre de las columnas por “Fecha” y “Meta”» (Metas_2023)
- [x] ✅ OE2 · Consulta Tabla 5 · «Cambie el tipo de datos de “Fecha” y “Meta”» (Metas_2023)
- [x] ✅ OE2 · Creación de consulta consolidada · «Seleccione la consulta “Metas_2020”… Anexar consultas para crear una nueva»
- [x] ✅ OE2 · Consolidada · «Cambie el nombre por Dim_Metas»
- [x] ✅ OE2 · Consolidada · «Deshabilite la opción “Habilitar carga” en “Metas_2020”»
- [x] ✅ OE2 · Consolidada · «Deshabilite la opción “Habilitar carga” en “Metas_2021”»
- [x] ✅ OE2 · Consolidada · «Deshabilite la opción “Habilitar carga” en “Metas_2022”»
- [x] ✅ OE2 · Consolidada · «Deshabilite la opción “Habilitar carga” en “Metas_2023”»
- [x] ✅ OE2 · Consulta Tabla 6 · «Cambie el nombre de la consulta por “Dim_Territorios”»
- [ ] 🔍 OE2 · Tabla 6 · «Elimine filas superiores y coloque los encabezados correctos»
- [ ] 🔍 OE2 · Tabla 6 · «Elimine fila(s) inferiores que no contienen datos a usarse en el modelo»
- [ ] 🔍 OE2 · Tabla 6 · «Elimine columnas que no usará en su modelo de datos»
- [ ] 🔍 OE2 · Tabla 6 · «Revise que el tipo de datos sea el correcto en cada columna»
- [ ] 🔍 OE2 · «Cierre y aplique todos los cambios»

## OE3 – Tabla Calendario + Modelado de datos
- [x] ✅ OE3 · «Usando la opción “Nueva Tabla”, cree una tabla calendario… desde el (2020/1/1) hasta el (2023/12/31)»
- [x] ✅ OE3 · «…cree las columnas Año…»
- [x] ✅ OE3 · «…Trimestre»
- [x] ✅ OE3 · «…Mes_Num»
- [x] ✅ OE3 · «…Mes_Corto (haga uso de la función DAX Format)»
- [x] ✅ OE3 · «…Mes_Largo (haga uso de la función DAX Format)»
- [x] ✅ OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Fact_Ventas[ID Cliente] → Dim_Clientes[ID Cliente]
- [x] ✅ OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Fact_Ventas[ID Producto] → Dim_Productos[ID Producto]
- [x] ✅ OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Fact_Ventas[ID Vendedor] → Dim_Vendedores[ID Vendedor]
- [x] ✅ OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Fact_Ventas[Fecha de Venta] → Calendario[Date]
- [x] ✅ OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Dim_Metas[ID_Vendedor] → Dim_Vendedores[ID Vendedor]
- [x] ✅ OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Dim_Metas[Fecha] → Calendario[Date]
- [x] ✅ OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Dim_Clientes[Departamento] → Dim_Territorios[Departamento]
- [x] ✅ OE3 · «…modelado correcto» · integridad referencial (no la pide la guía, la verifica la auditoría)

## OE4 – DAX
- [x] ✅ OE4 · «Cree una nueva consulta llamada “Medidas” para agrupar las medidas a crear»
- [x] ✅ OE4 · «Utilice las funciones SumX, Related, Divide»
- [x] ✅ OE4 · «a. Total Facturado = Cantidad * Precio Unitario (SumX)»
- [x] ✅ OE4 · «b. Costo Total = Cantidad * Costo Unitario (SumX, Related)»
- [x] ✅ OE4 · «b. Utilidad Total = Total Facturado – Costo total»
- [x] ✅ OE4 · «c. % Cumplimiento metas = Total Facturado / SumMetas (Divide)»
- [x] ✅ OE4 · «d. Meta = SumMetas»
- [x] ✅ OE4 · «Utilice las funciones de inteligencia de tiempo SAMEPERIODLASTYEAR · a. Ventas del Año Anterior»

## OE5 – Visualizaciones + Compartir informe
- [x] ✅ OE5 · Página de inicio · «Crear una portada con su nombre»
- [x] ✅ OE5 · Inicio · «…título del informe»
- [x] ✅ OE5 · Inicio · «…y un botón de navegación que lleve a la primera página de Análisis General»
- [x] ✅ OE5 · Inicio · «Utilice el logo de su preferencia»
- [x] ✅ OE5 · Análisis General · Uso de Tarjetas · «Utilice “Tarjetas (nuevas)” para visualizar Facturación Total…»
- [x] ✅ OE5 · Tarjetas · «…Costo Total…»
- [x] ✅ OE5 · Tarjetas · «…y Utilidad»
- [ ] ❌ OE5 · Tarjetas · «**Plus (Puede agregar imágenes png para representar mejor las tarjetas)»
- [x] ✅ OE5 · Uso de Mapa · «Utilice un “Mapa” para representar las Ventas (Total Facturado) por Departamento»
- [x] ✅ OE5 · Uso de Tabla · «Utilice una “Tabla” para mostrar el cumplimiento de ventas de cada vendedor»
- [ ] 🔍 OE5 · Tabla · «formato condicional… en la columna de % de Cumplimiento» · «Menor al 60%, Rojo»
- [x] ✅ OE5 · Tabla · «Entre 60% al 79% Amarillo»
- [x] ✅ OE5 · Tabla · «Mayor al 80% Verde»
- [x] ✅ OE5 · Uso de Medidor · «Utilice la visualización “Medidor” para representar el avance total, relacionando el Total Facturado vs Meta»
- [ ] 🟡 OE5 · Medidor · «** Utilice una etiqueta personalizada en el valor del globo que represente el porcentaje de avance»
- [x] ✅ OE5 · Uso de Barras Agrupadas · «Utilice la visualización “Gráfico de barras agrupadas” para representar el Top 5 de clientes por facturación»
- [x] ✅ OE5 · Uso de Segmentaciones · «Utilice… “Chiclet Slicer” de AppSource… por Año»
- [x] ✅ OE5 · Segmentaciones · «…y Mes Corto»
- [x] ✅ OE5 · Segmentaciones · «Para ordenar la columna de mes_corto debe utilizar la opción ordenar por columna… por la columna mes_num»
- [x] ✅ OE5 · Análisis de Tendencia · Uso de Tarjetas · «Facturación Total»
- [x] ✅ OE5 · Tendencia · Tarjetas · «…y Facturación Total del Año Anterior»
- [ ] 🔍 OE5 · Uso de KPI · «Utilice la visualización “KPI” para visualizar un comparativo entre las ventas actuales y las ventas del período anterior»
- [x] ✅ OE5 · Uso de Columnas + Líneas · «Utilice la visualización “Gráfico de Columnas Agrupadas y Líneas” para visualizar un comparativo entre la meta y el total facturado por mes»
- [x] ✅ OE5 · Columnas + Líneas · «** En el caso de la línea, represente el % de cumplimiento»
- [x] ✅ OE5 · Columnas + Líneas · «** El gráfico debe contener una jerarquía de Año-Trimestre-Mes en su eje X»
- [x] ✅ OE5 · Uso de Segmentaciones (Tendencia) · «Reutilice las segmentaciones de la página anterior»
- [ ] ➖ OE5 · «Publique y comparta el enlace de su proyecto en el grupo de trabajo»
- [ ] ❌ OE5 · «El archivo Pbix deberá subirse a la plataforma Classroom»

## Resumen

| Objetivo | Evaluados | ✅ | 🟡 | 🔍 | ❌ | % cumplido |
|---|---:|---:|---:|---:|---:|---:|
| OE1 | 5 | 5 | 0 | 0 | 0 | 100 % |
| OE2 | 49 | 44 | 0 | 5 | 0 | 90 % |
| OE3 | 14 | 14 | 0 | 0 | 0 | 100 % |
| OE4 | 8 | 8 | 0 | 0 | 0 | 100 % |
| OE5 | 27 | 22 | 1 | 2 | 2 | 81 % |
| **Total** | 103 | 93 | 1 | 7 | 2 | **90 %** |

## Notas
- El PDF define “mayor al 80%” en verde y “60% a 79%” en amarillo; la imagen del cuadro de íconos fija los límites en 0.6 y 0.8 (>=0.8 verde). El modelo usa esos límites.
- El PDF dice “ID de clientes” en la viñeta de Dim_Productos; se entiende ID Producto.
- Los enlaces a las fuentes en línea están en `docs/requerimiento/*.txt`.
