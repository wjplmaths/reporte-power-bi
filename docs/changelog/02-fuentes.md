# 02 – Consultas de conexión a las fuentes (sin transformar)

**Fecha:** 2026-11-01
**Objetivo atendido:** OE1 (conexión a diferentes fuentes de datos).

## Qué se hizo
- Se creó el esqueleto del proyecto PBIP: `Ventas.pbip`, el modelo semántico (TMDL) y un informe mínimo con una página en blanco ("Portada"). El informe es necesario para que Desktop abra el `.pbip`.
- Se escribieron las 9 consultas de conexión, sin ninguna transformación.
- Se creó el parámetro `RutaBase` con la ruta de `data/raw`.
- Las URLs de G.Drive y Wikipedia se tomaron de `docs/requerimiento/*.txt`.
- No se declararon columnas en las tablas: los encabezados reales de `Fact_Ventas` y de las tablas web se arreglan en OE2.

## Archivos creados/modificados
- `pbip/Ventas.pbip`
- `pbip/Ventas.SemanticModel/definition.pbism`
- `pbip/Ventas.SemanticModel/definition/{database,model,expressions}.tmdl`
- `pbip/Ventas.SemanticModel/definition/tables/{Fact_Ventas,Dim_Clientes,Dim_Productos,Tabla 1..Tabla 6}.tmdl`
- `pbip/Ventas.Report/definition.pbir` y `definition/{version,report}.json`, `definition/pages/pages.json`, `definition/pages/Portada/page.json`
- `docs/checklist-requerimiento.md` (OE1 anotado, sin marcar)
- `docs/modelo/diccionario.md` (sección de consultas)
- `docs/CHANGELOG.md`, `prompts/02-fuentes.md`

## Código clave
Parámetro y fuente web compartida (`expressions.tmdl`):

```m
RutaBase = "C:\Users\wiljo\OneDrive\Escritorio\Trabajo final de Power BI\data\raw"
    meta [IsParameterQuery=true, Type="Text", IsParameterQueryRequired=true]

GDrive_Fuente = Web.Page(Web.Contents("https://docs.google.com/spreadsheets/d/1BbiT3b81T7sEzpAFW7_ks5qU07rEeHal/edit?gid=1022144234"))
```

Excel y CSV:

```m
// Fact_Ventas
let
    Origen = Excel.Workbook(File.Contents(RutaBase & "\Fact_Ventas.xlsx"), null, true),
    Hoja = Origen{[Item="Fact_Ventas",Kind="Sheet"]}[Data]
in
    Hoja

// Dim_Clientes (Columns=5) y Dim_Productos (Columns=9)
Csv.Document(File.Contents(RutaBase & "\Dim_Clientes.csv"),
    [Delimiter=",", Columns=5, Encoding=65001, QuoteStyle=QuoteStyle.None])
```

Tablas web (`Tabla 1` a `Tabla 5` salen de `GDrive_Fuente`; `Tabla 6` de Wikipedia):

```m
Datos = GDrive_Fuente{[Id="Table 1"]}[Data]
Datos = Web.Page(Web.Contents("https://es.wikipedia.org/wiki/Nicaragua")){[Id="Table 6"]}[Data]
```

## Cómo validar en Power BI Desktop
1. Abrir `pbip/Ventas.pbip` (no se abrió Desktop desde aquí).
2. Si pide la ruta, revisar el parámetro `RutaBase` en Transformar datos > Administrar parámetros.
3. En Transformar datos, comprobar que cada consulta carga sin error. `Fact_Ventas` debe mostrar el título en la primera fila.
4. Abrir cada `Tabla N` y confirmar su contenido:
   - `Tabla 1`: vendedores.
   - `Tabla 2` a `Tabla 5`: metas de 2020 a 2023.
   - `Tabla 6`: Organización Territorial.
5. Si los nombres de tabla del Navegador no coinciden con `Table N`, corregir el `Id` en la consulta.

## Pendientes o riesgos
- **Nada de esto se probó en Desktop.** No hay columnas declaradas en el modelo; si Desktop no las crea al actualizar, hará falta "Cerrar y aplicar" tras OE2.
- **G.Drive:** la URL es una vista de Hojas de cálculo de Google (`/edit`). Desde fuera esa página trae una sola tabla. Es probable que `Table 1` a `Table 5` no existan con esa URL. Si falla, probar el enlace de publicación en la web (`/pubhtml`) o el que usó el curso.
- **Wikipedia:** en el HTML de la página hay 13 `<table>` y no pude saber cuál es "Organización Territorial". El `Id="Table 6"` sale del PDF y puede no coincidir con el Navegador. Confirmarlo en Desktop.
- El prompt decía `Fact_Ventas.xls`, pero el archivo real es `.xlsx`; la consulta apunta al `.xlsx`.
- `RutaBase` es una ruta absoluta de esta máquina; hay que cambiarla si el proyecto se mueve.
- El informe mínimo y el esquema PBIR (versiones de `$schema`) se escribieron sin validarlos en Desktop.
