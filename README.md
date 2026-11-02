# Informe de ventas 2020-2023

Proyecto final del Máster en Power BI. Informe de ventas nacionales (Nicaragua) de una empresa de productos varios: alcance de las ventas por departamento, cumplimiento de metas por vendedor, comportamiento por año, trimestre y mes, y comparativos con el año anterior.

**Autor:** Ing. Heydy Mairena

El requerimiento completo está en [`docs/requerimiento/`](docs/requerimiento/) (la guía `GUIA DE TRABAJO FINAL.pdf` es la única fuente de verdad del proyecto).

## Contenido del informe

| Página | Qué muestra |
|---|---|
| **Inicio** | Portada con logo, título, autor y botones de navegación |
| **Análisis General** | Tarjetas de Total Facturado, Costo Total y Utilidad; mapa de ventas por departamento; tabla de cumplimiento por vendedor con íconos (✗ / ! / ✔); medidor de avance frente a la meta; Top 5 de clientes; segmentaciones Chiclet de Año y Mes |
| **Análisis de Tendencia** | Tarjetas de Total Facturado y Ventas del Año Anterior; KPI frente al año anterior; columnas y línea (meta, facturado y % de cumplimiento) con eje Año-Trimestre-Mes; las mismas segmentaciones, sincronizadas |

Reglas del formato condicional del % de cumplimiento: menor a 60 % ✗, de 60 % a menos de 80 % !, 80 % o más ✔.

## Modelo de datos

**Fuentes**
- `Fact_Ventas`: `data/raw/Fact_Ventas.xlsx` (266 128 filas).
- `Dim_Clientes` y `Dim_Productos`: CSV en `data/raw/` (442 y 227 filas).
- `Dim_Vendedores` y `Dim_Metas`: libro público de Google Drive "Vendedores + Metas_Mensuales", leído como origen Web (CSV exportado de cada hoja). Las metas de 2020 a 2023 son consultas intermedias (`Metas_2020…2023`, sin carga) que `Dim_Metas` anexa: 912 filas.
- `Dim_Territorios`: tabla de Organización Territorial de Wikipedia (17 departamentos).
- `Calendario`: tabla calculada del 2020-01-01 al 2023-12-31, con Año, Trimestre, Mes_Num, Mes_Corto y Mes_Largo; marcada como tabla de fechas.

**Relaciones** (todas de muchos a uno, con filtro en un solo sentido): `Fact_Ventas` con `Dim_Clientes`, `Dim_Productos`, `Dim_Vendedores` y `Calendario` (por Fecha de Venta); `Dim_Metas` con `Dim_Vendedores` y `Calendario`; `Dim_Clientes` con `Dim_Territorios` por Departamento. Diagrama en [`docs/modelo/diagrama-relaciones.md`](docs/modelo/diagrama-relaciones.md).

**Medidas** (consulta `Medidas`): `Total Facturado` (SUMX), `Costo Total` (SUMX + RELATED), `Utilidad Total`, `SumMetas` (oculta), `Meta`, `% Cumplimiento metas` (DIVIDE), `Ventas del Año Anterior` (SAMEPERIODLASTYEAR) y `Etiqueta % Avance` (texto para el tooltip del medidor).

Totales de referencia sin filtros: Total Facturado C$201,686,224.53, Costo Total C$114,107,426.25, Utilidad C$87,578,798.28, Meta C$269,764,999.68 y cumplimiento del 74.8 %. Se comprobaron recalculando desde los archivos de origen.

Diccionario de datos completo: [`docs/modelo/diccionario.md`](docs/modelo/diccionario.md).

## Estructura del repositorio

```
├── CLAUDE.md                 reglas permanentes para Claude Code
├── README.md
├── data/raw/                 Fact_Ventas.xlsx, Dim_Clientes.csv, Dim_Productos.csv
├── assets/
│   ├── svg/                  fondos de portada y análisis, íconos de tarjetas y logo (SVG)
│   ├── themes/               tema de Power BI y variantes (claro, oscuro, corporativo)
│   └── icono-*.jpeg          íconos originales
├── docs/
│   ├── requerimiento/        guía en PDF y enlaces de las fuentes
│   ├── modelo/               diccionario de datos y diagrama de relaciones
│   ├── changelog/            un archivo por cambio (01 a 36)
│   ├── CHANGELOG.md          índice de cambios
│   ├── CHECKLIST_FINAL.md    auditoría contra la guía, requisito por requisito
│   ├── checklist-requerimiento.md
│   └── PASOS_MANUALES_DESKTOP.md
├── prompts/                  copia de cada prompt usado (trazabilidad)
├── pbip/                     proyecto de Power BI en formato PBIP
│   ├── Ventas.pbip
│   ├── Ventas.SemanticModel/ modelo en TMDL
│   └── Ventas.Report/        informe en PBIR
└── dist/                     entregable (.pbix)
```

## Cómo abrirlo

1. Requisitos: Power BI Desktop con la opción de proyectos de Power BI (PBIP) y el visual **Chiclet Slicer** de AppSource.
2. Abrir `pbip/Ventas.pbip` (`Archivo` > `Abrir informe` > `Examinar este dispositivo`).
3. Las tres fuentes locales llevan **la ruta absoluta escrita dentro de la consulta** (`C:\Users\wiljo\OneDrive\Escritorio\Trabajo final de Power BI\data\raw\…`). Si mueves la carpeta del proyecto, edítala en `Transformar datos` > `Editor avanzado` de `Fact_Ventas`, `Dim_Clientes` y `Dim_Productos`.
4. Las fuentes de Google Drive y Wikipedia necesitan internet y permiso anónimo (`Archivo` > `Opciones y configuración` > `Configuración de origen de datos`).
5. `Inicio` > `Actualizar` para cargar los datos.

## Cómo generar el .pbix

Con el informe abierto y actualizado: `Archivo` > `Guardar como` > tipo `Archivos de Power BI (*.pbix)` > carpeta `dist/`. El entregable actual es `dist/Proyecto-HeydyMairena.pbix`.

## Estado

La auditoría del 2026-11-01 contra la guía (103 requisitos evaluados, sin contar la publicación) dio un **90 % cumplido**: OE1 100 %, OE2 90 %, OE3 100 %, OE4 100 %, OE5 81 %. Detalle en [`docs/CHECKLIST_FINAL.md`](docs/CHECKLIST_FINAL.md).

Pendiente o por validar en Desktop:
- Confirmar que `Dim_Territorios` carga con 17 filas y 4 columnas, la regla roja de los íconos (ningún vendedor está bajo 60 %) y el KPI.
- Etiqueta personalizada con el % de avance en el valor del globo del medidor: no se pudo hacer por archivo (la etiqueta dinámica salía como “(En blanco)”); queda como paso manual opcional.
- Imágenes en las tarjetas (el “plus” de la guía): se configuran en Desktop.
- Publicar el informe y compartir el enlace, y subir el `.pbix` a Classroom.

Los pasos exactos, con la ruta de menú de cada uno, están en [`docs/PASOS_MANUALES_DESKTOP.md`](docs/PASOS_MANUALES_DESKTOP.md).

## Forma de trabajo

Cada cambio se documenta con un archivo numerado en `docs/changelog/`, una línea en `docs/CHANGELOG.md`, el checklist actualizado, la copia del prompt en `prompts/` y un commit con el formato `feat(OEx): …`, `fix(OEx): …` o `docs: …`. Las reglas completas están en [`CLAUDE.md`](CLAUDE.md).
