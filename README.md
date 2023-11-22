# DAX Guide - For Power Bi Reports
## Guide for common functions and their use

## Overview
[DAX Guide](https://dax.guide/)


# Creating a Measure
We need to create a measure that counts the total records that match with the required filters with logical connectors AND (&&) OR (||)
```dax
DEFINE
    MEASURE Tbl_Cruces[Mayores&Centro] =
        CALCULATE (
            COUNTROWS ( 'Tbl_Cruces' ),
            FILTER (
                ALL ( 'Tbl_Cruces' ),
                'Tbl_Cruces'[Tipo_Cruce] = "Mayor"
                    && 'Tbl_Cruces'[Región] = "Centro"
            )
        )
```

# Summarizing a Table
Useful inorder to display only the required colums from a table.

```dax
EVALUATE
CALCULATETABLE (
    SUMMARIZE (
        Tbl_Cruces,
        Tbl_Cruces[Id],
        Tbl_Cruces[Tipo_Cruce],
        Tbl_Cruces[Sistema],
        Tbl_Cruces[Región],
        Tbl_Cruces[Estatus]
    )
)
```
## Sumarizing a Table Using Filters
```dax
EVALUATE
CALCULATETABLE (
    SUMMARIZE (
        PBIpull,
        PBIpull[Type],
        PBIpull[Year Month Code],
        PBIpull[Year],
        PBIpull[Month],
        PBIpull[Area],
        PBIpull[Sub-Area],
        "Totals", SUM ( PBIpull[Count] )
    ),
    FILTER (
        PBIpull,
        AND ( PBIpull[Type] = "08&04 Reforecast", PBIpull[Year Month Code] <= 202206 )
    ),
    FILTER ( PBIpull, PBIpull[Count] > 0 )
)
```