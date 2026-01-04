# PowerBI_AutoShop_Operations_Dashboard — dax_measures (TXT)
# Separador de argumentos DAX: coma (,)
# Nota: si tus tablas/columnas tienen otros nombres, cambia SOLO los nombres, no la lógica.

////////////////////////////////////////////////////////////
// 1) INGRESOS / MARGEN (Visión ejecutiva)
////////////////////////////////////////////////////////////

Ingresos Mano de Obra (Brutos) =
CALCULATE(
    SUMX(
        ServiceLinesTbl,
        ServiceLinesTbl[labor_hours] * ServiceLinesTbl[labor_rate_charged]
    ),
    WorkOrdersTbl[status] = "Completed"
)

Ingresos Mano de Obra (Netos) =
CALCULATE(
    SUMX(
        ServiceLinesTbl,
        ServiceLinesTbl[labor_hours] * ServiceLinesTbl[labor_rate_charged]
            - ServiceLinesTbl[labor_discount]
    ),
    WorkOrdersTbl[status] = "Completed"
)

Horas Facturadas (h) =
CALCULATE(
    SUM(ServiceLinesTbl[labor_hours]),
    WorkOrdersTbl[status] = "Completed"
)

Tarifa efectiva labor (AUD/h) =
DIVIDE(
    [Ingresos Mano de Obra (Netos)],
    [Horas Facturadas (h)]
)

% Descuento Mano de Obra =
DIVIDE(
    CALCULATE(
        SUM(ServiceLinesTbl[labor_discount]),
        WorkOrdersTbl[status] = "Completed"
    ),
    [Ingresos Mano de Obra (Brutos)]
)

Ingresos Repuestos (Netos) =
CALCULATE(
    SUMX(
        PartLinesTbl,
        PartLinesTbl[qty] * PartLinesTbl[sale_price]
            - PartLinesTbl[part_discount]
    ),
    WorkOrdersTbl[status] = "Completed"
)

Costo Repuestos (AUD) =
CALCULATE(
    SUMX(
        PartLinesTbl,
        PartLinesTbl[qty] * PartLinesTbl[unit_cost]
    ),
    WorkOrdersTbl[status] = "Completed"
)

KPI Margen Repuestos % =
DIVIDE(
    [Ingresos Repuestos (Netos)] - [Costo Repuestos (AUD)],
    [Ingresos Repuestos (Netos)]
)

Ingresos netos (AUD) =
[Ingresos Mano de Obra (Netos)] + [Ingresos Repuestos (Netos)]

Margen bruto (%) =
DIVIDE(
    [Ingresos netos (AUD)] - [Costo Repuestos (AUD)],
    [Ingresos netos (AUD)]
)

////////////////////////////////////////////////////////////
// 2) DIAGNÓSTICO & FLUJO (esperas)
////////////////////////////////////////////////////////////

Órdenes completadas =
CALCULATE(
    DISTINCTCOUNT(WorkOrdersTbl[work_order_id]),
    WorkOrdersTbl[status] = "Completed"
)

Diagnósticos completados =
CALCULATE(
    DISTINCTCOUNT(WorkOrdersTbl[work_order_id]),
    WorkOrdersTbl[status] = "Completed",
    WorkOrdersTbl[service_type] = "Diagnostic"
)

Horas de espera en diagnóstico (h) (WO-safe) =
CALCULATE(
    SUMX(
        VALUES(WorkOrdersTbl[work_order_id]),
        MAX(WorkOrdersTbl[diagnostic_wait_hours])
    ),
    WorkOrdersTbl[status] = "Completed",
    WorkOrdersTbl[service_type] = "Diagnostic"
)

Espera promedio por diagnóstico (h) =
DIVIDE(
    [Horas de espera en diagnóstico (h) (WO-safe)],
    [Diagnósticos completados]
)

Diagnósticos con espera >36h (conteo) =
CALCULATE(
    [Diagnósticos completados],
    WorkOrdersTbl[diagnostic_wait_hours] > 36
)

Diagnósticos con espera >36h (%) =
DIVIDE(
    [Diagnósticos con espera >36h (conteo)],
    [Diagnósticos completados]
)

