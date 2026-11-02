Crea la tabla contenedora "Medidas" (tabla calculada con una columna oculta o
tabla vacía) y estas medidas con formato y carpeta de visualización:
- Total Facturado = SUMX(Fact_Ventas, Cantidad * Precio Unitario)
- Costo Total = SUMX(Fact_Ventas, Cantidad * RELATED(Dim_Productos[Costo Unitario]))
- Utilidad Total = [Total Facturado] - [Costo Total]
- Meta = SUMMETAS: suma de Dim_Metas[Meta]
- % Cumplimiento metas = DIVIDE([Total Facturado], [Meta])
- Ventas del Año Anterior = CALCULATE([Total Facturado], SAMEPERIODLASTYEAR(Calendario[Date]))
Usa los nombres reales de columnas que encuentres en el modelo (corrígelos si difieren).
Añade una medida de etiqueta de % de avance para el medidor. Documenta cada medida
con su descripción en TMDL. Aplica la convención de documentación.