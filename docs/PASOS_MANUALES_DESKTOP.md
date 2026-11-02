# Pasos manuales en Power BI Desktop

Lo que tienes que hacer tú en Desktop para cerrar el proyecto, en orden. Origen: la auditoría final (`docs/CHECKLIST_FINAL.md`). Los nombres de menú son los de Desktop en español; si tu versión los muestra distinto, el icono o la cinta equivalente está indicado entre paréntesis.

**Valores de referencia** (calculados desde los archivos de origen, para que compruebes que todo carga bien):

| Elemento | Valor esperado |
|---|---|
| Filas de `Fact_Ventas` | 266 128 |
| Filas de `Dim_Clientes` / `Dim_Productos` / `Dim_Vendedores` | 442 / 227 / 19 |
| Filas de `Dim_Metas` | 912 (19 vendedores × 12 meses × 4 años) |
| Filas de `Dim_Territorios` | 17 departamentos |
| Filas de `Calendario` | 1 461 (2020-01-01 a 2023-12-31) |
| `Total Facturado` (sin filtros) | C$201,686,224.53 |
| `Costo Total` / `Utilidad Total` | C$114,107,426.25 / C$87,578,798.28 |
| `Meta` | C$269,764,999.68 |
| `% Cumplimiento metas` | 74.8 % |

## A. Abrir y comprobar que todo carga

1. **Abrir el proyecto.** `Archivo` > `Abrir informe` > `Examinar este dispositivo` y elige `pbip/Ventas.pbip`. Si aparece “El informe tiene problemas”, pulsa `Copiar los detalles al Portapapeles` y envíame el texto.
2. **Revisar permisos de las fuentes.** `Archivo` > `Opciones y configuración` > `Configuración de origen de datos`. Para `docs.google.com` y `es.wikipedia.org`: `Editar permisos` > `Anónimo` > `Aceptar`. Los tres archivos locales (`Fact_Ventas.xlsx`, `Dim_Clientes.csv`, `Dim_Productos.csv`) llevan la ruta escrita dentro de la consulta: si moviste la carpeta del proyecto, hay que editarla en el Editor de Power Query (`Editor avanzado`).
3. **Actualizar y aplicar.** Pestaña `Inicio` > `Transformar datos` > `Transformar datos`. Recorre las consultas y confirma que ninguna tiene el triángulo de error. Después `Inicio` > `Cerrar y aplicar` (en el Editor de Power Query) y luego `Inicio` > `Actualizar` en la ventana principal. Comprueba los recuentos de la tabla de referencia (vista de tabla, barra inferior “Tabla: … (N filas)”). Es el requisito “Cierre y aplique todos los cambios” de la guía.
4. **Revisar `Dim_Territorios`** (la consulta que más falta por confirmar). En el Editor de Power Query, selecciona `Dim_Territorios` y comprueba que quedan 17 filas y 4 columnas (`Departamento`, `Cabecera`, `Población (2023)`, `Superficie (km²)`), sin la fila “* Fuente…”. Si falla, abre el paso `Datos` y mándame una captura de sus columnas.
5. **Comprobar las metas.** En el Editor de Power Query, selecciona `Metas_2020` (y luego las otras tres): el panel `Pasos aplicados` debe mostrar 8 pasos (Origen, FilasSuperioresEliminadas, EncabezadosPromovidos, ColumnasUtiles, IdRenombrado, MesesEnFilas, ColumnasRenombradas, TiposCorrectos) y el resultado tres columnas `ID_Vendedor`, `Fecha` y `Meta` (228 filas por año). Clic derecho sobre cada una: `Habilitar carga` debe estar **desmarcado** (el nombre sale en cursiva). `Dim_Metas` sí debe estar marcado y dar 912 filas. Lo mismo en `Dim_Vendedores`: debe aparecer el paso `FilasSuperioresEliminadas`.

## B. Modelo y cálculos

6. **Verificar el modelo.** Icono `Vista de modelo` (tercer icono de la barra izquierda). Deben verse 7 relaciones de línea continua, cada una `*` → `1` y con una sola flecha desde el lado “1”: `Fact_Ventas` con `Dim_Clientes`, `Dim_Productos`, `Dim_Vendedores` y `Calendario`; `Dim_Metas` con `Dim_Vendedores` y `Calendario`; `Dim_Clientes` con `Dim_Territorios`. Debe parecerse al diagrama de la p.5 de la guía. (Hay además una relación automática con una tabla de fecha oculta; no es un error.)
7. **Verificar `Calendario`.** Icono `Vista de tabla` > selecciona `Calendario`:
   - Debe tener `Date`, `Año`, `Trimestre` (“1 Trim”), `Mes_Num`, `Mes_Corto` (ENE…) y `Mes_Largo` (ENERO…).
   - Mira septiembre: si `Mes_Corto` dice `SEPT` en lugar de `SEP`, se corrige con un cambio de una línea (avísame).
   - `Herramientas de tabla` > `Marcar como tabla de fechas`: debe aparecer marcada con la columna `Date`.
   - Selecciona `Mes_Corto` > `Herramientas de columna` > `Ordenar por columna` > `Mes_Num` (ya viene configurado; confirma que el orden en un visual es ENE, FEB, MAR…).
8. **Verificar las medidas.** En el panel `Datos`, tabla `Medidas`. Arrastra `Total Facturado`, `Costo Total`, `Utilidad Total`, `Meta`, `% Cumplimiento metas` y `Ventas del Año Anterior` a una tarjeta o tabla y compara con la tabla de referencia. `Ventas del Año Anterior` debe estar en blanco para 2020.

## C. Visuales

