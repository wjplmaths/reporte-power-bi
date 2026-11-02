# 06 – Se quita el parámetro RutaBase

**Fecha:** 2026-11-01
**Objetivo atendido:** OE1 (conexión a diferentes fuentes de datos).

## Qué se hizo
- Se eliminó el parámetro `RutaBase` de `expressions.tmdl` y su `ref expression` de `model.tmdl`.
- Las tres consultas locales (`Fact_Ventas`, `Dim_Clientes`, `Dim_Productos`) ahora llevan la ruta completa de su archivo dentro de la propia consulta.
- Power Query no admite rutas relativas, así que sin parámetro la ruta tiene que ir escrita en cada consulta.
- Las consultas web y `fnBuscarTabla` no cambian.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/expressions.tmdl`, `definition/model.tmdl`
- `pbip/Ventas.SemanticModel/definition/tables/{Fact_Ventas,Dim_Clientes,Dim_Productos}.tmdl`
- `docs/modelo/diccionario.md`, `docs/CHANGELOG.md`, `prompts/06-quitar-rutabase.md`

## Código clave
```m
Origen = Excel.Workbook(File.Contents("C:\Users\wiljo\OneDrive\Escritorio\Trabajo final de Power BI\data\raw\Fact_Ventas.xlsx"), null, true)
Origen = Csv.Document(File.Contents("C:\Users\wiljo\OneDrive\Escritorio\Trabajo final de Power BI\data\raw\Dim_Clientes.csv"), [Delimiter=",", Columns=5, Encoding=65001, QuoteStyle=QuoteStyle.None])
```

## Cómo validar en Power BI Desktop
1. Cerrar y reabrir `pbip/Ventas.pbip`.
2. En Transformar datos, `Fact_Ventas`, `Dim_Clientes` y `Dim_Productos` deben cargar sin error. Ya no aparece la consulta `RutaBase`.

## Pendientes o riesgos
- Si la carpeta del proyecto se mueve o se abre en otro equipo, hay que cambiar la ruta en las tres consultas (Editor avanzado de cada una). Con el parámetro bastaba cambiar un sitio.
- Desktop puede pedir de nuevo credenciales o niveles de privacidad de las fuentes, porque las rutas cambian de origen.