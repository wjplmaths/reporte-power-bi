1. Renombrar Tabla 1 a Dim_Vendedores: quitar filas superiores, promover encabezados,
   quitar columnas no usadas y tipar.
2. Renombrar Tabla 2, 3, 4 y 5 a Metas_2020, Metas_2021, Metas_2022 y Metas_2023.
   En cada una: quitar filas superiores, promover encabezados, quitar "Columna 1",
   anular dinamización de todas las columnas excepto ID_Vendedor, renombrar a
   "Fecha" y "Meta" y tipar (Fecha = date, Meta = número). Si los encabezados de mes
   vienen como texto, conviértelos a fecha de forma robusta y documenta cómo.
3. Crear Dim_Metas anexando las cuatro consultas (Table.Combine) y deshabilitar la
   carga de Metas_2020..2023 (enableLoad = false / isHidden según TMDL).
4. Renombrar Tabla 6 a Dim_Territorios: quitar filas superiores e inferiores sin
   datos, promover encabezados, quitar columnas no usadas y tipar.
Aplica la convención de documentación.