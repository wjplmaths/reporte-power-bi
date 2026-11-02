# Checklist final – auditoría del proyecto contra la guía

**Fecha:** 2026-11-01  
**Única fuente de verdad:** `docs/requerimiento/GUIA DE TRABAJO FINAL.pdf` (9 páginas leídas completas, incluidas las 7 imágenes: logo, tabla de Calendario, diagrama del modelo, formato condicional, ícono de Power BI y la frase final).  
**Alcance:** lectura de `pbip/` (TMDL y PBIR) sin modificarlo. El checklist anterior no se usó como base: se construyó de nuevo desde el PDF y luego se contrastó.

**Rutas abreviadas en la columna Evidencia:** `SM/` = `pbip/Ventas.SemanticModel/definition/`; `RP/` = `pbip/Ventas.Report/definition/pages/`. Los números son líneas del archivo.

**Estados:** ✅ cumplido con evidencia en archivo · 🟡 parcial · ❌ pendiente · 🔍 no verificable por archivo (hay que validarlo en Desktop).

## Resumen

| Objetivo | Requisitos | ✅ | 🟡 | 🔍 | ❌ | % cumplido (✅) | % ponderado (✅ + ½🟡) |
|---|---:|---:|---:|---:|---:|---:|---:|
| OE1 – Conexión a diferentes fuentes de datos | 5 | 5 | 0 | 0 | 0 | 100 % | 100 % |
| OE2 – Transformación de datos | 49 | 35 | 9 | 5 | 0 | 71 % | 81 % |
| OE3 – Tabla Calendario + Modelado de datos | 14 | 14 | 0 | 0 | 0 | 100 % | 100 % |
| OE4 – DAX | 8 | 6 | 2 | 0 | 0 | 75 % | 88 % |
| OE5 – Visualizaciones + Compartir informe | 28 | 22 | 1 | 2 | 3 | 79 % | 80 % |
| **Total** | **104** | **82** | **12** | **7** | **3** | **79 %** | **85 %** |

Lectura del resultado: el 🔍 y el ❌ no son errores de código. Los 🔍 se confirman en Desktop (ver `PASOS_MANUALES_DESKTOP.md`) y los ❌ son la entrega (publicar, `.pbix`, Classroom) y el “plus” de imágenes en tarjetas. Los 🟡 son de tres clases: el nombre `SumMetas` (OE4), la etiqueta del globo del medidor (OE5) y los pasos de la guía que el método elegido (CSV exportado) vuelve innecesarios, como quitar filas superiores o “Columna 1” (OE2).

## OE1 – Conexión a diferentes fuentes de datos

| # | Requisito (cita de la guía) | Estado | Evidencia | Acción requerida |
|---|---|:---:|---|---|
| OE1.1 | p.1 · OE1 · «Conectarse al archivo “xls” (Fact_Ventas)» | ✅ | `SM/tables/Fact_Ventas.tmdl:L79` Excel.Workbook sobre `Fact_Ventas.xlsx`; el archivo real es .xlsx | — |
| OE1.2 | p.1 · OE1 · «Conectarse a los archivos “csv” (Dim_Clientes)» | ✅ | `SM/tables/Dim_Clientes.tmdl:L60` | — |
| OE1.3 | p.1 · OE1 · «…y (Dim_Productos)» | ✅ | `SM/tables/Dim_Productos.tmdl:L86` | — |
| OE1.4 | p.1 · OE1 · «Conectarse al archivo en línea “G.Drive” (Vendedores + Metas_Mensuales) ** Importar como tipo Web» | ✅ | `SM/tables/Dim_Vendedores.tmdl:L27` Web.Contents (origen Web) para Vendedores; `SM/expressions.tmdl:L24` y siguientes (Web.Contents por hoja) | Nota: se usa el CSV exportado de cada hoja (Web.Contents), no `Web.Page` con “Tabla 1…5”. El resultado es Web, pero los pasos difieren de la guía (ver OE2). |
| OE1.5 | p.1 · OE1 · «Conectarse al enlace Web “Wikipedia” (Extraer tabla: Organización Territorial – Tabla 6)» | ✅ | `SM/tables/Dim_Territorios.tmdl:L42` Web.Page sobre es.wikipedia.org/wiki/Nicaragua; la tabla se elige por contenido (columna “Cabecera”) | Nota: en el Navegador de Desktop es la tabla 5 (confirmado por el usuario); el nombre “Tabla 6” de la guía es el de la consulta. |

## OE2 – Transformación de datos

