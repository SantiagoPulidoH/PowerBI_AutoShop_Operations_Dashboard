# Build Steps — Demo 2 (Taller Mecánico) | Cómo reproducir el proyecto

## 1) Archivos requeridos
- `/data_sample/MetroFix_AutoCare_DEMO_dataset.xlsx`
- `/pbix/Demo_2_Taller_Mecanica.pbix` (ajusta el nombre real)

## 2) Importación en Power BI
1. Abrir Power BI Desktop
2. Obtener datos > Excel > seleccionar el archivo del dataset
3. Cargar tablas principales:
   - WorkOrdersTbl
   - ServiceLinesTbl
   - PartLinesTbl
   - PurchasesTbl / PurchaseLinesTbl
   - PartsTbl
   - SuppliersTbl
   - TechnicianShiftsTbl
   - TechniciansTbl
   - DateTableTbl

## 3) Relaciones recomendadas
- DateTableTbl[Date] -> WorkOrdersTbl[open_date] (o close_date según tu modelo)
- WorkOrdersTbl[work_order_id] -> ServiceLinesTbl[work_order_id]
- WorkOrdersTbl[work_order_id] -> PartLinesTbl[work_order_id]
- PartsTbl[part_id] -> PartLinesTbl[part_id]
- PartsTbl[part_id] -> PurchaseLinesTbl[part_id]
- SuppliersTbl[supplier_id] -> PurchaseLinesTbl[supplier_id]
- TechniciansTbl[technician_id] -> TechnicianShiftsTbl[technician_id]
- TechniciansTbl[technician_id] -> WorkOrdersTbl[technician_id] (si aplica)

## 4) Medidas DAX
Crear medidas en carpeta “Medidas”:
- Resumen ejecutivo
- Diagnóstico & flujo
- Utilización & HOLD
- Inventario & proveedores

Ver listado completo en:
`/docs/dax_measures.md`

## 5) Diseño del reporte
- Fondo unificado (imagen)
- Cards KPI con misma tipografía y estilo
- Colores consistentes:
  - color principal (turquesa / verde agua)
  - grises para secundarios
  - cuidado con saturación del fondo

## 6) Exportables
- PDF ejecutivo: `/reports/Reporte-Ejecutivo-Operativo-2025.pdf`
- Capturas del dashboard: `/assets/`

## 7) Si el PBIX no encuentra el Excel
Transformar datos > Configuración de origen de datos > Cambiar origen  
Apuntar al archivo en `/data_sample/`
