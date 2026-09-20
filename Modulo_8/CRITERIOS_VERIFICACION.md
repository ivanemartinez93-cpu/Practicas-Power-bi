# Evidencia y criterios de verificación — Módulo 8

**Entregable:** `Martinez_Ivan_Checkpoint2.pbix`

Este documento permite verificar de forma directa cada requisito del Checkpoint 2.

| Criterio de aceptación | Implementación | Dónde verificar |
|---|---|---|
| 4 relaciones con cardinalidad 1:N | Clientes→Ventas, Productos→Ventas, Categorías→Productos, Fechas→Ventas | Vista de modelo |
| Dirección de filtro única | Configurada en las 4 relaciones | Propiedades de cada relación |
| Relaciones activas | Las 4 relaciones están activas | Vista de modelo |
| `Dim_Fechas` marcada como tabla de fechas | Columna de fecha: `Date` | Herramientas de tabla / Vista de modelo |
| `Dim_Fechas` relacionada con ventas | `Dim_Fechas[Date]` → `Fact_Ventas[fecha_venta]` | Vista de modelo |
| Tabla `_Medidas` | Contenedor exclusivo de medidas DAX | Panel de datos |
| 5 medidas DAX | Total Ventas, Ventas Online, Ventas YTD, Ventas LY, % Crecimiento Anual | Tabla `_Medidas` |
| Uso de `CALCULATE` | Medida `Ventas Online` | Expresión DAX |
| Inteligencia de tiempo | `TOTALYTD` y `SAMEPERIODLASTYEAR` | Medidas `Ventas YTD` y `Ventas LY` |
| Uso de `VAR` | `% Crecimiento Anual` | Expresión DAX |
| Uso de `DIVIDE` | `% Crecimiento Anual` | Expresión DAX |
| Validación funcional | Matriz por mes y año | Página `Validación` |

## Prueba funcional de la matriz

La página **Validación** utiliza:

- Filas: `Dim_Fechas[Mes Nombre]`
- Columnas: `Dim_Fechas[Año]`
- Valores: `Total Ventas`, `Ventas YTD`, `Ventas LY`, `% Crecimiento Anual`

Controles esperados:

1. Enero YTD = Total Ventas de enero.
2. Febrero YTD = enero + febrero.
3. Ventas LY de 2024 = valores equivalentes de 2023.
4. Ventas LY queda BLANK cuando no existe período anterior.
5. % Crecimiento Anual se muestra con formato de porcentaje.

## Archivo a corregir

No es necesario revisar archivos de módulos anteriores para esta evaluación.  
El archivo correspondiente a este checkpoint es:

`Modulo_8/Martinez_Ivan_Checkpoint2.pbix`