| # | Requisito (cita de la guía) | Estado | Evidencia | Acción requerida |
|---|---|:---:|---|---|
| OE2.1 | p.2 · OE2 · Consulta Fact_Ventas · «Elimine filas superiores y coloque los encabezados correctos» | ✅ | `SM/tables/Fact_Ventas.tmdl:L82` Table.Skip(Hoja, 3) + `SM/tables/Fact_Ventas.tmdl:L84`. Cruce independiente: la suma Cantidad×Precio de la fuente (266 128 filas) da C$201,686,224.53, igual que Desktop | Ninguna |
| OE2.2 | p.2 · OE2 · Fact_Ventas · «Elimine columnas que no usará en su modelo de datos» | ✅ | `SM/tables/Fact_Ventas.tmdl:L87` conserva 7 de 10 columnas; quita ID Factura, Fecha de Envío y Tasa de Impuesto | Nota: el diagrama de la guía conserva Fecha de Envío e ID Factura; quitarlas no incumple la viñeta. |
| OE2.3 | p.2 · OE2 · Fact_Ventas · «Revise que el tipo de datos sea el correcto en cada columna» | ✅ | `SM/tables/Fact_Ventas.tmdl:L91` IDs y Cantidad Int64, Fecha de Venta date, Precio Unitario number | — |
| OE2.4 | p.2 · OE2 · Consulta Dim_Clientes · «Elimine ID de clientes “null”» | ✅ | `SM/tables/Dim_Clientes.tmdl:L64` Table.SelectRows ID <> null y <> "" | La fuente no trae nulos (442 filas), el paso no quita ninguna. |
| OE2.5 | p.2 · OE2 · Dim_Clientes · «Modifique el Departamento “Región Autónoma de la Costa Caribe Norte” por “Costa Caribe Norte”» | ✅ | `SM/tables/Dim_Clientes.tmdl:L66` | — |
| OE2.6 | p.2 · OE2 · Dim_Clientes · «…“Región Autónoma de la Costa Caribe Sur” por “Costa Caribe Sur”» | ✅ | `SM/tables/Dim_Clientes.tmdl:L67`. Comprobado en la fuente: los 17 departamentos resultantes coinciden con los de Wikipedia | — |
| OE2.7 | p.2 · OE2 · Dim_Clientes · «Revise que el tipo de datos sea el correcto en cada columna» | ✅ | `SM/tables/Dim_Clientes.tmdl:L69` ID Int64, Fecha de Alta date (es-ES), resto text | — |
| OE2.8 | p.2 · OE2 · Consulta Dim_Productos · «Elimine ID de clientes “null”» (la guía dice “clientes”; se entiende ID Producto) | ✅ | `SM/tables/Dim_Productos.tmdl:L90` | Ninguna (la fuente no trae nulos: 227 filas). |
| OE2.9 | p.2 · OE2 · Dim_Productos · «Revise que el tipo de datos sea el correcto en cada columna» | ✅ | `SM/tables/Dim_Productos.tmdl:L92` Costo Unitario number leído con en-US | — |
| OE2.10 | p.2 · OE2 · Consulta Tabla 1 · «Cambie el nombre de la consulta por “Dim_Vendedores”» | ✅ | `SM/tables/Dim_Vendedores.tmdl:L1` | — |
| OE2.11 | p.2 · OE2 · Tabla 1 · «Elimine filas superiores y coloque los encabezados correctos» | 🟡 | `SM/tables/Dim_Vendedores.tmdl:L29` encabezados promovidos. No hay paso “Quitar filas superiores” porque el CSV exportado no trae filas basura | Equivalente en resultado; el paso literal de la guía no existe en Pasos aplicados. |
| OE2.12 | p.2 · OE2 · Tabla 1 · «Elimine columnas que no usará en su modelo de datos» | ✅ | `SM/tables/Dim_Vendedores.tmdl:L31` quita Primer Nombre | — |
| OE2.13 | p.2 · OE2 · Tabla 1 · «Revise que el tipo de datos sea el correcto en cada columna» | ✅ | `SM/tables/Dim_Vendedores.tmdl:L35` ID Int64, Nombre text | — |
| OE2.14 | p.2 · OE2 · Consulta Tabla 2 · «Cambie el nombre de la consulta por “Metas_2020”» | ✅ | `SM/expressions.tmdl:L24` | — |
| OE2.15 | p.2 · OE2 · Consulta Tabla 2 · «Elimine filas superiores y coloque los encabezados correctos» (Metas_2020) | 🟡 | `SM/expressions.tmdl:L6` dentro de `fnMetasMensuales`, compartida por las 4 consultas. El CSV no trae filas superiores | Equivalente en resultado; Pasos aplicados de cada Metas_AAAA solo muestra “Origen” y “Metas”. |
| OE2.16 | p.2 · OE2 · Consulta Tabla 2 · «Elimine columnas que no usará (Columna 1)» (Metas_2020) | 🟡 | El CSV exportado no genera “Columna 1”; no hay columna sobrante (sale `ID_vendedor` + 12 meses) | Equivalente en resultado (no hay nada que eliminar). |
| OE2.17 | p.2 · OE2 · Consulta Tabla 2 · «Anule la dinamización de otras columnas, a partir de la columna ID_Vendedor» (Metas_2020) | ✅ | `SM/expressions.tmdl:L10` Table.UnpivotOtherColumns(…, {"ID_Vendedor"}, "Fecha", "Meta") | — |
| OE2.18 | p.2 · OE2 · Consulta Tabla 2 · «Cambie el nombre de las columnas por “Fecha” y “Meta”» (Metas_2020) | ✅ | `SM/expressions.tmdl:L10` | — |
| OE2.19 | p.2 · OE2 · Consulta Tabla 2 · «Cambie el tipo de datos de “Fecha” y “Meta”» (Metas_2020) | ✅ | `SM/expressions.tmdl:L15` Fecha date (d/M/yyyy, en-US); Meta number sin símbolo de moneda | — |
| OE2.20 | p.3 · OE2 · Consulta Tabla 3 · «Cambie el nombre de la consulta por “Metas_2021”» | ✅ | `SM/expressions.tmdl:L36` | — |
| OE2.21 | p.3 · OE2 · Consulta Tabla 3 · «Elimine filas superiores y coloque los encabezados correctos» (Metas_2021) | 🟡 | `SM/expressions.tmdl:L6` dentro de `fnMetasMensuales`, compartida por las 4 consultas. El CSV no trae filas superiores | Equivalente en resultado; Pasos aplicados de cada Metas_AAAA solo muestra “Origen” y “Metas”. |
| OE2.22 | p.3 · OE2 · Consulta Tabla 3 · «Elimine columnas que no usará (Columna 1)» (Metas_2021) | 🟡 | El CSV exportado no genera “Columna 1”; no hay columna sobrante (sale `ID_vendedor` + 12 meses) | Equivalente en resultado (no hay nada que eliminar). |
| OE2.23 | p.3 · OE2 · Consulta Tabla 3 · «Anule la dinamización de otras columnas, a partir de la columna ID_Vendedor» (Metas_2021) | ✅ | `SM/expressions.tmdl:L10` Table.UnpivotOtherColumns(…, {"ID_Vendedor"}, "Fecha", "Meta") | — |
| OE2.24 | p.3 · OE2 · Consulta Tabla 3 · «Cambie el nombre de las columnas por “Fecha” y “Meta”» (Metas_2021) | ✅ | `SM/expressions.tmdl:L10` | — |
| OE2.25 | p.3 · OE2 · Consulta Tabla 3 · «Cambie el tipo de datos de “Fecha” y “Meta”» (Metas_2021) | ✅ | `SM/expressions.tmdl:L15` Fecha date (d/M/yyyy, en-US); Meta number sin símbolo de moneda | — |
| OE2.26 | p.3 · OE2 · Consulta Tabla 4 · «Cambie el nombre de la consulta por “Metas_2022”» | ✅ | `SM/expressions.tmdl:L48` | — |
| OE2.27 | p.3 · OE2 · Consulta Tabla 4 · «Elimine filas superiores y coloque los encabezados correctos» (Metas_2022) | 🟡 | `SM/expressions.tmdl:L6` dentro de `fnMetasMensuales`, compartida por las 4 consultas. El CSV no trae filas superiores | Equivalente en resultado; Pasos aplicados de cada Metas_AAAA solo muestra “Origen” y “Metas”. |
| OE2.28 | p.3 · OE2 · Consulta Tabla 4 · «Elimine columnas que no usará (Columna 1)» (Metas_2022) | 🟡 | El CSV exportado no genera “Columna 1”; no hay columna sobrante (sale `ID_vendedor` + 12 meses) | Equivalente en resultado (no hay nada que eliminar). |
| OE2.29 | p.3 · OE2 · Consulta Tabla 4 · «Anule la dinamización de otras columnas, a partir de la columna ID_Vendedor» (Metas_2022) | ✅ | `SM/expressions.tmdl:L10` Table.UnpivotOtherColumns(…, {"ID_Vendedor"}, "Fecha", "Meta") | — |
| OE2.30 | p.3 · OE2 · Consulta Tabla 4 · «Cambie el nombre de las columnas por “Fecha” y “Meta”» (Metas_2022) | ✅ | `SM/expressions.tmdl:L10` | — |
| OE2.31 | p.3 · OE2 · Consulta Tabla 4 · «Cambie el tipo de datos de “Fecha” y “Meta”» (Metas_2022) | ✅ | `SM/expressions.tmdl:L15` Fecha date (d/M/yyyy, en-US); Meta number sin símbolo de moneda | — |
| OE2.32 | p.3 · OE2 · Consulta Tabla 5 · «Cambie el nombre de la consulta por “Metas_2023”» | ✅ | `SM/expressions.tmdl:L60` | — |
| OE2.33 | p.3 · OE2 · Consulta Tabla 5 · «Elimine filas superiores y coloque los encabezados correctos» (Metas_2023) | 🟡 | `SM/expressions.tmdl:L6` dentro de `fnMetasMensuales`, compartida por las 4 consultas. El CSV no trae filas superiores | Equivalente en resultado; Pasos aplicados de cada Metas_AAAA solo muestra “Origen” y “Metas”. |
| OE2.34 | p.3 · OE2 · Consulta Tabla 5 · «Elimine columnas que no usará (Columna 1)» (Metas_2023) | 🟡 | El CSV exportado no genera “Columna 1”; no hay columna sobrante (sale `ID_vendedor` + 12 meses) | Equivalente en resultado (no hay nada que eliminar). |
| OE2.35 | p.3 · OE2 · Consulta Tabla 5 · «Anule la dinamización de otras columnas, a partir de la columna ID_Vendedor» (Metas_2023) | ✅ | `SM/expressions.tmdl:L10` Table.UnpivotOtherColumns(…, {"ID_Vendedor"}, "Fecha", "Meta") | — |
| OE2.36 | p.3 · OE2 · Consulta Tabla 5 · «Cambie el nombre de las columnas por “Fecha” y “Meta”» (Metas_2023) | ✅ | `SM/expressions.tmdl:L10` | — |
| OE2.37 | p.3 · OE2 · Consulta Tabla 5 · «Cambie el tipo de datos de “Fecha” y “Meta”» (Metas_2023) | ✅ | `SM/expressions.tmdl:L15` Fecha date (d/M/yyyy, en-US); Meta number sin símbolo de moneda | — |
| OE2.38 | p.3 · OE2 · Creación de consulta consolidada · «Seleccione la consulta “Metas_2020”… Anexar consultas para crear una nueva» | ✅ | `SM/tables/Dim_Metas.tmdl:L39` Table.Combine({Metas_2020, …, Metas_2023}) | El gesto de “seleccionar Metas_2020” no es verificable por archivo; el resultado (anexo de las 4) sí. |
| OE2.39 | p.3 · OE2 · Consolidada · «Cambie el nombre por Dim_Metas» | ✅ | `SM/tables/Dim_Metas.tmdl:L1` | — |
| OE2.40 | p.3 · OE2 · Consolidada · «Deshabilite la opción “Habilitar carga” en “Metas_2020”» | ✅ | `SM/expressions.tmdl:L24` definida como `expression` (sin tabla cargada); ausente de las `ref table` de `SM/model.tmdl:L23` solo Dim_Metas carga | Comprobar en Desktop que aparece en cursiva (paso 5 de PASOS_MANUALES). |
| OE2.41 | p.3 · OE2 · Consolidada · «Deshabilite la opción “Habilitar carga” en “Metas_2021”» | ✅ | `SM/expressions.tmdl:L36` definida como `expression` (sin tabla cargada); ausente de las `ref table` de `SM/model.tmdl:L23` solo Dim_Metas carga | Comprobar en Desktop que aparece en cursiva (paso 5 de PASOS_MANUALES). |
| OE2.42 | p.3 · OE2 · Consolidada · «Deshabilite la opción “Habilitar carga” en “Metas_2022”» | ✅ | `SM/expressions.tmdl:L48` definida como `expression` (sin tabla cargada); ausente de las `ref table` de `SM/model.tmdl:L23` solo Dim_Metas carga | Comprobar en Desktop que aparece en cursiva (paso 5 de PASOS_MANUALES). |
| OE2.43 | p.3 · OE2 · Consolidada · «Deshabilite la opción “Habilitar carga” en “Metas_2023”» | ✅ | `SM/expressions.tmdl:L60` definida como `expression` (sin tabla cargada); ausente de las `ref table` de `SM/model.tmdl:L23` solo Dim_Metas carga | Comprobar en Desktop que aparece en cursiva (paso 5 de PASOS_MANUALES). |
| OE2.44 | p.4 · OE2 · Consulta Tabla 6 · «Cambie el nombre de la consulta por “Dim_Territorios”» | ✅ | `SM/tables/Dim_Territorios.tmdl:L1` | — |
| OE2.45 | p.4 · OE2 · Tabla 6 · «Elimine filas superiores y coloque los encabezados correctos» | 🔍 | `SM/tables/Dim_Territorios.tmdl:L47` + `SM/tables/Dim_Territorios.tmdl:L49` | Validar en Desktop (la consulta no se ha confirmado tras la última reescritura). |
| OE2.46 | p.4 · OE2 · Tabla 6 · «Elimine fila(s) inferiores que no contienen datos a usarse en el modelo» | 🔍 | `SM/tables/Dim_Territorios.tmdl:L61` filtra la nota “* Fuente…” y filas sin población | Validar en Desktop: deben quedar 17 filas. |
| OE2.47 | p.4 · OE2 · Tabla 6 · «Elimine columnas que no usará en su modelo de datos» | 🔍 | `SM/tables/Dim_Territorios.tmdl:L58` deja 4 de 6 columnas (quita ISO:NI y Densidad) | Validar en Desktop. |
| OE2.48 | p.4 · OE2 · Tabla 6 · «Revise que el tipo de datos sea el correcto en cada columna» | 🔍 | `SM/tables/Dim_Territorios.tmdl:L64` Población Int64, Superficie number | Validar en Desktop. |
| OE2.49 | p.4 · OE2 · «Cierre y aplique todos los cambios» | 🔍 | Hay valores reales en las capturas de Desktop de Fact, Clientes, Vendedores, Metas y Productos; de Dim_Territorios no hay captura | Hacer Cerrar y aplicar y confirmar que no hay errores (paso 3). |

