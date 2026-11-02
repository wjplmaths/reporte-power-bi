# 18 – Página Análisis General

**Fecha:** 2026-11-01
**Objetivo atendido:** OE5 (visualizaciones), página de Análisis General.

## Punto de partida
El informe ya se abre bien en Desktop: el usuario editó la página Inicio (logo `icono-facturacion`, autor "Heydy Mairena") y guardó, lo que confirma que el formato PBIR de los pasos anteriores es válido. Las estructuras nuevas se escribieron imitando las que Desktop dejó guardadas en `Inicio` (imagen, botón, texto).

## Qué se hizo
11 visuales en una cuadrícula de 1280 × 720 (margen de 20 y separación de 10):

| Zona | Visual | Contenido |
|---|---|---|
| Fila superior | 2 × Chiclet Slicer | `Calendario[Año]` y `Calendario[Mes_Corto]` |
| Fila de tarjetas | 3 × Tarjeta (nueva) | `Total Facturado`, `Costo Total`, `Utilidad Total` |
| Fila de tarjetas | 2 × Imagen | `icono-facturacion` junto a Total Facturado; `icono-ventas` junto a Utilidad Total |
| Izquierda, abajo | Mapa | `Total Facturado` por `Dim_Clientes[Departamento]` |
| Centro, abajo | Tabla | Vendedor, `Total Facturado`, `Meta`, `% Cumplimiento metas` |
| Derecha, arriba | Medidor | Valor `Total Facturado`; máximo y objetivo `Meta` |
| Derecha, abajo | Barras agrupadas | Top 5 de `Dim_Clientes[Nombre Cliente]` por `Total Facturado`, con filtro Top N 5 |

Formato condicional de la tabla (columna `% Cumplimiento metas`, color de fuente): menor a 60 % rojo, de 60 % a menos de 80 % amarillo (79.99 % incluido) y 80 % o más verde. Se compara con el valor sin redondear.

Otros cambios:
- `report.json`: se registró `icono-ventas.jpeg` como recurso y el visual personalizado `ChicletSlicer1448562802762`.
- Solo hay 2 imágenes para 3 tarjetas, así que `Costo Total` va sin ícono.

## Pasos manuales en Desktop (partes frágiles)
1. **Íconos en la tabla** (la guía muestra íconos; el JSON usa colores): seleccionar la tabla, Formato > Celdas > `% Cumplimiento metas` > Íconos (fx). Estilo de formato "Reglas", basado en `% Cumplimiento metas`, diseño a la izquierda de los datos, estilo X / ! / ✔:
   - si valor `>=` 0 y `<` 0.6: X rojo;
   - si valor `>=` 0.6 y `<` 0.8: ! amarillo;
   - si valor `>=` 0.8 y `<=` Máx.: ✔ verde.
   Después se puede quitar el color de fuente.
2. **Etiqueta del medidor:** Formato del medidor > Valor de llamada (o Etiqueta de datos) > en el valor, fx > "Valor del campo" y elegir `% Cumplimiento metas`. Así el globo muestra el porcentaje de avance. Si no aparece esa opción, arrastrar `% Cumplimiento metas` al campo "Información sobre herramientas" y ajustar el texto.
3. **Chiclet Slicer, orden de meses:** el modelo ya ordena `Mes_Corto` por `Mes_Num`, así que los meses deben salir ENE, FEB, MAR... Si salen alfabéticos, seleccionar `Mes_Corto` en la vista de datos > Ordenar por columna > `Mes_Num`.
4. **Top 5:** si el filtro Top N del JSON no se aplica (el gráfico muestra todos los clientes), abrir el panel de Filtros, `Nombre Cliente` > Tipo de filtro "N superiores" > Superior 5 > Por valor `Total Facturado` > Aplicar filtro.

## Archivos creados/modificados
- `pbip/Ventas.Report/definition/pages/AnalisisGeneral/visuals/*/visual.json` (11 visuales)
- `pbip/Ventas.Report/definition/report.json`, `StaticResources/RegisteredResources/icono-ventas.jpeg`
- `docs/checklist-requerimiento.md`, `docs/changelog/18-analisis-general.md`, `docs/CHANGELOG.md`, `prompts/18-analisis-general.md`

## Cómo validar en Power BI Desktop
1. Cerrar Desktop sin guardar y reabrir `pbip/Ventas.pbip`; abrir la pestaña Análisis General.
2. Las tarjetas deben mostrar cifras en córdobas, sin errores. Si el costo sale en blanco, hay IDs de producto huérfanos (changelog 14).
3. El mapa debe mostrar burbujas por departamento. La tabla debe listar los 19 vendedores con los colores según su `% Cumplimiento`.
4. Los chiclets de año y mes deben filtrar todos los visuales. Hay que seleccionar un año para ver un porcentaje de cumplimiento coherente: sin filtro de año suma las metas y las ventas de los cuatro años.

## Pendientes o riesgos
- No se probó en Desktop.
- Los puntos más inciertos son el filtro Top N, el visual Chiclet (los nombres de los roles `Category` y la configuración) y el formato condicional. Si alguno falla, el resto del informe no se ve afectado salvo que Desktop muestre un error de carga: pega el mensaje.
- El medidor usa `Meta` como máximo y como objetivo, así que llega al 100 % justo en la meta; puede pasar de ese valor si se supera.
- La página tiene el mapa por `Dim_Clientes[Departamento]`, como pedía el prompt. Con `Dim_Territorios[Departamento]` se vería igual.
- Faltan por hacer la página de Análisis de Tendencia y la publicación (OE5).