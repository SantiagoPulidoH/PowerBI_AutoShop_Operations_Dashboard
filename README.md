# 🚗 Power BI — AutoShop Operations Dashboard (DEMO 2) | 2025
**Diagnóstico • Utilización • Tiempo muerto (HOLD) • Inventario & Proveedores**  
> Caso tipo: taller mecánico pequeño (6 bahías / 6 técnicos). Dataset sintético para portafolio.

---

## 🔥 Por qué este dashboard existe (la historia en 10 segundos)
Un taller no se quiebra por “falta de clientes”. Se quiebra por **fugas invisibles**:
1) **Diagnósticos que se quedan esperando** (carros ocupando bahía + tiempo muerto).  
2) **Capacidad disponible que no se monetiza** (horas pagadas vs horas facturadas).  
3) **Inventario mal gestionado** (capital inmovilizado + quiebres de stock por reorder).

Este demo muestra cómo detectar esas fugas, cuantificarlas y convertirlas en un **plan 90 días**.

---

## ✅ Lo que puedes ver aquí (entregables)
- **Dashboard Power BI (.pbix)** con 4 páginas operativas + 1 ejecutiva:
  1) Resumen Ejecutivo (KPIs 2025)
  2) Fuga #1 — Diagnóstico & Espera
  3) Fuga #2 — Utilización & Tiempo perdido (HOLD)
  4) Fuga #3 — Inventario & Proveedores
  5) Hallazgos clave + Impacto estimado + Plan de acción 90 días
- **Dataset sintético (Excel)** para reproducir el modelo.
- **Documentación técnica**: diccionario, DAX, supuestos y pasos.
- **Reporte ejecutivo (PDF)** listo para entrega / portafolio.

---

## 📸 Preview del dashboard
> Coloca aquí tus capturas en /assets (importante para que el repo “venda” solo)

- **Página 1 — Resumen Ejecutivo**  
  `assets/page_01_resumen.png`

- **Página 2 — Diagnóstico & Espera**  
  `assets/page_02_diagnostico_flujo.png`

- **Página 3 — Utilización & Tiempo perdido**  
  `assets/page_03_utilizacion_hold.png`

- **Página 4 — Inventario & Proveedores**  
  `assets/page_04_inventario.png`

- **Página 5 — Hallazgos + Plan 90 días**  
  `assets/page_05_hallazgos_plan_90_dias.png`

> Consejo: sube también un `assets/cover.png` (una captura “hero”) y colócala arriba del todo.

---

## 🧠 Hallazgos que este demo fuerza a ver (y que duelen)
### Fuga #1 — Diagnóstico
- La espera en diagnóstico revela **cuellos de botella** (aprobaciones, agenda, prioridad, repuestos).
- KPI clave: **% diagnósticos lentos (>36h)** y **horas totales de espera**.
- Impacto típico: menos throughput (menos carros terminados) y bahías bloqueadas.

### Fuga #2 — Utilización
- El negocio puede tener trabajo, pero si la **capacidad no se factura**, la rentabilidad se cae.
- KPI clave: **utilización (%)**, **horas no facturadas**, **tarifa efectiva (AUD/h)**.
- Traducción ejecutiva: “estamos pagando capacidad que no se convierte en ingresos”.

### Fuga #3 — Inventario
- Dos riesgos al mismo tiempo:
  - **capital inmovilizado** (stock que no rota)
  - **quiebre por reorder** (stock bajo punto de reorden)
- KPI clave: **capital inmovilizado (AUD)**, **SKUs bajo reorder**, **riesgo $ reorder**.
- Proveedores: lead time vs costo (cuadrantes para priorizar.

---

## 🎯 Qué pregunta de negocio responde
**Si yo fuera dueño del taller, este dashboard me responde:**
1) ¿Qué está causando que los carros se queden días ocupando espacio?  
2) ¿Estoy monetizando mi capacidad o solo estoy “ocupado”?  
3) ¿Qué repuestos me están bloqueando la operación o drenando caja?  
4) ¿Qué acciones hago esta semana para recuperar control?

---

## ▶️ Cómo correrlo (reproducible)
1. Descarga/clona el repo.
2. Abre el PBIX:  
   `pbix/Demo 2.pbix`  
3. Si Power BI pide ruta del dataset:
   - **Transformar datos → Configuración de origen de datos**
   - Apunta a: `data_sample/MetroFix_AutoCare_DEMO_dataset.xlsx`
4. Refresh.

---

## 🗂️ Estructura del repositorio

pbix/ -> archivo Power BI
data_sample/ -> dataset sintético (Excel)
assets/ -> capturas del dashboard (PNG)
docs/ -> diccionario, DAX, supuestos, build steps
reports/ -> reporte ejecutivo (PDF)

---

## 🔒 Privacidad / Nota importante
Dataset **100% sintético** (demo). No contiene información real de clientes, vehículos ni proveedores.

---

## 👤 Autor
Santiago Pulido Hurtado
📍 Melbourne, Australia  
- LinkedIn: www.linkedin.com/in/santiagopulidohurtado97
- Email: santiagopulidorhuartado97@gmail.com

---

## 📎 Documentación
- Diccionario de datos: `docs/data_dictionary.md`
- Catálogo de medidas DAX: `docs/dax_measures.md`
- Supuestos y limitaciones: `docs/assumptions_limitations.md`
- Pasos de construcción: `docs/build_steps.md`
- Reporte ejecutivo: `reports/Reporte-Ejecutivo-Operativo-2025.pdf`