## OE3 – Tabla Calendario + Modelado de datos

| # | Requisito (cita de la guía) | Estado | Evidencia | Acción requerida |
|---|---|:---:|---|---|
| OE3.1 | p.5 · OE3 · «Usando la opción “Nueva Tabla”, cree una tabla calendario… desde el (2020/1/1) hasta el (2023/12/31)» | ✅ | `SM/tables/Calendario.tmdl:L61`; `SM/tables/Calendario.tmdl:L65` | — |
| OE3.2 | p.5 · OE3 · «…cree las columnas Año…» | ✅ | `SM/tables/Calendario.tmdl:L66` | — |
| OE3.3 | p.5 · OE3 · «…Trimestre» | ✅ | `SM/tables/Calendario.tmdl:L67` formato “1 Trim”, igual que la imagen de la guía | — |
| OE3.4 | p.5 · OE3 · «…Mes_Num» | ✅ | `SM/tables/Calendario.tmdl:L68` | — |
| OE3.5 | p.5 · OE3 · «…Mes_Corto (haga uso de la función DAX Format)» | ✅ | `SM/tables/Calendario.tmdl:L69` | Comprobar en Desktop que septiembre sale “SEP” y no “SEPT” (paso 7). |
| OE3.6 | p.5 · OE3 · «…Mes_Largo (haga uso de la función DAX Format)» | ✅ | `SM/tables/Calendario.tmdl:L70` | — |
| OE3.7 | p.5 · OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Fact_Ventas[ID Cliente] → Dim_Clientes[ID Cliente] | ✅ | `SM/relationships.tmdl:L7` muchos→uno, filtro en un solo sentido, activa (valores por defecto de TMDL); coincide con el diagrama del PDF | — |
| OE3.8 | p.5 · OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Fact_Ventas[ID Producto] → Dim_Productos[ID Producto] | ✅ | `SM/relationships.tmdl:L11` muchos→uno, filtro en un solo sentido, activa (valores por defecto de TMDL); coincide con el diagrama del PDF | — |
| OE3.9 | p.5 · OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Fact_Ventas[ID Vendedor] → Dim_Vendedores[ID Vendedor] | ✅ | `SM/relationships.tmdl:L15` muchos→uno, filtro en un solo sentido, activa (valores por defecto de TMDL); coincide con el diagrama del PDF | — |
| OE3.10 | p.5 · OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Fact_Ventas[Fecha de Venta] → Calendario[Date] | ✅ | `SM/relationships.tmdl:L19` (el diagrama no indica qué columna de fecha se relaciona; se usa Fecha de Venta) muchos→uno, filtro en un solo sentido, activa (valores por defecto de TMDL); coincide con el diagrama del PDF | — |
| OE3.11 | p.5 · OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Dim_Metas[ID_Vendedor] → Dim_Vendedores[ID Vendedor] | ✅ | `SM/relationships.tmdl:L23` muchos→uno, filtro en un solo sentido, activa (valores por defecto de TMDL); coincide con el diagrama del PDF | — |
| OE3.12 | p.5 · OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Dim_Metas[Fecha] → Calendario[Date] | ✅ | `SM/relationships.tmdl:L27` muchos→uno, filtro en un solo sentido, activa (valores por defecto de TMDL); coincide con el diagrama del PDF | — |
| OE3.13 | p.5 · OE3 · «Verifique que las consultas tengan el modelado de datos y conexiones correctas, incluyendo la tabla calendario» · diagrama de la guía: Dim_Clientes[Departamento] → Dim_Territorios[Departamento] | ✅ | `SM/relationships.tmdl:L31` muchos→uno, filtro en un solo sentido, activa (valores por defecto de TMDL); coincide con el diagrama del PDF | — |
| OE3.14 | p.5 · OE3 · «…modelado correcto» · integridad referencial (no la pide la guía, la verifica la auditoría) | ✅ | Cruce de la fuente: 0 ID Cliente, 0 ID Producto y 0 ID Vendedor de Fact_Ventas huérfanos; las fechas de venta (2020-01-02 a 2023-12-30) caen dentro de Calendario | Ninguna |

