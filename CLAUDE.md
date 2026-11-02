# Proyecto Final Máster Power BI – Informe de ventas 2020-2023

## Contexto
Informe de ventas nacionales (Nicaragua) 2020-2023. El requerimiento completo está en
docs/requerimiento/. El proyecto vive en pbip/ (formato .pbip: TMDL + PBIR).
El entregable final es un .pbix que generará el usuario desde Power BI Desktop.

## Reglas
- Edita solo archivos dentro de pbip/, docs/, prompts/ y assets/.
- Trabaja en español: nombres de tablas, columnas y medidas según el requerimiento
  (Dim_Clientes, Dim_Productos, Dim_Vendedores, Dim_Metas, Dim_Territorios,
  Calendario, Fact_Ventas, Medidas).
- Antes de editar, lee los archivos TMDL/JSON existentes y respeta su formato.
- No inventes nombres de columnas: inspecciona los datos reales en data/raw/.
- Haz cambios pequeños. Si algo es demasiado frágil de hacer por archivo
  (p. ej. un visual complejo), dímelo y dame los pasos manuales para Desktop.
- No cierres ni abras Power BI Desktop; yo valido en Desktop.

## Convención de documentación (OBLIGATORIA tras cada prompt)
1. Crea docs/changelog/NN-slug.md (NN = siguiente número correlativo) con:
   - Fecha y objetivo del requerimiento atendido (OE1..OE5)
   - Qué se hizo (resumen)
   - Archivos creados/modificados
   - Código clave (M o DAX) en bloques
   - Cómo validar en Power BI Desktop
   - Pendientes o riesgos
2. Añade una línea al índice docs/CHANGELOG.md.
3. Actualiza docs/checklist-requerimiento.md marcando lo cumplido.
4. Si cambió el modelo, actualiza docs/modelo/diccionario.md.
5. Guarda el prompt recibido en prompts/NN-slug.md.
6. Haz commit: "feat(OEx): descripción corta" (o fix/docs).

## Autor del documento:
Heydy Mairena