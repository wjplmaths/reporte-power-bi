# 03 – Ajuste de las consultas de conexión (Wikipedia y G.Drive)

**Fecha:** 2026-11-01
**Objetivo atendido:** OE1 (conexión a diferentes fuentes de datos). Repite el prompt 2 con dos datos nuevos del usuario.

## Qué se hizo
- Wikipedia: la tabla de Organización Territorial es la **Tabla 5** del Navegador, no la 6. Se cambió `Tabla 6` para que lea `Table 5`.
- La consulta se sigue llamando `Tabla 6`, como en la guía del PDF, para que el renombrado a `Dim_Territorios` en OE2 coincida con ella.
- G.Drive: el usuario confirma que el enlace es público. No cambia ninguna consulta. Se comprobó desde fuera que:
  - el enlace responde sin pedir inicio de sesión;
  - el libro se llama `Vendedores + Metas_Mensuales.xlsx` y tiene 5 hojas: Vendedores, Metas 2020, Metas 2021, Metas 2022 y Metas 2023;
  - la hoja del `gid` del enlace es Vendedores, con las columnas `ID Vendedor`, `Nombre Completo` y `Primer Nombre`.
- El resto de las consultas del paso 02 queda igual.

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/Tabla 6.tmdl`
- `docs/checklist-requerimiento.md`, `docs/modelo/diccionario.md`
- `docs/changelog/03-fuentes-ajuste.md`, `docs/CHANGELOG.md`, `prompts/03-fuentes-ajuste.md`

## Código clave
```m
// Tabla 6 (Organización Territorial)
let
    Origen = Web.Page(Web.Contents("https://es.wikipedia.org/wiki/Nicaragua")),
    Datos = Origen{[Id="Table 5"]}[Data]
in
    Datos
```

## Cómo validar en Power BI Desktop
1. Abrir `pbip/Ventas.pbip` y revisar el parámetro `RutaBase`.
2. En `Tabla 6`, comprobar que sale la tabla de Organización Territorial (departamentos y regiones).
3. En `Tabla 1` a `Tabla 5`, comprobar que salen Vendedores y las metas 2020 a 2023 (en ese orden).
4. Si el orden de las tablas web no coincide, cambiar el `Id` en la consulta correspondiente.

## Pendientes o riesgos
- Sigue sin probarse en Desktop. La página de Hojas de cálculo de Google trae en su HTML una sola tabla visible (la de la hoja activa). Desktop puede exponer las 5 hojas de otra forma, pero no lo pude comprobar.
- Si `Table 1` a `Table 5` no aparecen en `Web.Page`, la alternativa es el enlace `/pubhtml` del libro. Hoy ese enlace devuelve una página sin tablas desde fuera.
- Wikipedia puede cambiar el orden de sus tablas con el tiempo.