## OE4 – DAX

| # | Requisito (cita de la guía) | Estado | Evidencia | Acción requerida |
|---|---|:---:|---|---|
| OE4.1 | p.6 · OE4 · «Cree una nueva consulta llamada “Medidas” para agrupar las medidas a crear» | ✅ | `SM/tables/Medidas.tmdl:L1`; `SM/tables/Medidas.tmdl:L53` consulta M en blanco (partition `m`) | — |
| OE4.2 | p.6 · OE4 · «Utilice las funciones SumX, Related, Divide» | ✅ | `SM/tables/Medidas.tmdl:L5`; `SM/tables/Medidas.tmdl:L9`; `SM/tables/Medidas.tmdl:L24` | — |
| OE4.3 | p.6 · OE4 · «a. Total Facturado = Cantidad * Precio Unitario (SumX)» | ✅ | `SM/tables/Medidas.tmdl:L5` SUMX(Fact_Ventas, Cantidad * Precio Unitario) | — |
| OE4.4 | p.6 · OE4 · «b. Costo Total = Cantidad * Costo Unitario (SumX, Related)» | ✅ | `SM/tables/Medidas.tmdl:L10` SUMX + RELATED(Dim_Productos[Costo Unitario]) | — |
| OE4.5 | p.6 · OE4 · «b. Utilidad Total = Total Facturado – Costo total» | ✅ | `SM/tables/Medidas.tmdl:L15`. Cruce de la fuente: 201,686,224.53 − 114,107,426.25 = 87,578,798.28 (Desktop muestra C$87.58 mill.) | — |
| OE4.6 | p.6 · OE4 · «c. % Cumplimiento metas = Total Facturado / SumMetas (Divide)» | 🟡 | `SM/tables/Medidas.tmdl:L25` DIVIDE([Total Facturado], [Meta]) | La guía nombra “SumMetas”; el modelo divide entre `[Meta]` (mismo valor) y no existe ninguna medida llamada SumMetas. |
| OE4.7 | p.6 · OE4 · «d. Meta = SumMetas» | 🟡 | `SM/tables/Medidas.tmdl:L20` SUM(Dim_Metas[Meta]). Suma de metas en la fuente: 269,764,999.68, igual que Desktop | Ídem: falta el nombre “SumMetas” (equivalente en valor). |
| OE4.8 | p.6 · OE4 · «Utilice las funciones de inteligencia de tiempo SAMEPERIODLASTYEAR · a. Ventas del Año Anterior» | ✅ | `SM/tables/Medidas.tmdl:L30` CALCULATE([Total Facturado], SAMEPERIODLASTYEAR(Calendario[Date])) | — |

