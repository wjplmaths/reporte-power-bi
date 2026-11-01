En el modelo semántico (pbip/Ventas.SemanticModel) crea las consultas Power Query (M)
de conexión sin transformar aún:
- Fact_Ventas desde data/raw/Fact_Ventas.xls (Excel.Workbook)
- Dim_Clientes y Dim_Productos desde CSV (Csv.Document, UTF-8, delimitador según el archivo)
- Google Drive (Vendedores + Metas_Mensuales): usar Web.BrowserContents o Web.Page
  sobre esta URL: <https://docs.google.com/spreadsheets/d/1BbiT3b81T7sEzpAFW7_ks5qU07rEeHal/edit?gid=1022144234#gid=1022144234>. Debe quedar como origen tipo Web y exponer
  Table 1 a Table 5 (Vendedores y metas 2020-2023).
- Wikipedia: <https://es.wikipedia.org/wiki/Nicaragua>, extrayendo la Tabla 5 (Organización Territorial).
Usa un parámetro de M para la ruta base de data/raw para que sea fácil de cambiar.
Aplica la convención de documentación.