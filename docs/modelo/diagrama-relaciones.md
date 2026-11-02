# Diagrama del modelo

Relaciones definidas en `pbip/Ventas.SemanticModel/definition/relationships.tmdl`. Todas son de muchos a uno (`*` hacia `1`), con filtro en una sola dirección (del lado "uno" al lado "muchos") y están activas.

```mermaid
erDiagram
    Dim_Clientes    ||--o{ Fact_Ventas : "ID Cliente"
    Dim_Productos   ||--o{ Fact_Ventas : "ID Producto"
    Dim_Vendedores  ||--o{ Fact_Ventas : "ID Vendedor"
    Calendario      ||--o{ Fact_Ventas : "Date = Fecha de Venta"
    Dim_Vendedores  ||--o{ Dim_Metas   : "ID Vendedor = ID_Vendedor"
    Calendario      ||--o{ Dim_Metas   : "Date = Fecha"
    Dim_Territorios ||--o{ Dim_Clientes : "Departamento"

    Fact_Ventas {
        int ID_Transaccion
        int ID_Vendedor FK
        int ID_Cliente FK
        int ID_Producto FK
        date Fecha_de_Venta FK
        int Cantidad
        decimal Precio_Unitario
    }
    Dim_Clientes {
        int ID_Cliente PK
        text Nombre_Cliente
        date Fecha_de_Alta
        text Departamento FK
        text Pais
    }
    Dim_Productos {
        int ID_Producto PK
        text Producto
        text Marca
        decimal Costo_Unitario
    }
    Dim_Vendedores {
        int ID_Vendedor PK
        text Nombre_Completo
    }
    Dim_Metas {
        int ID_Vendedor FK
        date Fecha FK
        decimal Meta
    }
    Calendario {
        date Date PK
        int Anio
        text Trimestre
        int Mes_Num
        text Mes_Corto
        text Mes_Largo
    }
    Dim_Territorios {
        text Departamento PK
        text Cabecera
        int Poblacion_2023
        decimal Superficie_km2
    }
```

## Relaciones

| # | Desde (muchos) | Hacia (uno) | Estado |
|---|---|---|---|
| 1 | `Fact_Ventas[ID Cliente]` | `Dim_Clientes[ID Cliente]` | Activa |
| 2 | `Fact_Ventas[ID Producto]` | `Dim_Productos[ID Producto]` | Activa |
| 3 | `Fact_Ventas[ID Vendedor]` | `Dim_Vendedores[ID Vendedor]` | Activa |
| 4 | `Fact_Ventas[Fecha de Venta]` | `Calendario[Date]` | Activa |
| 5 | `Dim_Metas[ID_Vendedor]` | `Dim_Vendedores[ID Vendedor]` | Activa |
| 6 | `Dim_Metas[Fecha]` | `Calendario[Date]` | Activa |
| 7 | `Dim_Clientes[Departamento]` | `Dim_Territorios[Departamento]` | Activa |

Además existe una relación automática de Power BI, `Dim_Clientes[Fecha de Alta]` hacia una tabla de fecha oculta (`LocalDateTable_*`). No forma parte del diseño.

## Cómo se propagan los filtros
- Un año o mes de `Calendario` filtra tanto `Fact_Ventas` como `Dim_Metas`.
- Un vendedor de `Dim_Vendedores` filtra `Fact_Ventas` y `Dim_Metas`. Así se puede comparar `Total Facturado` con `Meta` por vendedor.
- Un departamento de `Dim_Territorios` filtra `Dim_Clientes` y, a través de él, `Fact_Ventas`. Por eso el mapa de ventas usa `Dim_Territorios[Departamento]`.

## Columnas ocultas
Las llaves no se muestran en el panel de campos: `Fact_Ventas` (`ID Transacción`, `ID Vendedor`, `ID Cliente`, `ID Producto`, `Fecha de Venta`), `Dim_Clientes[ID Cliente]`, `Dim_Productos[ID Producto]`, `Dim_Vendedores[ID Vendedor]`, `Dim_Metas[ID_Vendedor]` y `Dim_Metas[Fecha]`. `Calendario[Date]` queda visible. `Departamento` (en `Dim_Clientes` y `Dim_Territorios`) tiene categoría de datos "Estado o provincia".