# Diccionario de datos – fuentes locales

Resultado de inspeccionar `data/raw/` (2026-11-01). Las fuentes en línea (G.Drive y Wikipedia) todavía no se inspeccionaron: sus enlaces están en `docs/requerimiento/`.

Los nombres de columna son los **reales de origen**, con tildes y espacios; el modelo final puede renombrarlos en Power Query.

## Fact_Ventas.xlsx
- Hoja única: `Fact_Ventas`. Es `.xlsx` (el PDF dice "xls").
- Filas con datos: **266 128**. La última fila de la hoja es la 266 132.
- Estructura de la hoja:
  - **Columna A vacía**: los datos empiezan en la columna B.
  - **Fila 1**: título "CONSOLIDADO DE VENTAS 2020-2023" (basura).
  - **Fila 2**: vacía, solo con formato.
  - **Fila 3**: no existe.
  - **Fila 4**: encabezados.
  - **Fila 5 en adelante**: datos.
  - **Columna L**: celda de formato en el encabezado, sin datos.
- Para Power Query: quitar 3 filas superiores y promover encabezados. Luego quitar la columna A vacía y la L.

| Columna | Tipo sugerido | Rango / notas |
|---|---|---|
| ID Transacción | Número entero | 24078 – 290205, 266 128 únicos (llave de la tabla de hechos) |
| ID Vendedor | Número entero | 121 – 192. Relaciona con `Dim_Vendedores` |
| ID Cliente | Número entero | 114 – 713. Relaciona con `Dim_Clientes` |
| ID Factura | Número entero | 12548 – 117646 |
| ID Producto | Número entero | 214 – 670. Relaciona con `Dim_Productos` |
| Fecha de Venta | Fecha | Guardada como serie de Excel: 43832 = 2020-01-02 a 45290 = 2023-12-30. Relaciona con `Calendario` |
| Fecha de Envío | Fecha | 43834 a 45300 (2020-01-04 a 2024-01-09, fuera del rango de `Calendario`) |
| Cantidad | Número entero | 1 – 450 |
| Precio Unitario | Decimal | 0.66 – 1955.97 |
| Tasa de Impuesto | Número entero | Siempre 15. Se puede descartar si no se usa |

- No hay valores vacíos en las columnas B a K.
- `Total Facturado` y `Costo Total` no existen como columna, se calculan con DAX (OE4).

## Dim_Clientes.csv
- 442 filas de datos y 5 columnas. Codificación UTF-8 sin BOM. En Power Query hay que elegir **65001: UTF-8**, o `País` y los departamentos salen con caracteres rotos.
- Sin filas basura arriba. Encabezados en la fila 1.

| Columna | Tipo sugerido | Notas |
|---|---|---|
| ID Cliente | Número entero | 442 únicos, sin duplicados ni vacíos |
| Nombre Cliente | Texto | 442 únicos |
| Fecha de Alta | Fecha | Formato `d/m/aaaa` (ej. `15/3/2004`). Hay que fijar la configuración regional **es-NI/es-ES** al convertir |
| Departamento | Texto | 17 valores |
| País | Texto | Siempre "Nicaragua" |

- No hay ID `null` en el archivo actual. El paso "eliminar ID null" del requerimiento no quita ninguna fila, pero se aplica igual.
- Departamentos con nombre largo a reemplazar: "Región Autónoma de la Costa Caribe Norte" (29 clientes) y "Región Autónoma de la Costa Caribe Sur" (31 clientes).

## Dim_Productos.csv
- 227 filas de datos y 9 columnas. UTF-8 sin BOM, mismo cuidado que en clientes.
- Sin filas basura arriba. Encabezados en la fila 1.

| Columna | Tipo sugerido | Notas |
|---|---|---|
| ID Producto | Número entero | 227 únicos, sin duplicados ni vacíos |
| Producto | Texto | 227 únicos |
| Marca | Texto | 6 marcas: Empaca Todo, Conexión TI, Northwind, Veluz, Zoofarm, DeliFood |
| Costo Unitario | Decimal | Punto como separador decimal (ej. `20.94`). Hay que convertir con configuración regional **en-US**. En una configuración regional con coma decimal quedaría mal |
| Grupo Contribución | Texto | Baja / Media / Alta Contribución |
| Grupo Margen | Texto | Bajo / Medio / Alto Margen |
| Color | Texto | 10 valores, incluye "N/A" (51 filas) |
| Tiempo de Espera (dias) | Número entero | 2, 3, 7, 12, 14, 20 |
| Es articulo refrigerado | Texto | "Artículo Seco" (219) o "Artículo Refrigerado" (8) |