Diagnósticos “sin falla” (conteo) =
CALCULATE(
    [Diagnósticos completados],
    WorkOrdersTbl[diagnosis_result] = "Intermittent / No Fault Found"
)

Tasa “sin falla” en diagnóstico (%) =
DIVIDE(
    [Diagnósticos “sin falla” (conteo)],
    [Diagnósticos completados]
)

Costo de oportunidad por espera (Diagnóstico) (AUD) =
[Horas de espera en diagnóstico (h) (WO-safe)] * [Tarifa efectiva labor (AUD/h)]

////////////////////////////////////////////////////////////
// 3) UTILIZACIÓN (capacidad vs monetización)
////////////////////////////////////////////////////////////

Horas disponibles netas (h) =
SUM(TechnicianShiftsTbl[available_hours])
- SUM(TechnicianShiftsTbl[absent_hours])
+ SUM(TechnicianShiftsTbl[overtime_hours])

Horas no facturadas (h) =
[Horas disponibles netas (h)] - [Horas Facturadas (h)]

Utilización de técnicos (%) =
DIVIDE(
    [Horas Facturadas (h)],
    [Horas disponibles netas (h)]
)

////////////////////////////////////////////////////////////
// 4) HOLD (si decides mantenerlo)
////////////////////////////////////////////////////////////

Horas en HOLD (h) (WO-safe) =
SUMX(
    VALUES(WorkOrdersTbl[work_order_id]),
    MAX(WorkOrdersTbl[hold_hours])
)

Órdenes en HOLD (conteo) =
CALCULATE(
    DISTINCTCOUNT(WorkOrdersTbl[work_order_id]),
    WorkOrdersTbl[hold_reason] <> BLANK()
)

Horas HOLD por Diagnóstico (h) =
DIVIDE(
    [Horas en HOLD (h) (WO-safe)],
    [Diagnósticos completados]
)

////////////////////////////////////////////////////////////
// 5) INVENTARIO (reorder, capital inmovilizado)
////////////////////////////////////////////////////////////

SKUs stocked (activos) =
CALCULATE(
    DISTINCTCOUNT(PartsTbl[part_id]),
    PartsTbl[on_hand_qty] > 0
)

KPI Capital inmovilizado (AUD) =
SUMX(
    PartsTbl,
    PartsTbl[on_hand_qty] * PartsTbl[unit_cost]
)

SKUs en Riesgo (Reorder) =
CALCULATE(
    DISTINCTCOUNT(PartsTbl[part_id]),
    FILTER(
        PartsTbl,
        PartsTbl[on_hand_qty] < PartsTbl[reorder_point_qty]
    )
)

Riesgo $ (Reorder) =
SUMX(
    FILTER(
        PartsTbl,
        PartsTbl[on_hand_qty] < PartsTbl[reorder_point_qty]
    ),
    (PartsTbl[reorder_point_qty] - PartsTbl[on_hand_qty]) * PartsTbl[unit_cost]
)

////////////////////////////////////////////////////////////
// 6) PROVEEDORES (scatter: lead time vs costo, tamaño = qty)
////////////////////////////////////////////////////////////

Purchases Qty =
SUM(PurchaseLinesTbl[purchase_qty])

Purchases Cost $ =
SUMX(
    PurchaseLinesTbl,
    PurchaseLinesTbl[purchase_qty] * PurchaseLinesTbl[unit_cost]
)

Costo Promedio por Unidad =
DIVIDE(
    [Purchases Cost $],
    [Purchases Qty]
)

Lead Time Promedio (días) =
AVERAGE(PurchaseLinesTbl[lead_time_days])

////////////////////////////////////////////////////////////
// 7) TOOLTIP (si lo quieres “bonito” en un solo visual)
////////////////////////////////////////////////////////////

Tooltip — Horas Facturadas (h) =
[Horas Facturadas (h)]

Tooltip — Horas disponibles netas (h) =
[Horas disponibles netas (h)]

Tooltip — Horas no facturadas (h) =
[Horas no facturadas (h)]

Tooltip — Utilización (%) =
[Utilización de técnicos (%)]

Tooltip — HOLD total (h) =
[Horas en HOLD (h) (WO-safe)]
