\# Diccionario de Datos — Demo 2 (Taller Mecánico) | 2025



Este documento describe las tablas del modelo y sus campos principales.

El modelo está diseñado como un esquema operativo (Work Orders) + soporte (clientes, técnicos, repuestos, compras, calendario).



---



\## 1) DateTableTbl (calendario)

\- Date (fecha)

\- Year, MonthNumber, MonthName, YearMonth (atributos tiempo)



Uso: segmentación por mes/año en todas las páginas.



---



\## 2) WorkOrdersTbl (órdenes de trabajo)

Campos clave:

\- work\_order\_id (ID único)

\- open\_datetime / close\_datetime / close\_date

\- status (Completed, etc.)

\- technician\_id, advisor\_id, customer\_id, vehicle\_id

\- is\_diagnostic (bandera diagnóstico)

\- hold\_reason (motivo principal de espera)

\- waiting\_hours (horas de espera)

\- active\_labor\_hours (horas activas de taller)

\- cycle\_hours (tiempo total del WO)

\- diagnosis\_outcome (resultado diagnóstico: No Fault Found, Resolved, etc.)



Uso:

\- Diagnóstico \& flujo

\- HOLD por motivo

\- Tablas de órdenes con mayor espera

\- Cálculos de productividad/tiempo



---



\## 3) ServiceLinesTbl (líneas de servicio)

Campos:

\- work\_order\_id (FK)

\- service\_id (FK)

\- labor\_hours

\- labor\_rate\_charged

\- labor\_discount

\- technician\_id (quién ejecuta)



Uso:

\- Ingresos de mano de obra (netos)

\- Tarifa efectiva labor

\- Análisis por servicio



---



\## 4) PartLinesTbl (consumo de repuestos en órdenes)

Campos:

\- work\_order\_id (FK)

\- part\_id (FK)

\- quantity\_used

\- unit\_cost

\- part\_name, part\_category



Uso:

\- Used Qty (consumo)

\- Costo repuestos usados

\- Rotación / demanda



---



\## 5) PurchasesTbl / PurchaseLinesTbl (compras)

Campos:

\- supplier\_id, part\_id

\- purchase\_date

\- quantity\_purchased

\- unit\_cost



Uso:

\- Purchases Qty

\- Costo compras

\- Proveedores: lead time vs costo vs volumen



---



\## 6) PartsTbl (catálogo de repuestos)

Campos:

\- part\_id

\- part\_name, part\_category

\- reorder\_point\_qty

\- unit\_cost\_std (si existe)

\- supplier\_id (principal)



Uso:

\- Reorder point

\- Clasificación por categoría

\- Capital inmovilizado / riesgo



---



\## 7) SuppliersTbl (proveedores)

Campos:

\- supplier\_id

\- supplier\_name

\- lead\_time\_days (si aplica)



Uso:

\- Dispersión: lead time vs costo (tamaño=qty)

\- Segmentación por proveedor



---



\## 8) TechnicianShiftsTbl (capacidad / disponibilidad)

Campos:

\- shift\_date (FK calendario)

\- technician\_id

\- available\_hours

\- absent\_hours

\- overtime\_hours



Uso:

\- Horas disponibles netas

\- Capacidad mensual vs monetización



---



\## 9) TechniciansTbl

\- technician\_id

\- technician\_name



Uso:

\- Utilización por técnico

\- Slicers



---



\## 10) CustomersTbl / VehiclesTbl (contexto)

Clientes:

\- customer\_id, customer\_name



Vehículos:

\- vehicle\_id, plate, model, etc.



Uso:

\- Drilldown por orden

\- Tablas de detalle



---



\## 11) TargetsTbl (metas / benchmarks)

\- SLA targets (si existen)

\- objetivos por mes



Uso:

\- Comparación con meta (opcional)



