# 01 – Checklist del requerimiento y diccionario de fuentes locales

**Fecha:** 2026-11-01
**Objetivo atendido:** preparación transversal (OE1 a OE5). Es el análisis previo al modelo; no cambia `pbip/`.

## Qué se hizo
- Se leyó la guía del PDF y se convirtió cada requisito en una casilla, agrupada por OE1 a OE5.
- Se inspeccionaron `Fact_Ventas.xlsx`, `Dim_Clientes.csv` y `Dim_Productos.csv`: columnas, tipos, filas basura y calidad básica.
- Se documentó la estructura de cada archivo y las relaciones previstas.

## Archivos creados/modificados
- `docs/checklist-requerimiento.md` (nuevo contenido)
- `docs/modelo/diccionario.md` (nuevo)
- `docs/changelog/01-checklist-diccionario.md` (este archivo)
- `docs/CHANGELOG.md` (índice)
- `prompts/01-checklist-diccionario.md` (copia del prompt)

## Código clave
No aplica: no se escribió M ni DAX. Hallazgos que condicionan el M de OE2:

```text
Fact_Ventas: quitar 3 filas superiores (título en B1, B2 vacía, B3 inexistente), encabezados en la fila 4;
             la columna A está vacía y la L es solo formato.
CSV:         UTF-8 sin BOM (Csv.Document con Encoding=65001).
Fechas:      Dim_Clientes[Fecha de Alta] en d/m/aaaa; Fact_Ventas guarda serie de Excel (se importa como fecha).
Decimales:   Dim_Productos[Costo Unitario] usa punto decimal.
```

## Cómo validar en Power BI Desktop
No hay nada que abrir todavía. Para contrastar el diccionario: en Power Query, importar los tres archivos y comprobar los conteos (266 128 / 442 / 227 filas de datos).

## Pendientes o riesgos
- No se inspeccionaron las fuentes en línea (G.Drive y Wikipedia): se hará en el paso OE1.
- No se verificó si todos los ID Cliente de `Fact_Ventas` (hasta 713) existen en `Dim_Clientes` (442).
- `Fecha de Envío` llega a enero de 2024, fuera del calendario.
- El PDF deja sin regla el tramo 79%–80% del formato condicional.
- `Dim_Clientes` y `Dim_Productos` no traen ID `null`; los pasos de limpieza correspondientes no quitarán filas.