## OE5 – Visualizaciones + Compartir informe

| # | Requisito (cita de la guía) | Estado | Evidencia | Acción requerida |
|---|---|:---:|---|---|
| OE5.1 | p.7 · OE5 · Página de inicio · «Crear una portada con su nombre» | ✅ | `RP/Inicio/visuals/7891e6ba53b33685ebd6/visual.json:L22` cuadro de texto “Ing. Heydy Mairena” | — |
| OE5.2 | p.7 · OE5 · Inicio · «…título del informe» | ✅ | `RP/Inicio/visuals/d7eae6f5b88872b84645/visual.json:L22` “Informe de Ventas Nacionales 2020-2023” | — |
| OE5.3 | p.7 · OE5 · Inicio · «…y un botón de navegación que lleve a la primera página de Análisis General» | ✅ | `RP/Inicio/visuals/457aa4efc5a37687a61c/visual.json:L119` → `RP/Inicio/visuals/457aa4efc5a37687a61c/visual.json:L126` | — |
| OE5.4 | p.7 · OE5 · Inicio · «Utilice el logo de su preferencia» | ✅ | `RP/Inicio/visuals/b9bba74a5ea34ab52103/visual.json:L47` imagen del recurso registrado | Baja: queda una referencia residual `general.imageUrl → logo.jpeg` que ya no existe (L23). |
| OE5.5 | p.7 · OE5 · Análisis General · Uso de Tarjetas · «Utilice “Tarjetas (nuevas)” para visualizar Facturación Total…» | ✅ | `RP/AnalisisGeneral/visuals/8fee8e02321b6161dbd7/visual.json:L13` con `Total Facturado` | — |
| OE5.6 | p.7 · OE5 · Tarjetas · «…Costo Total…» | ✅ | `RP/AnalisisGeneral/visuals/ab61f6a4669797f6427b/visual.json:L26` (cardVisual) | — |
| OE5.7 | p.7 · OE5 · Tarjetas · «…y Utilidad» | ✅ | `RP/AnalisisGeneral/visuals/f8cb3316cc3062867cc3/visual.json:L26` (cardVisual) | — |
| OE5.8 | p.7 · OE5 · Tarjetas · «**Plus (Puede agregar imágenes png para representar mejor las tarjetas)» | ❌ | La página Análisis General no tiene visuales de imagen (los íconos de las tarjetas se quitaron). Hay SVG listos en `assets/svg/` | Opcional (es un “plus”): añadir los íconos a las tarjetas. |
| OE5.9 | p.7 · OE5 · Uso de Mapa · «Utilice un “Mapa” para representar las Ventas (Total Facturado) por Departamento» | ✅ | `RP/AnalisisGeneral/visuals/b5982da9636de7dfe6ac/visual.json:L13` con `Departamento` y `Total Facturado` | Comprobar visualmente que León aparece en Nicaragua (paso 11). |
| OE5.10 | p.7 · OE5 · Uso de Tabla · «Utilice una “Tabla” para mostrar el cumplimiento de ventas de cada vendedor» | ✅ | `RP/AnalisisGeneral/visuals/00ede76c2e04fc432cd8/visual.json:L13` con Nombre Completo, Total Facturado, Meta y % Cumplimiento | — |
| OE5.11 | p.7 · OE5 · Tabla · «formato condicional… en la columna de % de Cumplimiento» · «Menor al 60%, Rojo» | 🔍 | `RP/AnalisisGeneral/visuals/00ede76c2e04fc432cd8/visual.json:L144` >= 0 y < 0.6 → ícono X (SymbolLow) | Ningún vendedor está por debajo del 60 % en los datos, así que la X no se ha visto en pantalla. Validar el estilo con un filtro o en la ventana “Iconos”. |
| OE5.12 | p.7 · OE5 · Tabla · «Entre 60% al 79% Amarillo» | ✅ | `RP/AnalisisGeneral/visuals/00ede76c2e04fc432cd8/visual.json:L195` >= 0.6 y < 0.8 → ! (SymbolMedium); visto en la captura del usuario (71.5 %, 76.9 %…) | — |
| OE5.13 | p.7 · OE5 · Tabla · «Mayor al 80% Verde» | ✅ | `RP/AnalisisGeneral/visuals/00ede76c2e04fc432cd8/visual.json:L222` >= 0.8 → ✔ (SymbolHigh), igual que la imagen de la guía; visto en la captura (84.0 %, 83.9 %, 80.5 %) | — |
| OE5.14 | p.8 · OE5 · Uso de Medidor · «Utilice la visualización “Medidor” para representar el avance total, relacionando el Total Facturado vs Meta» | ✅ | `RP/AnalisisGeneral/visuals/a7112fe163d6dacd2aef/visual.json:L13` con Y=`Total Facturado`, Max y Objetivo=`Meta` | — |
| OE5.15 | p.8 · OE5 · Medidor · «** Utilice una etiqueta personalizada en el valor del globo que represente el porcentaje de avance» | 🟡 | El valor del globo muestra `Total Facturado` (`RP/AnalisisGeneral/visuals/a7112fe163d6dacd2aef/visual.json:L91`); el % solo está como tooltip (`Etiqueta % Avance`, `RP/AnalisisGeneral/visuals/a7112fe163d6dacd2aef/visual.json:L52`) | Pendiente: etiqueta personalizada en el valor del globo (Prompt de corrección 1). |
| OE5.16 | p.8 · OE5 · Uso de Barras Agrupadas · «Utilice la visualización “Gráfico de barras agrupadas” para representar el Top 5 de clientes por facturación» | ✅ | `RP/AnalisisGeneral/visuals/826d3d0b43c276cb6538/visual.json:L13` con filtro `RP/AnalisisGeneral/visuals/826d3d0b43c276cb6538/visual.json:L153` TopN (Top=5, orden por Total Facturado) | — |
| OE5.17 | p.8 · OE5 · Uso de Segmentaciones · «Utilice… “Chiclet Slicer” de AppSource… por Año» | ✅ | `RP/AnalisisGeneral/visuals/3321249c81bd226877bd/visual.json:L13` (campo `Calendario[Año]`); visual instalado: `pbip/Ventas.Report/definition/report.json:L39` publicCustomVisuals | — |
| OE5.18 | p.8 · OE5 · Segmentaciones · «…y Mes Corto» | ✅ | `RP/AnalisisGeneral/visuals/dc3a8f8200499969ebf4/visual.json:L26` (Chiclet) | — |
| OE5.19 | p.8 · OE5 · Segmentaciones · «Para ordenar la columna de mes_corto debe utilizar la opción ordenar por columna… por la columna mes_num» | ✅ | `SM/tables/Calendario.tmdl:L48` en Mes_Corto y en Mes_Largo | — |
| OE5.20 | p.8 · OE5 · Análisis de Tendencia · Uso de Tarjetas · «Facturación Total» | ✅ | `RP/AnalisisTendencia/visuals/be84d800121607356183/visual.json:L26` (cardVisual) | — |
| OE5.21 | p.8 · OE5 · Tendencia · Tarjetas · «…y Facturación Total del Año Anterior» | ✅ | `RP/AnalisisTendencia/visuals/f37550a9f2f0e357711b/visual.json:L26` (cardVisual) | — |
| OE5.22 | p.8 · OE5 · Uso de KPI · «Utilice la visualización “KPI” para visualizar un comparativo entre las ventas actuales y las ventas del período anterior» | 🔍 | `RP/AnalisisTendencia/visuals/2413073d751b166ce799/visual.json:L13` con Indicator=`Total Facturado`, Goal=`Ventas del Año Anterior`, TrendLine=`Año` (`RP/AnalisisTendencia/visuals/2413073d751b166ce799/visual.json:L52`) | Tras corregir el rol `TrendLine` (changelog 26) no hay captura que confirme que ya pinta (paso 12). |
| OE5.23 | p.8 · OE5 · Uso de Columnas + Líneas · «Utilice la visualización “Gráfico de Columnas Agrupadas y Líneas” para visualizar un comparativo entre la meta y el total facturado por mes» | ✅ | `RP/AnalisisTendencia/visuals/bc9ea33c1b97d3ff6a61/visual.json:L13` con columnas `Meta` y `Total Facturado`; visto en la captura del usuario | — |
| OE5.24 | p.8 · OE5 · Columnas + Líneas · «** En el caso de la línea, represente el % de cumplimiento» | ✅ | `RP/AnalisisTendencia/visuals/bc9ea33c1b97d3ff6a61/visual.json:L97` = `% Cumplimiento metas` | — |
| OE5.25 | p.8 · OE5 · Columnas + Líneas · «** El gráfico debe contener una jerarquía de Año-Trimestre-Mes en su eje X» | ✅ | `RP/AnalisisTendencia/visuals/bc9ea33c1b97d3ff6a61/visual.json:L41`: eje con Año, Trimestre y Mes_Corto, los tres activos; se ve el eje jerárquico en la captura | Nota: son 3 campos en el eje, no una jerarquía creada en el modelo. |
| OE5.26 | p.8 · OE5 · Uso de Segmentaciones (Tendencia) · «Reutilice las segmentaciones de la página anterior» | ✅ | `RP/AnalisisTendencia/visuals/9d8786ab7b4ad3055b92/visual.json:L81` y `RP/AnalisisTendencia/visuals/10d3e50a6525ddc4d538/visual.json:L94`: mismos grupos (`Año`, `Mes_Corto`) que en Análisis General | Comprobar que la selección viaja entre páginas (paso 13). |
| OE5.27 | p.8 · OE5 · «Publique y comparta el enlace de su proyecto en el grupo de trabajo» | ❌ | El proyecto no está publicado (no hay enlace) | Manual en Desktop y en el servicio de Power BI (pasos 17 y 18). |
| OE5.28 | p.8 · OE5 · «El archivo Pbix deberá subirse a la plataforma Classroom» | ❌ | `dist/` solo contiene `TrabajoFinalCursoHeydyMairena.pbix`, que es una carpeta descomprimida con un informe en blanco, no un .pbix válido | Guardar como .pbix desde Desktop y subirlo a Classroom (pasos 16 y 19). |

