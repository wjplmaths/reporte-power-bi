# 12 – Formato de fecha de Calendario según la imagen de la guía

**Fecha:** 2026-11-01
**Objetivo atendido:** OE3 (tabla Calendario). Revisión de `Calendario` contra la imagen de ejemplo del PDF.

## Comparación con la imagen
| Punto | Imagen de la guía | `Calendario.tmdl` |
|---|---|---|
| Orden de columnas | Date, Año, Trimestre, Mes_Num, Mes_Corto, Mes_Largo | Igual |
| Rango | Desde el 2020/1/1 hasta el 2023/12/31 | Igual (1 461 filas) |
| `Año` | 2020, número sin separador de miles | Entero, formato `0` |
| `Trimestre` | `1 Trim` | `QUARTER & " Trim"`, igual |
| `Mes_Num` | 1, 2 | Entero, igual |
| `Mes_Corto` | `ENE`, `FEB` | `UPPER(FORMAT(...,"MMM","es-ES"))`, igual |
| `Mes_Largo` | `ENERO`, `FEBRERO` | `UPPER(FORMAT(...,"MMMM","es-ES"))`, igual |
| `Date` | `27/1/2020`: día/mes/año sin ceros | **Distinto**: tenía formato "Long Date" (`lunes, 27 de enero de 2020`) |

## Qué se cambió
`Date` pasó de `Long Date` a `d/M/yyyy`, para que se vea como en la guía (`27/1/2020`, `1/2/2020`).

## Archivos creados/modificados
- `pbip/Ventas.SemanticModel/definition/tables/Calendario.tmdl`
- `docs/changelog/12-calendario-formato-fecha.md`, `docs/CHANGELOG.md`, `prompts/12-calendario-formato-fecha.md`

## Cómo validar en Power BI Desktop
En la vista de tabla de `Calendario`, `Date` debe verse `1/1/2020`, `27/1/2020`, `1/2/2020`, y el resto de columnas como en la imagen.

## Pendientes o riesgos
- Sigue sin probarse en Desktop. La imagen no permite ver el mes de septiembre; puede salir `SEPT` en vez de `SEP` (ver changelog 11).
- La imagen no muestra relaciones; el modelado (relaciones con `Fact_Ventas`, `Dim_Metas` y las demás tablas) sigue pendiente.