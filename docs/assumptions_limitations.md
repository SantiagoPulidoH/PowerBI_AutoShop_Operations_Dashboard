
---

# ✅ `docs/assumptions_limitations.md` (COPIAR / PEGAR)

```md
# Supuestos y Limitaciones — PowerBI_AutoShop_Operations_Dashboard (DEMO 2)

Este proyecto es un **demo de portafolio** basado en un dataset sintético (no datos reales).
El objetivo es demostrar análisis, modelado, storytelling y capacidad ejecutiva de BI.

---

## 1) Alcance temporal
- El demo se interpreta principalmente bajo el filtro **Año = 2025**.
- La tabla de fechas (`Date table`) controla el contexto de tiempo.
- Las métricas de órdenes y operación se basan en `WorkOrders[close_date]`.

---

## 2) Definiciones operativas (importantes)
Para evitar confusiones, estas son las definiciones usadas en el reporte:

### “Horas facturadas”
- Se usan **WorkOrders[active_labor_hours]** en órdenes completadas.
- No se usa la suma de `ServiceLines[labor_hours]` como total anual porque puede duplicar si existen múltiples líneas por orden.

### “Horas disponibles netas”
- Definición:  
  `available_hours - absent_hours + overtime_hours`
- Fuente: `TechnicianShifts`

### “Horas en HOLD / espera”
- Se usa `WorkOrders[waiting_hours]`.
- Se analiza por `hold_reason` como driver del tiempo perdido.

### “Diagnóstico”
- Se define por `WorkOrders[is_diagnostic] = TRUE`.
- El análisis de diagnósticos se limita a órdenes completadas con ciclo > 0.

---

## 3) Supuestos de costos e impacto financiero
El reporte incluye métricas “ejecutivas” que son **estimaciones**:

### Costo de oportunidad por espera
- Fórmula:  
  `Horas de espera diagnóstico * Tarifa efectiva labor`
- Importante: **no representa gasto contable**, es una estimación del valor de capacidad perdida.

### Costo de mano de obra estimado
- Se aproxima con:
  `ServiceLines[labor_hours] * Technicians[hourly_cost_rate]`
- No incluye cargas sociales, overhead, administración ni costos indirectos.

---

## 4) Inventario (Stock teórico)
Dado que el dataset no contiene un snapshot real de inventario por fecha, se calcula un **stock teórico**:

- Stock teórico (AsOf) = compras acumuladas − consumo acumulado  
- Compras: `PurchaseLines[qty_purchased]`
- Consumo: `PartLines[quantity_used]`

Limitación:
- No incluye inventario inicial real, ajustes manuales, mermas, devoluciones, pérdidas o robos.
- Por lo tanto, el stock es **aproximado**, útil para análisis y priorización, no para contabilidad exacta.

---

## 5) Limitaciones del dataset y del modelo
- Dataset **sintético** (para demo), no refleja todos los casos reales.
- No existen:
  - Tiempos de entrega reales por evento (solo agregados por proveedor).
  - Costos reales completos de operación (arriendo, herramientas, administración).
  - Satisfacción del cliente / NPS.
- El modelo asume consistencia de registros y fechas; registros faltantes pueden sesgar promedios.

---

## 6) Interpretación recomendada (cómo usar el dashboard)
El dashboard está diseñado para responder a 3 preguntas ejecutivas:

1) **Dónde se pierde tiempo y por qué (diagnóstico / hold).**
2) **Cómo se traduce eso en utilización y monetización.**
3) **Qué riesgo existe en inventario y proveedores (stock, lead time, capital).**

La última página entrega:
- Hallazgos clave
- Impacto financiero estimado
- Plan prioritario 90 días

---

## 7) Nota de privacidad
- El proyecto NO usa datos reales.
- No se publica “Publish to web” para evitar exposición innecesaria.
- Se distribuye únicamente como repositorio técnico para portafolio.

---