## Comparación con el checklist anterior (`docs/checklist-requerimiento.md`)

Hallazgos al contrastar el checklist previo con el PDF:

1. **Transcripción incorrecta (OE4, p.6):** el checklist decía `% Cumplimiento metas = Total Facturado / Meta`; la guía dice `/ SumMetas (Divide)`.
2. **Requisitos fusionados:** las cuatro consultas de metas (p.2-3) aparecían como una línea por año con 6 acciones juntas; la guía las lista una a una. Ahora están separadas.
3. **Viñetas que faltaban:** “Utilice las funciones SumX, Related, Divide” (p.6) y la distinción entre las tres reglas de color del formato condicional (p.7).
4. **Notas desactualizadas:** el checklist afirmaba “imágenes png en las tarjetas” (creado) y “tarjeta bajo el medidor con la etiqueta de avance”; ninguno de los dos elementos existe ya en el informe (se quitaron en Desktop).
5. **Estados:** todas las casillas estaban sin marcar aunque había evidencia en archivo y capturas; se actualizan.
6. **Lo que sí estaba bien:** la nota de que la guía dice “ID de clientes” en Dim_Productos (se entiende ID Producto) y el aviso de que el .xls es .xlsx.

## Hallazgos adicionales (incoherencias y calidad)

| # | Severidad | Hallazgo | Evidencia | Acción |
|---|---|---|---|---|
| H1 | Media | El tema personalizado del changelog 29 ya no está aplicado: el informe usa `CopilotDefault`. El archivo `tema-ventas.json` ya no está registrado ni copiado en `StaticResources`. | `pbip/Ventas.Report/definition/report.json` → `themeCollection.customTheme.name = "CopilotDefault"`; `StaticResources/RegisteredResources/` solo contiene `icono-facturacion…jpeg` | Decidir: volver a aplicar `assets/themes/tema-ventas*.json` (Vista > Temas > Examinar temas) o dar de baja los changelogs 29-30. La guía no exige tema. |
| H2 | Media | Etiqueta del medidor: la guía pide una etiqueta personalizada en el valor del globo; hoy el globo muestra `Total Facturado` y el % solo está en el tooltip. | Medidor `a7112fe…`: `calloutValue` sin etiqueta dinámica; rol `Tooltips` = `Etiqueta % Avance` | Prompt de corrección 1. |
| H3 | Baja | No existe una medida llamada `SumMetas`; `Meta` y `% Cumplimiento metas` usan `SUM(Dim_Metas[Meta])` directamente. | `SM/tables/Medidas.tmdl` (medidas `Meta` y `% Cumplimiento metas`) | Prompt de corrección 2 (opcional). |
| H4 | Baja | Las consultas `Metas_AAAA` solo muestran 2 pasos (“Origen” y “Metas”); los 6 pasos de la guía viven dentro de la función `fnMetasMensuales`. Un revisor que abra los Pasos aplicados no los verá uno a uno. | `SM/expressions.tmdl` (`fnMetasMensuales` y `Metas_2020…2023`) | Prompt de corrección 3 (opcional). |
| H5 | Baja | Imagen de la portada con una referencia residual a un recurso inexistente (`general.imageUrl → logo.jpeg`) además de la imagen válida. | `RP/Inicio/visuals/b9bba74a5ea34ab52103/visual.json` | Prompt de corrección 4. |
| H6 | Baja | Rejilla irregular en Análisis General: las tarjetas miden 283, 293 y 253 px de ancho; las segmentaciones empiezan en y=0 (pegadas al borde superior) en ambas páginas; los botones inferiores miden 124-146 px de ancho y 31-37 de alto. | Posiciones en cada `visual.json` (ver tabla de visuales en el changelog 34) | Prompt de corrección 5. |
| H7 | Baja | Visuales sin título visible: tarjetas (5), segmentaciones (4). Los demás visuales de datos sí tienen título. No lo pide la guía. | `visualContainerObjects.title` ausente en las tarjetas y segmentaciones | Opcional. |
| H8 | Info | El modelo conserva la fecha automática de Power BI: tablas ocultas `LocalDateTable_…` y `DateTableTemplate_…` y una relación `Dim_Clientes[Fecha de Alta] → LocalDateTable`. El diagrama de la guía también muestra el ícono de calendario en esa columna. | `SM/relationships.tmdl` (primera relación); `SM/model.tmdl` (`__PBI_TimeIntelligenceEnabled`) | Opcional: Archivo > Opciones > Carga de datos > desmarcar “Fecha y hora automáticas”. |
| H9 | Info | El diagrama de la guía conserva columnas que el modelo quitó: `Fecha de Envío` e `ID Factura` (Fact_Ventas), `Primer Nombre` (Dim_Vendedores), `ISO:NI` y `Densidad` (Dim_Territorios). Las viñetas piden eliminar las que no se usen, así que no es incumplimiento. | Imagen de la p.5 frente a `SM/tables/*.tmdl` | Ninguna; solo estar al tanto si se compara con el ejemplo. |
| H10 | Info | La carpeta `dist/TrabajoFinalCursoHeydyMairena.pbix` no es un .pbix, es un proyecto descomprimido con una página en blanco. Puede confundirse con el entregable. | `dist/TrabajoFinalCursoHeydyMairena.pbix/Report/Layout` | Retirarla cuando exista el .pbix real. |
| H11 | Info | Páginas extra no pedidas por la guía: `TooltipMapa` y `TooltipBarras` (ocultas), segunda botonera de navegación, medida `Etiqueta % Avance`, función `fnMetasMensuales`. | `RP/TooltipMapa`, `RP/TooltipBarras` | Ninguna; no incumplen nada. |
| H12 | Info | Sin errores de nombres: las consultas y medidas usan exactamente los de la guía (`Dim_Vendedores`, `Metas_2020…2023`, `Dim_Metas`, `Dim_Territorios`, `Medidas`, `Total Facturado`, `Costo Total`, `Utilidad Total`, `% Cumplimiento metas`, `Meta`, `Ventas del Año Anterior`). Sin columnas huérfanas en relaciones ni relaciones inactivas o ambiguas. | `SM/tables`, `SM/relationships.tmdl` | Ninguna. |

