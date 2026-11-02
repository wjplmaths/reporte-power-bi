Define las relaciones en relationships.tmdl según el diagrama del PDF
(Fact_Ventas con Dim_Clientes, Dim_Productos, Dim_Vendedores y Calendario;
Dim_Metas con Dim_Vendedores y Calendario; Dim_Clientes con Dim_Territorios por
Departamento). Verifica cardinalidad (muchos a uno) y dirección del filtro.
Si hay ambigüedad en la fecha de Fact_Ventas (Fecha de Venta vs Fecha de Envío),
usa Fecha de Venta como relación activa y la otra inactiva. Marca la categoría de datos
de Departamento como "Estado o provincia" para el mapa. Oculta las columnas de
llaves en el panel de campos. Genera un diagrama del modelo en Mermaid en
docs/modelo/. Aplica la convención de documentación.