- No hay ID `null` en el archivo actual. Mismo caso que en clientes.

## Relaciones previstas (a confirmar en OE3)
- `Fact_Ventas[ID Cliente]` → `Dim_Clientes[ID Cliente]` (muchos a uno)
- `Fact_Ventas[ID Producto]` → `Dim_Productos[ID Producto]` (muchos a uno)
- `Fact_Ventas[ID Vendedor]` → `Dim_Vendedores` (muchos a uno)
- `Fact_Ventas[Fecha de Venta]` → `Calendario[Fecha]` (muchos a uno)
- `Dim_Metas` → `Dim_Vendedores` y `Calendario`
- `Dim_Clientes[Departamento]` ↔ `Dim_Territorios`, para el mapa por departamento

## Riesgos detectados
- La tabla de hechos tiene IDs de cliente hasta 713, pero `Dim_Clientes` solo tiene 442 clientes. **No se verificó** si todos los IDs de la tabla de hechos existen en `Dim_Clientes`. Hay que comprobarlo en Desktop (relación con huérfanos).
- La `Fecha de Envío` llega hasta enero de 2024, fuera del calendario 2020-2023. No debe usarse como relación activa con `Calendario`.

## Consultas de conexión (OE1, sin transformar)
Definidas en `pbip/Ventas.SemanticModel/definition/`. Aún no hay columnas declaradas en el modelo. El enlace de G.Drive es público: contiene el libro `Vendedores + Metas_Mensuales.xlsx` con 5 hojas (Vendedores, Metas 2020, Metas 2021, Metas 2022, Metas 2023). La hoja Vendedores trae `ID Vendedor`, `Nombre Completo` y `Primer Nombre`.

| Consulta | Origen | Función M | Carga |
|---|---|---|---|
| `RutaBase` | Parámetro de texto con la ruta de `data/raw` | — | No |
| `fnBuscarTabla` | Función auxiliar: devuelve la tabla de `Web.Page` que contiene todos los textos pedidos | — | No |
| `GDrive_Fuente` | Google Drive (enlace en `docs/requerimiento/Enlace G_Drive.txt`) | `Web.Page` | No |
| `Fact_Ventas` | `Fact_Ventas.xlsx`, hoja `Fact_Ventas` | `Excel.Workbook` | Sí |
| `Dim_Clientes` | `Dim_Clientes.csv` (UTF-8, coma, 5 columnas) | `Csv.Document` | Sí |
| `Dim_Productos` | `Dim_Productos.csv` (UTF-8, coma, 9 columnas) | `Csv.Document` | Sí |
| `Tabla 1` | `GDrive_Fuente`, tabla que contiene `ID Vendedor` (vendedores) | `Web.Page` | Sí |
| `Tabla 2` a `Tabla 5` | `GDrive_Fuente`, tabla que contiene `ID_vendedor` y `1/1/AAAA` (metas 2020 a 2023) | `Web.Page` | Sí |
| `Tabla 6` | Wikipedia, tabla elegida por contenido (columna `Cabecera`): Organización Territorial | `Web.Page` | Sí |

En OE2 se renombran: `Tabla 1` a `Dim_Vendedores`, `Tabla 2` a `Tabla 5` a `Metas_2020` a `Metas_2023` (sin carga) y `Tabla 6` a `Dim_Territorios`.
### Estructura esperada de `Tabla 6` (según el HTML de Wikipedia, 2026-11-01)
Fila de título "Departamentos de Nicaragua", encabezados `Departamento | ISO:NI | Cabecera | Población (2023) | Superficie (km²) | Densidad`, 17 filas de departamentos y una fila final de fuente ("* Fuente: Estimación del 2023 - INIDE") que se elimina en OE2. Los departamentos usan "Costa Caribe Norte" y "Costa Caribe Sur", igual que `Dim_Clientes` tras su limpieza.
### Estructura del libro de G.Drive (leída de su exportación a xlsx, 2026-11-01)
- **Vendedores:** 19 filas de datos, columnas `ID Vendedor`, `Nombre Completo`, `Primer Nombre`.
- **Metas 2020 a Metas 2023:** columna `ID_vendedor` (con v minúscula) y una columna por mes, con encabezados de texto `1/1/AAAA`, `1/2/AAAA`... (día/mes/año). El valor es la meta mensual del vendedor, con decimales largos. Hay que anular la dinamización de las columnas de mes (OE2).
- Vista web (`Web.Page`): se espera una columna extra de numeración de filas y una fila de letras de columna. Eso explica "Columna 1" y las filas superiores de la guía; no se pudo ver desde aquí.