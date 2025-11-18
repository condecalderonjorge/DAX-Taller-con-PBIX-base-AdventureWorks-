# 🧭 DAX — Taller con PBIX base (AdventureWorks)

**Autor:** Jorge Conde Calderón  
**Tecnologías:** Power BI · DAX · Excel  
**Fecha:** 2025

---

## 🎯 Objetivo del proyecto
Repositorio de medidas DAX y guía de mantenimiento del archivo `Taller DAX archivo base.pbix`.

---

## Contenidos
- `new_measures.dax`: medidas realizadas.
- `clean_measures.dax`: versión normalizada y mejoras.
- `Taller DAX archivo base.pbix`: archivo pbi base de AdventureWorks.
- `Taller DAX cargado.pbix`: archivo pbi con las medidas importadas.


## Requisitos
- Power BI Desktop (Junio 2024 o superior recomendado)
- DAX Studio 3.x y/o Tabular Editor 2/3 (opcional)


## Uso
1. Abre el PBIX.
2. Importa/actualiza medidas del repo.


## Licencia
MIT — ver `LICENSE`.


## 📊 Medidas incluidas

Estas son todas las medidas DAX para usar en el archivo base del taller (`Taller DAX archivo base.pbix`):

```DAX
[% MArgen] = DIVIDE([Ventas Totales], [Coste Total], BLANK()) - 1

[Color Evolucion] = IF([Evolucion] > 0, "#0CBE44", "#F90A0A")

[Color Evolucion 2] = IF([Evolucion] = 0, "#000000", IF([Evolucion] > 0, "#0CBE44", "#F90A0A"))

[Color Evolucion 3] = SWITCH(TRUE(), [Evolucion] = 0, "#000000", [Evolucion] > 0, "#0CBE44", [Evolucion] < 0, "#F90A0A")

[Color Evolucion 4] = 
SWITCH(
    TRUE(),
    [Evolucion] = 1, "#0CBE44",
    [Evolucion] > 0.1, "#E69B19",
    [Evolucion] < 0.1, "#F90A0A"
)

[Coste Total] = SUM(FactSales[TotalProductCost])

[Diff] = [Media] - [Media Estatica]

[Evolucion] = IF([Ventas Totales 2012] = BLANK(), 0, DIVIDE([Ventas Totales 2012], [Ventas Totales 2011]) - 1)

[Margen] = SUM(FactSales[SalesAmount]) - SUM(FactSales[TotalProductCost])

[Margen 2] = [Ventas Totales] - [Coste Total]

[Margen 3] = SUM(FactSales[SalesAmount]) - [Coste Total] - SUM(FactSales[Freight]) - SUM(FactSales[TaxAmt])

[Media] = AVERAGE(FactSales[SalesAmount])

[Media Estatica] = CALCULATE(AVERAGE(FactSales[SalesAmount]), ALL())

[Media Estatica all except] = CALCULATE(AVERAGE(FactSales[SalesAmount]), ALLEXCEPT(DimCurrency, DimCurrency[CurrencyName]))

[Media Estatica Europe] = CALCULATE(AVERAGE(FactSales[SalesAmount]), DimTerritory[SalesTerritoryGroup] = "Europe", ALL())

[Media Por Prducto] = SUM(FactSales[SalesAmount]) / COUNT(DimProduct[ProductKey])

[Media Variables] = 
VAR ventas = SUM(FactSales[SalesAmount USD])
VAR facturas = DISTINCTCOUNT(FactSales[SalesOrderNumber])
RETURN DIVIDE(ventas, facturas, BLANK())

[Mediana] = MEDIAN(FactSales[SalesAmount])

[Mediana Estatica] = CALCULATE(MEDIAN(FactSales[SalesAmount]), ALL())

[Recuento] = COUNT(FactSales[SalesAmount])

[Recuento NUlos] = COUNTBLANK(DimProduct[ProductSubcategoryKey])

[Recuento Productos] = COUNT(DimProduct[SafetyStockLevel])

[Recuento sin nulos] = DISTINCTCOUNTNOBLANK(DimProduct[ProductSubcategoryKey])

[Recuento Subcategoria] = COUNT(DimProduct[ProductSubcategoryKey])

[REcuento Unico] = DISTINCTCOUNT(DimProduct[SafetyStockLevel])

[Recuento Unicos Subcategoria] = DISTINCTCOUNT(DimProduct[ProductSubcategoryKey])

[Selector DAX] =
IF(
    HASONEVALUE(_SEL[Selector]),
    SWITCH(
        VALUES(_SEL[Selector]),
        "Original", [Ventas Totales],
        "USD", [Ventas Totales en USD],
        "Mediana", [Mediana]
    ),
    [Ventas Totales]
)

[Selector DAX Color] =
IF(
    HASONEVALUE(_SEL[Selector]),
    SWITCH(
        VALUES(_SEL[Selector]),
        "Original", "#118DFF",
        "USD", "#FF930F",
        "Mediana", "#258F53"
    ),
    "#118DFF"
)

[Tarjeta Evolucion] = 
IF([Ventas Totales 2012] = BLANK(), "No hay Datos", "La variacion de año a año ha sido de: " & FORMAT([Evolucion], "0.00%"))

[Today] = YEAR(NOW())

[Ventales Total CY] = CALCULATE(SUM(FactSales[SalesAmount]), DimDates[CalendarYear] = YEAR(NOW()))

[Ventales Total PY] = CALCULATE(SUM(FactSales[SalesAmount]), DimDates[CalendarYear] = YEAR(NOW()) - 1)

[Ventales Totales Europa y Pacific] = CALCULATE(SUM(FactSales[SalesAmount]), DimTerritory[SalesTerritoryGroup] IN {"Europe", "Pacific"})

[Ventas Totales] = SUM(FactSales[SalesAmount])

[Ventas Totales 2011] = CALCULATE(SUM(FactSales[SalesAmount]), DimDates[CalendarYear] = 2011)

[Ventas Totales 2012] = CALCULATE(SUM(FactSales[SalesAmount]), DimDates[CalendarYear] = 2012)

[Ventas Totales 2012 (Ship)] = CALCULATE(SUM(FactSales[SalesAmount]), DimDates[CalendarYear] = 2012, USERELATIONSHIP(DimDates[DateKey], FactSales[ShipDateKey]))

[Ventas Totales en USD] = SUM(FactSales[SalesAmount USD])

[ventas totales mes anterior] =
VAR PM = MAX(DimDates[PERIOD]) - 1
RETURN CALCULATE(SUM(FactSales[SalesAmount]), FILTER(ALL(DimDates[PERIOD]), DimDates[PERIOD] = PM))

[Ventas Totales sin Europa ni Pacifico] = CALCULATE(SUM(FactSales[SalesAmount]), NOT DimTerritory[SalesTerritoryGroup] IN {"Europe", "Pacific"})

[Ventas Totales SIN UK] = CALCULATE([Ventas Totales], DimTerritory[SalesTerritoryCountry] <> "United Kingdom")

[Ventas Totales UK] = CALCULATE(SUM(FactSales[SalesAmount]), DimTerritory[SalesTerritoryCountry] = "United Kingdom")

[Ventas Totales UK 2012] = CALCULATE(SUM(FactSales[SalesAmount]), DimTerritory[SalesTerritoryCountry] = "United Kingdom", DimDates[DateKey] >= 20120101, DimDates[DateKey] <= 20121231)

[YTD] = TOTALYTD(SUM(FactSales[SalesAmount]), DimDates[FullDateAlternateKey])

[YTD CAL] =
VAR PM = MAX(DimDates[PERIOD])
RETURN CALCULATE(SUM(FactSales[SalesAmount]), FILTER(ALL(DimDates[PERIOD]), DimDates[PERIOD] <= PM))

[YTD CAL anterior] =
VAR PM = MAX(DimDates[PERIOD]) - 1
RETURN CALCULATE(SUM(FactSales[SalesAmount]), FILTER(ALL(DimDates[PERIOD]), DimDates[PERIOD] <= PM))
