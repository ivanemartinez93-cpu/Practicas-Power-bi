# Módulo 8 — Checkpoint 2: Modelo de datos y medidas DAX

## Entregable evaluable

**Archivo principal:** `Martinez_Ivan_Checkpoint2.pbix`

Este archivo continúa el trabajo realizado en el módulo anterior y contiene el modelo de datos de RetailPro con relaciones activas, tabla calendario, tabla exclusiva de medidas DAX y una página de validación.

> Para corregir esta entrega, abrir `Martinez_Ivan_Checkpoint2.pbix` en Power BI Desktop y seguir la sección **Cómo verificar cada criterio de la rúbrica**.

---

## 1. Modelo de datos

El modelo contiene una tabla de hechos y cuatro dimensiones principales:

- `Fact_Ventas`
- `Dim_Clientes`
- `Dim_Productos`
- `Dim_Categorias`
- `Dim_Fechas`

Además, existe la tabla `_Medidas`, utilizada exclusivamente como contenedor de medidas DAX.

### Relaciones implementadas

| Tabla dimensión | Columna | Relación | Tabla destino | Columna |
|---|---|---|---|---|
| `Dim_Clientes` | `id_cliente` | 1:N | `Fact_Ventas` | `id_cliente` |
| `Dim_Productos` | `id_producto` | 1:N | `Fact_Ventas` | `id_producto` |
| `Dim_Categorias` | `id_categoria` | 1:N | `Dim_Productos` | `id_categoria` |
| `Dim_Fechas` | `Date` | 1:N | `Fact_Ventas` | `fecha_venta` |

Las relaciones fueron configuradas como **activas** y con **dirección de filtro única**.

---

## 2. Tabla calendario

Se creó `Dim_Fechas` mediante DAX tomando el rango mínimo y máximo de fechas de ventas.

```DAX
Dim_Fechas =
CALENDAR(
    MIN(Fact_Ventas[fecha_venta]),
    MAX(Fact_Ventas[fecha_venta])
)
```

Columnas calculadas incluidas:

- `Año`
- `Mes Número`
- `Mes Nombre`
- `Trimestre`
- `Semana`

`Dim_Fechas` fue marcada como **tabla de fechas** usando la columna `Date`.

---

## 3. Tabla de medidas core

La tabla `_Medidas` se utiliza exclusivamente para almacenar medidas DAX.

### Medidas implementadas

#### Total Ventas

```DAX
Total Ventas =
SUM(Fact_Ventas[total_venta])
```

#### Ventas Online

```DAX
Ventas Online =
CALCULATE(
    [Total Ventas],
    Fact_Ventas[canal] = "Online"
)
```

#### Ventas YTD

```DAX
Ventas YTD =
TOTALYTD(
    [Total Ventas],
    Dim_Fechas[Date]
)
```

#### Ventas LY

```DAX
Ventas LY =
CALCULATE(
    [Total Ventas],
    SAMEPERIODLASTYEAR(Dim_Fechas[Date])
)
```

#### % Crecimiento Anual

```DAX
% Crecimiento Anual =
VAR VentasActual = [Total Ventas]
VAR VentasAnterior = [Ventas LY]
RETURN
    DIVIDE(
        VentasActual - VentasAnterior,
        VentasAnterior
    )
```

La medida `% Crecimiento Anual` está formateada como porcentaje.

---

## 4. Página de validación

El archivo contiene una página llamada **Validación** con una matriz configurada de la siguiente forma:

- **Filas:** `Dim_Fechas[Mes Nombre]`
- **Columnas:** `Dim_Fechas[Año]`
- **Valores:**
  - `Total Ventas`
  - `Ventas YTD`
  - `Ventas LY`
  - `% Crecimiento Anual`

La matriz permite comprobar visualmente:

- que enero YTD coincide con enero de Total Ventas;
- que febrero YTD acumula enero + febrero;
- que Ventas LY muestra el período comparable del año anterior;
- que cuando no existe período anterior se obtiene BLANK;
- que el crecimiento anual se calcula sobre ventas actuales vs. ventas del año anterior.

---

# Cómo verificar cada criterio de la rúbrica

## Criterio 1 — Relaciones 1:N activas y dirección única

1. Abrir `Martinez_Ivan_Checkpoint2.pbix`.
2. Ir a **Vista de modelo**.
3. Verificar las cuatro relaciones listadas en este README.
4. Abrir las propiedades de cada relación y confirmar:
   - cardinalidad **Uno a varios (1:N)**;
   - dirección de filtro cruzado **Única**;
   - relación **Activa**.

## Criterio 2 — Dim_Fechas marcada como tabla de fechas

1. Seleccionar `Dim_Fechas`.
2. Confirmar que está marcada como **tabla de fechas**.
3. Verificar que la columna utilizada es `Date`.
4. Confirmar su relación con `Fact_Ventas[fecha_venta]`.

## Criterio 3 — Tabla _Medidas exclusiva

1. Ubicar `_Medidas` en el panel de datos.
2. Verificar que contiene las cinco medidas indicadas.
3. Confirmar que se utiliza como contenedor de medidas y no como tabla de datos de negocio.

## Criterio 4 — Ventas YTD

1. Abrir la página **Validación**.
2. Comparar `Total Ventas` y `Ventas YTD`.
3. Enero debe coincidir con Total Ventas de enero.
4. Los meses siguientes deben acumular los meses anteriores dentro del mismo año.

## Criterio 5 — Ventas LY

1. En la matriz de **Validación**, revisar `Ventas LY`.
2. Para 2024 debe mostrar los valores correspondientes a 2023.
3. Para períodos sin año anterior comparable debe mostrar BLANK.

## Criterio 6 — % Crecimiento Anual con VAR y DIVIDE

1. Seleccionar la medida `% Crecimiento Anual`.
2. Revisar su expresión DAX.
3. Confirmar el uso de:
   - `VAR VentasActual`
   - `VAR VentasAnterior`
   - `DIVIDE()`

---

## Estructura de esta entrega

```text
Modulo_8/
├── Martinez_Ivan_Checkpoint2.pbix
├── README.md
└── CRITERIOS_VERIFICACION.md
```

El archivo `CRITERIOS_VERIFICACION.md` resume la correspondencia entre cada criterio de evaluación y la evidencia que debe revisarse dentro del PBIX.

---

## Autor

**Iván Emanuel Martínez**
