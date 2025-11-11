# DAX — Taller con PBIX base (AdventureWorks)


Repositorio de medidas DAX y guía de mantenimiento del archivo `Taller DAX archivo base.pbix`.


## Contenidos
- `new_measures.dax`: medidas realizadas.
- `clean_measures.dax`: versión normalizada y mejoras.
- `Taller DAX archivo base`: archivo pbi base de AdventureWorks.
- `Taller DAX archivo base con medidas`: archivo pbi con las medidas importadas.


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
MEASURE 'DAX'[% MArgen] = DIVIDE([Ventas Totales], [Coste Total], BLANK()) - 1

MEASURE 'DAX'[Color Evolucion] = IF([Evolucion] > 0, "#0CBE44", "#F90A0A")

MEASURE 'DAX'[Color Evolucion 2] = IF([Evolucion] = 0, "#000000", IF([Evolucion] > 0, "#0CBE44", "#F90A0A"))

MEASURE 'DAX'[Color Evolucion 3] = SWITCH(TRUE(), [Evolucion] = 0, "#000000", [Evolucion] > 0, "#0CBE44", [Evolucion] < 0, "#F90A0A")

MEASURE 'DAX'[Color Evolucion 4] = 
SWITCH(
    TRUE(),
    [Evolucion] = 1, "#0CBE44",
    [Evolucion] > 0.1, "#E69B19",
    [Evolucion] < 0.1, "#F90A0A"
)

MEASURE 'DAX'[Coste Total] = SUM(FactSales[TotalProductCost])

MEASURE 'DAX'[Diff] = [Media] - [Media Estatica]

MEASURE 'DAX'[Evolucion] = IF([Ventas Totales 2012] = BLANK(), 0, DIVIDE([Ventas Totales 2012], [Ventas Totales 2011]) - 1)

MEASURE 'DAX'[Margen] = SUM(FactSales[SalesAmount]) - SUM(FactSales[TotalProductCost])

MEASURE 'DAX'[Margen 2] = [Ventas Totales] - [Coste Total]

MEASURE 'DAX'[Margen 3] = SUM(FactSales[SalesAmount]) - [Coste Total] - SUM(FactSales[Freight]) - SUM(FactSales[TaxAmt])

MEASURE 'DAX'[Media] = AVERAGE(FactSales[SalesAmount])

MEASURE 'DAX'[Media Estatica] = CALCULATE(AVERAGE(FactSales[SalesAmount]), ALL())

MEASURE 'DAX'[Media Estatica all except] = CALCULATE(AVERAGE(FactSales[SalesAmount]), ALLEXCEPT(DimCurrency, DimCurrency[CurrencyName]))

MEASURE 'DAX'[Media Estatica Europe] = CALCULATE(AVERAGE(FactSales[SalesAmount]), DimTerritory[SalesTerritoryGroup] = "Europe", ALL())

MEASURE 'DAX'[Media Por Prducto] = SUM(FactSales[SalesAmount]) / COUNT(DimProduct[ProductKey])

MEASURE 'DAX'[Media Variables] = 
VAR ventas = SUM(FactSales[SalesAmount USD])
VAR facturas = DISTINCTCOUNT(FactSales[SalesOrderNumber])
RETURN DIVIDE(ventas, facturas, BLANK())

MEASURE 'DAX'[Mediana] = MEDIAN(FactSales[SalesAmount])

MEASURE 'DAX'[Mediana Estatica] = CALCULATE(MEDIAN(FactSales[SalesAmount]), ALL())

MEASURE 'DAX'[Recuento] = COUNT(FactSales[SalesAmount])

MEASURE 'DAX'[Recuento NUlos] = COUNTBLANK(DimProduct[ProductSubcategoryKey])

MEASURE 'DAX'[Recuento Productos] = COUNT(DimProduct[SafetyStockLevel])

MEASURE 'DAX'[Recuento sin nulos] = DISTINCTCOUNTNOBLANK(DimProduct[ProductSubcategoryKey])

MEASURE 'DAX'[Recuento Subcategoria] = COUNT(DimProduct[ProductSubcategoryKey])

