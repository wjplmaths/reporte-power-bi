# 34 – Auditoría final contra la guía

**Fecha:** 2026-11-01
**Objetivo atendido:** OE1 a OE5 (auditoría transversal). Esta pasada es de solo lectura sobre `pbip/`.

## Qué se hizo
- Se releyó el PDF completo (9 páginas) y se extrajeron sus 7 imágenes: logo, tabla de ejemplo del Calendario, diagrama del modelo, cuadro de formato condicional por íconos, ícono de Power BI y la frase de cierre.
- Se descompuso la guía en **104 requisitos**, uno por viñeta, sin fusionar (las cuatro consultas de metas, por ejemplo, van cada una con sus seis acciones).
- Cada requisito se contrastó con el contenido real de `pbip/` (TMDL y PBIR), con la línea del archivo como evidencia.
- Se contrastó el checklist anterior con el PDF.
- Se hicieron cruces independientes con los datos de origen.

## Resultado
| Objetivo | Requisitos | ✅ | 🟡 | 🔍 | ❌ | % cumplido |
|---|---:|---:|---:|---:|---:|---:|
| OE1 | 5 | 5 | 0 | 0 | 0 | 100 % |
| OE2 | 49 | 35 | 9 | 5 | 0 | 71 % |
| OE3 | 14 | 14 | 0 | 0 | 0 | 100 % |
| OE4 | 8 | 6 | 2 | 0 | 0 | 75 % |
| OE5 | 28 | 22 | 1 | 2 | 3 | 79 % |
| **Total** | **104** | **82** | **12** | **7** | **3** | **79 %** (85 % ponderando los 🟡 a la mitad) |

Los detalles, la cita de la guía y la evidencia de cada requisito están en `docs/CHECKLIST_FINAL.md`.

## Lo más relevante
1. **Pendientes reales (❌):** publicar y compartir el enlace, el `.pbix` en `dist/` y subirlo a Classroom (pasos manuales), y las imágenes en las tarjetas (es un “plus”, y se quitaron en Desktop).
2. **Parciales (🟡) de contenido:** la etiqueta personalizada del valor del globo del medidor (hoy solo hay tooltip) y el nombre `SumMetas` de OE4, que no existe como medida aunque `Meta` da el mismo valor.
3. **Parciales de método (OE2, 9):** la guía pide quitar filas superiores y “Columna 1” en `Dim_Vendedores` y `Metas_AAAA`, pero el CSV exportado de G.Drive no las trae. El resultado es el mismo y el paso literal no existe.
4. **Por validar en Desktop (🔍):** `Dim_Territorios` (4 viñetas y el “Cerrar y aplicar”), la regla roja de los íconos (ningún vendedor está bajo 60 %) y el KPI tras corregir su rol.
5. **Incoherencia detectada:** el tema `tema-ventas` (changelogs 29-30) ya no está aplicado; el informe usa `CopilotDefault`.
6. **Riesgo descartado:** los cruces con los datos de origen dan **0 huérfanos** (clientes, productos y vendedores de `Fact_Ventas` están todos en sus dimensiones) y los 17 departamentos de `Dim_Clientes` coinciden con los de Wikipedia. Los totales calculados en Python coinciden al centavo con los de Desktop: C$201,686,224.53 facturado y C$269,764,999.68 de metas.

## Discrepancias del checklist anterior
- `% Cumplimiento metas` estaba transcrito con `/ Meta`; la guía dice `/ SumMetas`.
- Requisitos fusionados (metas) y viñetas ausentes (“SumX, Related, Divide”).
- Notas obsoletas: “imágenes png en tarjetas” y “tarjeta bajo el medidor” ya no existen en el informe.
- Todas las casillas estaban sin marcar; `docs/checklist-requerimiento.md` se reescribió con los estados reales.

## Diseño de los visuales (referencia para el hallazgo H6)
Posiciones guardadas por Desktop (px, redondeadas):