9. **Chiclet Slicer.** En el panel `Visualizaciones`, pulsa `…` (`Obtener más objetos visuales`), busca “Chiclet Slicer” y `Agregar` si no está instalado. En las dos páginas de análisis, los dos chiclets (Año y Mes) deben verse como botones; si salen como lista, elige el icono del Chiclet Slicer en `Visualizaciones` con el visual seleccionado (conserva los campos).
10. **Íconos del formato condicional.** Página `Análisis General`, selecciona la tabla > `Formato visual` > `Elementos de celda` (o `Celdas`) > en `Serie`, elige `% Cumplimiento metas` > activa `Iconos` > `fx` (formato condicional). Las reglas deben ser: `>= 0` y `< 0.6` → ✗; `>= 0.6` y `< 0.8` → !; `>= 0.8` y `<= Máx.` → ✔, con el diseño “A la izquierda de los datos”, igual que la imagen de la p.7 de la guía. **Prueba la regla roja:** ningún vendedor está bajo 60 %, así que no verás la ✗; para verla, filtra un mes con el Chiclet y busca valores bajos, o cambia temporalmente el límite a `0.7` y devuélvelo a `0.6`.
11. **Mapa.** `Análisis General`: pasa el cursor por las burbujas y confirma que León aparece en Nicaragua (no en España) y que hay 17 burbujas. Si algún departamento cae fuera de Nicaragua, selecciona la columna `País` en `Vista de datos` > `Herramientas de columna` > `Categoría de datos` > `País o región`.
12. **KPI.** `Análisis de Tendencia`: el KPI “Total Facturado vs año anterior” debe mostrar el valor, la comparación con el objetivo y el fondo con años. Si sale vacío, pasa el cursor por el icono de información `i` que aparece en su esquina superior izquierda y mándame el mensaje.
13. **Segmentaciones sincronizadas.** Elige un año y un mes en `Análisis General`, cambia a `Análisis de Tendencia`: la misma selección debe estar marcada. Es el requisito “Reutilice las segmentaciones de la página anterior”. Si no se sincronizan: `Vista` > `Sincronizar segmentaciones`, selecciona cada segmentación y activa ambas páginas.
14. **Etiqueta personalizada del medidor (opcional, a mano).** La etiqueta escrita por archivo se pintaba como “(En blanco)” y se retiró (changelog 36), así que el globo muestra `Total Facturado` (C$201.69 mill.). Para que muestre el % de avance (la guía lo pide: “etiqueta personalizada en el valor del globo”): `Análisis General`, selecciona el medidor > `Formato visual` > `Valor de llamada` (también llamado `Etiqueta de datos`) > en el valor, pulsa `fx` y elige una opción de etiqueta basada en un campo o `Valor del campo`: `Etiqueta % Avance`. Si tu versión no la ofrece, el % de avance sigue visible en el tooltip del medidor. Cuando lo hagas, guarda el proyecto y avísame: lo copio al archivo y así queda en el repositorio.
15. **Comprobar tooltips, navegación y tema.**
    - Tooltips: las páginas `Tooltip Mapa` y `Tooltip Barras` se eliminaron en Desktop (la guía no las pide), así que el mapa y las barras Top 5 muestran el tooltip normal. No hay nada que comprobar; si quieres limpiar la referencia colgante, selecciona cada visual > `Formato visual` > `General` > `Información sobre herramientas` > `Tipo: Predeterminado`.
    - Navegación: `Ctrl` + clic en cada botón (Inicio ↔ Análisis General ↔ Análisis de Tendencia).
    - Tema (opcional): `Vista` > `Temas` > `Examinar temas` y elige uno de `assets/themes/` (el informe usa ahora `CopilotDefault`).

## D. Entrega

16. **Guardar como .pbix en `dist/`.** Antes, `Archivo` > `Guardar` para dejar también el proyecto `.pbip` al día. Después `Archivo` > `Guardar como`, en el tipo de archivo elige `Archivos de Power BI (*.pbix)` y guarda `dist/ProyectoVentas-HeydMairena.pbix`. Cuando exista el .pbix real, elimina la carpeta `dist/TrabajoFinalCursoHeydyMairena.pbix`, que no es un .pbix válido sino un proyecto descomprimido en blanco.
17. **Publicar.** `Inicio` > `Publicar` > elige el área de trabajo (por ejemplo `Mi área de trabajo`) > `Seleccionar`. Cuando termine, pulsa el enlace `Abrir “ProyectoVentas-HeydMairena.pbix” en Power BI`.
18. **Compartir el enlace.** En el servicio de Power BI, con el informe abierto: `Compartir` (barra superior) > escribe los destinatarios o elige `Copiar vínculo`, y pega el enlace en el grupo de trabajo. Si tu cuenta no permite compartir, otra opción es `Archivo` > `Insertar informe` > `Publicar en la Web (público)`, solo si el curso lo admite, ya que cualquiera con el enlace vería los datos.
19. **Subir a Classroom.** Sube `dist/ProyectoVentas-HeydMairena.pbix` a la tarea del curso en Classroom.

## Si algo falla

| Síntoma | Qué hacer |
|---|---|
| Error en `Fact_Ventas` o sale una fila de título como encabezado | En el Editor de Power Query, `Fact_Ventas` > paso `FilasSuperioresEliminadas`: el 3 de `Table.Skip(Hoja, 3)` es el número de filas a quitar; ajusta y avísame. |
| `Dim_Territorios` vacía o con error | Mándame la lista de columnas del paso `Datos`. |
| `Metas_*` falla | Comprueba que el enlace de G.Drive es público y que el permiso de `docs.google.com` es Anónimo (paso 2). |
| Mensaje de problemas al abrir el proyecto | `Copiar los detalles al Portapapeles` y envíamelo. |