MEASURE 'DAX'[REcuento Unico] = DISTINCTCOUNT(DimProduct[SafetyStockLevel])

MEASURE 'DAX'[Recuento Unicos Subcategoria] = DISTINCTCOUNT(DimProduct[ProductSubcategoryKey])

MEASURE 'DAX'[Selector DAX] =
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

MEASURE 'DAX'[Selector DAX Color] =
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

MEASURE 'DAX'[Tarjeta Evolucion] = 
IF([Ventas Totales 2012] = BLANK(), "No hay Datos", "La variacion de año a año ha sido de: " & FORMAT([Evolucion], "0.00%"))

MEASURE 'DAX'[Today] = YEAR(NOW())

MEASURE 'DAX'[Ventales Total CY] = CALCULATE(SUM(FactSales[SalesAmount]), DimDates[CalendarYear] = YEAR(NOW()))

MEASURE 'DAX'[Ventales Total PY] = CALCULATE(SUM(FactSales[SalesAmount]), DimDates[CalendarYear] = YEAR(NOW()) - 1)

MEASURE 'DAX'[Ventales Totales Europa y Pacific] = CALCULATE(SUM(FactSales[SalesAmount]), DimTerritory[SalesTerritoryGroup] IN {"Europe", "Pacific"})

MEASURE 'DAX'[Ventas Totales] = SUM(FactSales[SalesAmount])

MEASURE 'DAX'[Ventas Totales 2011] = CALCULATE(SUM(FactSales[SalesAmount]), DimDates[CalendarYear] = 2011)

MEASURE 'DAX'[Ventas Totales 2012] = CALCULATE(SUM(FactSales[SalesAmount]), DimDates[CalendarYear] = 2012)

MEASURE 'DAX'[Ventas Totales 2012 (Ship)] = CALCULATE(SUM(FactSales[SalesAmount]), DimDates[CalendarYear] = 2012, USERELATIONSHIP(DimDates[DateKey], FactSales[ShipDateKey]))

MEASURE 'DAX'[Ventas Totales en USD] = SUM(FactSales[SalesAmount USD])

MEASURE 'DAX'[ventas totales mes anterior] =
VAR PM = MAX(DimDates[PERIOD]) - 1
RETURN CALCULATE(SUM(FactSales[SalesAmount]), FILTER(ALL(DimDates[PERIOD]), DimDates[PERIOD] = PM))

MEASURE 'DAX'[Ventas Totales sin Europa ni Pacifico] = CALCULATE(SUM(FactSales[SalesAmount]), NOT DimTerritory[SalesTerritoryGroup] IN {"Europe", "Pacific"})

MEASURE 'DAX'[Ventas Totales SIN UK] = CALCULATE([Ventas Totales], DimTerritory[SalesTerritoryCountry] <> "United Kingdom")

MEASURE 'DAX'[Ventas Totales UK] = CALCULATE(SUM(FactSales[SalesAmount]), DimTerritory[SalesTerritoryCountry] = "United Kingdom")

MEASURE 'DAX'[Ventas Totales UK 2012] = CALCULATE(SUM(FactSales[SalesAmount]), DimTerritory[SalesTerritoryCountry] = "United Kingdom", DimDates[DateKey] >= 20120101, DimDates[DateKey] <= 20121231)

MEASURE 'DAX'[YTD] = TOTALYTD(SUM(FactSales[SalesAmount]), DimDates[FullDateAlternateKey])

MEASURE 'DAX'[YTD CAL] =
VAR PM = MAX(DimDates[PERIOD])
RETURN CALCULATE(SUM(FactSales[SalesAmount]), FILTER(ALL(DimDates[PERIOD]), DimDates[PERIOD] <= PM))

MEASURE 'DAX'[YTD CAL anterior] =
VAR PM = MAX(DimDates[PERIOD]) - 1
RETURN CALCULATE(SUM(FactSales[SalesAmount]), FILTER(ALL(DimDates[PERIOD]), DimDates[PERIOD] <= PM))