## Comprobaciones independientes con los datos de origen

Se recalcularon desde `data/raw/` y los CSV de G.Drive, sin pasar por Power BI:

| Comprobación | Resultado de la fuente | Valor en Desktop (capturas del usuario) |
|---|---|---|
| Total Facturado (Σ Cantidad × Precio, 266 128 filas) | C$201,686,224.53 | C$201,686,224.53 |
| Costo Total (Σ Cantidad × Costo del producto) | C$114,107,426.25 | C$114.11 mill. |
| Utilidad Total | C$87,578,798.28 | C$87.58 mill. |
| Meta (Σ de los 4 años de metas, 19 vendedores × 12 meses) | C$269,764,999.68 | C$269,764,999.68 |
| % Cumplimiento total | 74.8 % | 74.8 % |
| ID Cliente / ID Producto / ID Vendedor de Fact_Ventas sin dimensión | 0 / 0 / 0 | — |
| Departamentos de Dim_Clientes (tras el reemplazo) frente a Wikipedia | 17 = 17, sin diferencias | — |
| Rango de Fecha de Venta | 2020-01-02 a 2023-12-30 (dentro de Calendario) | — |

## Prompts de corrección priorizados

Los errores corregibles por archivo **no** se tocaron en esta pasada. En orden de prioridad:

