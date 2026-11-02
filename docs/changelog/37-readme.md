# 37 – README del proyecto

**Fecha:** 2026-11-01
**Objetivo atendido:** documentación general (OE1–OE5). El `README.md` estaba vacío.

## Qué se hizo
Se escribió el `README.md` de la raíz con:
- Descripción del proyecto y autor (Ing. Heydy Mairena, como indica `CLAUDE.md` y la portada).
- Las tres páginas del informe y sus visuales, y las reglas del formato condicional.
- El modelo de datos: fuentes, relaciones, medidas y totales de referencia (comprobados contra los archivos de origen).
- La estructura real del repositorio.
- Cómo abrir el proyecto (con el aviso de que las rutas de los archivos locales son absolutas), cómo generar el `.pbix` y dónde está el entregable (`dist/Proyecto-HeydyMairena.pbix`).
- El estado según la auditoría (90 % cumplido) y lo pendiente, con enlaces a `CHECKLIST_FINAL.md` y `PASOS_MANUALES_DESKTOP.md`.
- La forma de trabajo y la convención de documentación.

## Archivos creados/modificados
- `README.md`
- `docs/changelog/37-readme.md`, `docs/CHANGELOG.md`, `prompts/37-readme.md`

El `README.md` está en la raíz, fuera de las carpetas que `CLAUDE.md` permite editar por defecto; se editó porque el usuario lo pidió expresamente. No se tocó nada de `pbip/`.

## Cómo validar
Abrir `README.md` en el editor y comprobar que los enlaces relativos (docs, modelo, CLAUDE.md) abren sus archivos.

## Pendientes o riesgos
- La sección “Estado” refleja la auditoría del changelog 36. Los cambios posteriores hechos en Desktop (páginas de tooltip eliminadas, logo de la portada cambiado, `.pbix` generado) no alteran esas cifras, pero conviene repetir la auditoría tras los próximos cambios para actualizar el porcentaje.
- Hay una copia del proyecto dentro de `dist/` (`ProyectoVentas-HeydyMairena.pbip` y sus carpetas) junto al `.pbix`; el README no la describe porque no es parte de la estructura prevista.
