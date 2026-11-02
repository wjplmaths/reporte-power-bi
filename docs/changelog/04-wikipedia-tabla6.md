# 04 – Corrección de la consulta Tabla 6 (Wikipedia)

**Fecha:** 2026-11-01
**Objetivo atendido:** OE1 (conexión a diferentes fuentes de datos).

## Qué se hizo
- En Desktop, `Tabla 6` fallaba con `Expression.Error: La clave no coincidió con ninguna fila de la tabla` (`Id=Table 5`). Las demás consultas abren sin error.
- Los `Id` que genera `Web.Page` en Desktop no coinciden con la numeración del HTML de la página, ni con `Table 5` ni con `Table 6`.
- Ahora la consulta no depende del número: elige la tabla cuyo contenido incluye la columna `Cabecera`. Esa palabra solo aparece en la tabla de Organización Territorial.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/Tabla 6.tmdl`
- `docs/checklist-requerimiento.md`, `docs/modelo/diccionario.md`
- `docs/changelog/04-wikipedia-tabla6.md`, `docs/CHANGELOG.md`, `prompts/04-wikipedia-tabla6.md`

## Código clave
```m
let
    Origen = Web.Page(Web.Contents("https://es.wikipedia.org/wiki/Nicaragua")),
    Candidatas = Table.SelectRows(Origen, each try (List.Contains(Table.ColumnNames([Data]), "Cabecera") or List.AnyTrue(List.Transform(Table.ToColumns([Data]), (c) => List.Contains(c, "Cabecera")))) otherwise false),
    Datos = Candidatas{0}[Data]
in
    Datos
```

## Cómo validar en Power BI Desktop
1. Cerrar y volver a abrir `pbip/Ventas.pbip` (o actualizar la vista previa de `Tabla 6`).
2. `Tabla 6` debe mostrar la tabla de departamentos con `Cabecera`, población y superficie.
3. Si sigue fallando, abrir el paso `Origen` de `Tabla 6` y mirar la columna `Id`/`Caption` de la lista de tablas: pega esa lista y se ajusta la consulta.

## Pendientes o riesgos
- No se pudo ejecutar M desde aquí; la consulta se escribió a partir del HTML de la página y no está probada.
- Si `Candidatas` queda vacía, el error será `Expression.Error` al leer `{0}`. Pasaría si Wikipedia cambia el texto del encabezado.
- Las filas de título arriba y la fila de fuente abajo se limpian en OE2.