1. **[Alta] Etiqueta personalizada del medidor (H2)**  
   «En `pbip/Ventas.Report`, página Análisis General, medidor `a7112fe163d6dacd2aef`: haz que el valor del globo muestre el porcentaje de avance como etiqueta personalizada (por ejemplo la medida `Etiqueta % Avance` o `% Cumplimiento metas` con un decimal) usando las propiedades de etiqueta dinámica de `calloutValue`. Contrasta los nombres de propiedades con el esquema de temas de Power BI antes de escribirlos y conserva el resto de ajustes del visual. Documenta con la convención.»
2. **[Media] Decisión sobre el tema (H1)**  
   «Decide con el usuario si el informe usa `tema-ventas` (o una variante). Si sí: copia el tema elegido a `StaticResources/RegisteredResources`, registra como `CustomTheme` y referencia en `report.json`, validando contra el esquema. Si no: marca los changelogs 29 y 30 como “no aplicado”.»
3. **[Media] Medida SumMetas (H3)**  
   «En `Medidas`, crea la medida oculta `SumMetas = SUM(Dim_Metas[Meta])` con formato de moneda, y haz que `Meta = [SumMetas]` y `% Cumplimiento metas = DIVIDE([Total Facturado], [SumMetas])`, como en la guía p.6. No cambies ningún otro cálculo.»
4. **[Baja] Pasos visibles en Metas_AAAA (H4)**  
   «Expande `fnMetasMensuales` dentro de cada consulta `Metas_2020…2023` con los pasos nombrados y comentados de la guía (promover encabezados, renombrar, anular dinamización, renombrar a Fecha y Meta, tipos), manteniendo `Metas_AAAA` sin carga y `Dim_Metas` con `Table.Combine`. Verifica que `Dim_Metas` sigue dando 912 filas y 269,764,999.68.»
5. **[Baja] Imágenes en tarjetas (Plus, requisito 5.x)**  
   «En Análisis General coloca imágenes a la izquierda de las tarjetas Total Facturado, Costo Total y Utilidad Total, usando los PNG o SVG de `assets/` (`icono-facturado.svg`, `icono-costo.svg`, `icono-utilidad.svg`, convertidos a PNG si Desktop no admite SVG), sin mover las tarjetas ya ajustadas.»
6. **[Baja] Cuadrícula y limpieza (H5, H6)**  
   «Alinea en Análisis General y Análisis de Tendencia las tarjetas (mismo ancho), las segmentaciones (y=16 en lugar de 0) y los botones inferiores (mismo tamaño); elimina la referencia residual `general.imageUrl → logo.jpeg` de la imagen de Inicio. Solo posiciones y propiedades, ningún campo.»
7. **[Baja] Documentación**  
   «Sincroniza `docs/modelo/diccionario.md` y los changelogs 29-30 con el estado real tras las decisiones anteriores.»