| Página | Visual | x | y | ancho | alto |
|---|---|---:|---:|---:|---:|
| Inicio | image | 530 | 80 | 220 | 220 |
| Inicio | texto | 140 | 330 | 1000 | 90 |
| Inicio | texto | 240 | 425 | 800 | 66 |
| Inicio | botón a AnalisisGeneral | 490 | 520 | 300 | 56 |
| Inicio | botón a AnalisisTendencia | 490 | 590 | 300 | 56 |
| Análisis General | chiclet Año | 20 | 0 | 500 | 86 |
| Análisis General | chiclet Mes_Corto | 529 | 0 | 730 | 86 |
| Análisis General | tarjeta Total Facturado | 20 | 97 | 284 | 110 |
| Análisis General | tarjeta Costo Total | 347 | 97 | 294 | 111 |
| Análisis General | tarjeta Utilidad Total | 673 | 97 | 253 | 111 |
| Análisis General | gauge | 941 | 97 | 320 | 177 |
| Análisis General | azureMap | 20 | 218 | 375 | 482 |
| Análisis General | tableEx | 405 | 218 | 522 | 482 |
| Análisis General | clusteredBarChart | 941 | 285 | 321 | 369 |
| Análisis General | botón a Inicio | 941 | 663 | 147 | 38 |
| Análisis General | botón a AnalisisTendencia | 1120 | 663 | 139 | 37 |
| Análisis de Tendencia | chiclet Año | 21 | 0 | 499 | 81 |
| Análisis de Tendencia | chiclet Mes_Corto | 530 | 0 | 730 | 81 |
| Análisis de Tendencia | tarjeta Total Facturado | 20 | 96 | 300 | 150 |
| Análisis de Tendencia | tarjeta Ventas del Año Anterior | 330 | 96 | 300 | 150 |
| Análisis de Tendencia | kpi | 639 | 96 | 620 | 150 |
| Análisis de Tendencia | lineClusteredColumnComboChart | 20 | 258 | 1239 | 418 |
| Análisis de Tendencia | botón a AnalisisGeneral | 1135 | 681 | 124 | 33 |
| Análisis de Tendencia | botón a Inicio | 994 | 682 | 125 | 32 |

No hay solapes entre visuales de datos. Lo irregular: anchos de tarjeta distintos en Análisis General (284, 294, 253), segmentaciones pegadas al borde superior (y = 0) y botones inferiores de tamaño desigual.

## Archivos creados/modificados
- `docs/CHECKLIST_FINAL.md` (nuevo)
- `docs/PASOS_MANUALES_DESKTOP.md` (nuevo)
- `docs/checklist-requerimiento.md` (reescrito con los estados reales)
- `docs/changelog/34-auditoria-final.md`, `docs/CHANGELOG.md`, `prompts/34-auditoria-final.md`

No se modificó nada de `pbip/`. Los cambios de formato que Desktop deja guardados en `pbip/` quedan sin incluir en este commit.

## Cómo validar
1. Abrir `docs/CHECKLIST_FINAL.md` y recorrer el resumen y las tablas por objetivo.
2. Seguir `docs/PASOS_MANUALES_DESKTOP.md` en orden (pasos 1 a 19).
3. Decidir qué prompts de corrección ejecutar, en el orden propuesto al final del checklist.

## Pendientes o riesgos
- Los estados ✅ se basan en el contenido de los archivos; solo los marcados con “visto en la captura del usuario” tienen además evidencia visual. Todo lo demás se confirma al abrir en Desktop (paso 3 en adelante).
- Los porcentajes dependen de cómo se cuenten las viñetas (104 aquí, con las de metas separadas); no son una nota oficial.
- Lista priorizada de correcciones (sin ejecutar): 1) etiqueta personalizada del medidor, 2) decidir el tema, 3) medida `SumMetas`, 4) pasos visibles en `Metas_AAAA`, 5) imágenes en tarjetas, 6) cuadrícula y limpieza, 7) sincronizar documentación.
