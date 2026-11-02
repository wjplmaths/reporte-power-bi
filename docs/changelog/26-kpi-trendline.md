# 26 – KPI: el eje de tendencia es el rol `TrendLine`

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5, página Análisis de Tendencia. Corrige los changelogs 23 y 25.

## Qué pasó
Tras el changelog 25 (KPI sin eje de tendencia), Desktop mostró al pasar el cursor por el aviso del KPI: "No se puede mostrar este objeto visual. Se necesitan campos para los ejes Valor y Tendencia." Es decir, el KPI sí necesita eje de tendencia, y quitarlo fue un error.

## Causa real del KPI vacío
En el changelog 23 escribí el eje de tendencia con el rol `TrendAxis`, que no existe. El rol correcto del KPI en Power BI es `TrendLine`. Desktop conservaba el campo en el archivo pero no lo reconocía como eje de tendencia, por eso el KPI salía vacío y sin mensaje claro.

El nombre correcto salió de un ejemplo real de KPI en formato PBIR del repositorio público [data-goblin/power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development), donde los roles son `Indicator`, `Goal` y `TrendLine`, con una columna de año como eje.

## Qué se hizo
- KPI: se volvió a poner `Calendario[Año]`, ahora en el rol `TrendLine`. Indicador `Total Facturado`; objetivo `Ventas del Año Anterior`.
- Orden del eje por `Año` ascendente, para que el último punto sea el año más reciente de la selección.
- Se restauró el filtro de `Año` que había en el visual.
- El gráfico combinado no cambia: `Category`, `Y` y `Y2` son los nombres correctos (Desktop los reconoció y los dibuja).

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/AnalisisTendencia/visuals/2413073d751b166ce799/visual.json` (KPI)
- `docs/changelog/26-kpi-trendline.md`, `docs/CHANGELOG.md`, `prompts/26-kpi-trendline.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`; abrir Análisis de Tendencia.
2. El KPI debe mostrar `Total Facturado` del último año de la selección, la comparación con `Ventas del Año Anterior` y el gráfico de fondo con los años.
3. Con todos los años: el valor es el de 2023, comparado con 2022. Con 2021 y 2022 elegidos: el valor de 2022 comparado con 2021.
4. Esto difiere de la tarjeta de al lado, que suma todos los años seleccionados. La diferencia es esperada: el KPI toma el último punto del eje.

## Pendientes o riesgos
- No se probó en Desktop.
- En 2020 `Ventas del Año Anterior` está en blanco (no hay 2019), así que el KPI sin objetivo puede salir sin comparación si 2020 es el único año elegido.
- Lección: de aquí en adelante, los nombres de roles de visuales nuevos se contrastan con un ejemplo real antes de escribirlos.