# 05 – Corrección de las consultas Tabla 1 a Tabla 5 (G.Drive)

**Fecha:** 2026-11-01
**Objetivo atendido:** OE1 (conexión a diferentes fuentes de datos).

## Qué se hizo
- En Desktop, `Tabla 1` a `Tabla 5` fallaban con `Expression.Error: La clave no coincidió con ninguna fila de la tabla` (`Id=Table 1`). `Tabla 6` ya carga bien tras el cambio anterior.
- Es el mismo problema de Wikipedia: los `Id` de `Web.Page` no son los que se esperaban.
- Se creó la función `fnBuscarTabla(Fuente, Claves)`. Devuelve la tabla de `Web.Page` que contiene todos los textos de `Claves`, sin distinguir mayúsculas.
- Cada consulta busca su tabla por contenido:
  - `Tabla 1`: `ID Vendedor`
  - `Tabla 2`: `ID_vendedor` y `1/1/2020`
  - `Tabla 3`: `ID_vendedor` y `1/1/2021`
  - `Tabla 4`: `ID_vendedor` y `1/1/2022`
  - `Tabla 5`: `ID_vendedor` y `1/1/2023`
- Los textos clave salen de la exportación a xlsx del libro público, que se descargó para ver sus 5 hojas.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/expressions.tmdl` (función nueva)
- `pbip/Ventas.SemanticModel/definition/model.tmdl` (`ref expression fnBuscarTabla`)
- `pbip/Ventas.SemanticModel/definition/tables/Tabla 1.tmdl` a `Tabla 5.tmdl`
- `docs/checklist-requerimiento.md`, `docs/modelo/diccionario.md`
- `docs/changelog/05-gdrive-tablas.md`, `docs/CHANGELOG.md`, `prompts/05-gdrive-tablas.md`

## Código clave
```m
fnBuscarTabla = (Fuente as table, Claves as list) as table =>
let
    Celdas = (t as table) as list => List.Combine({Table.ColumnNames(t)} & List.Transform(Table.ToColumns(t), (c) => List.Transform(c, (v) => if v = null then "" else Text.Lower(Text.From(v))))),
    ContieneTodo = (t as table) as logical => List.AllTrue(List.Transform(Claves, (k) => List.Contains(List.Transform(Celdas(t), Text.Lower), Text.Lower(k)))),
    Halladas = Table.SelectRows(Fuente, each try ContieneTodo([Data]) otherwise false)
in
    Halladas{0}[Data]

// Tabla 3
Datos = fnBuscarTabla(Origen, {"ID_vendedor", "1/1/2021"})
```

## Cómo validar en Power BI Desktop
1. Reabrir `pbip/Ventas.pbip` o actualizar la vista previa de cada consulta.
2. `Tabla 1` debe mostrar los vendedores; `Tabla 2` a `Tabla 5`, las metas de 2020 a 2023.
3. Si una consulta falla con `Expression.Error ... {0}`, la tabla no está en `GDrive_Fuente`. Abre `GDrive_Fuente` y revisa qué tablas muestra, con sus columnas `Id` y `Name`, y pega una captura.

## Pendientes o riesgos
- No se pudo ejecutar M aquí; la función no está probada.
- Si `Web.Page` solo ve la hoja del `gid` (Vendedores), `Tabla 2` a `Tabla 5` seguirán fallando. En ese caso, una alternativa que sigue siendo origen Web es leer el libro por `Web.Contents` con la URL de exportación (`.../export?format=xlsx`) y `Excel.Workbook`. Esa vía trae las 5 hojas con encabezados limpios, pero cambia los pasos de OE2 (ya no habría "Columna 1" ni filas superiores).
- Si el formato de fecha del encabezado en la vista web difiere de `1/1/AAAA`, cambiar las claves.