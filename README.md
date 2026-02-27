# 📞 Identificación de Operadores Ineficaces — Telecom Analysis

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://python.org)
[![Pandas](https://img.shields.io/badge/Pandas-2.0-green.svg)](https://pandas.pydata.org)
[![Seaborn](https://img.shields.io/badge/Seaborn-0.12+-orange.svg)](https://seaborn.pydata.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## 🔍 Problema de Negocio

Una empresa de telecomunicaciones necesitaba identificar qué operadores de su call center tenían bajo rendimiento para priorizar capacitación y mejorar la experiencia del cliente.

---

## 📊 Dataset

| Dataset | Registros | Descripción |
|---------|-----------|-------------|
| `telecom_dataset_new.csv` | 53,902 | Registro de llamadas (dirección, duración, estado) |
| `telecom_clients.csv` | — | Información de clientes |

**Variables clave:** `operator_id`, `is_missed_call`, `call_duration`, `total_call_duration`, `direction` (in/out), `internal`

> 📁 Los datasets se encuentran en la carpeta `datasets/`

---

## 🛠 Stack Tecnológico

| Herramienta | Uso |
|-------------|-----|
| Python 3.8+ | Análisis principal |
| Pandas | Manipulación y limpieza de datos |
| NumPy | Cálculos estadísticos |
| Matplotlib / Seaborn | Visualizaciones |
| SciPy (Mann-Whitney U) | Pruebas de hipótesis |
| Tableau Public | Dashboards interactivos |

---

## 🎯 Metodología

### Preprocesamiento
- Conversión de fechas y eliminación de duplicados
- Eliminación de registros sin `operator_id` (~15.2% nulos)
- Separación de llamadas **entrantes** (in) y **salientes** (out)
- Cálculo de `waiting_time = total_call_duration − call_duration`

### Criterios de Ineficiencia (basados en percentiles)

| # | Criterio | Umbral | Descripción |
|---|----------|--------|-------------|
| 1 | Tasa de llamadas perdidas | > Percentil 75 | Más pérdidas que el 75% de colegas |
| 2 | Tiempo de espera promedio | > 43 segundos (P75) | Tiempos excesivos que generan abandono |
| 3 | Llamadas salientes | < Percentil 25 | Baja proactividad en contacto con clientes |

### Sistema de Puntuación (Score 0–3)

- **Score 0** → Operador Eficaz ✅
- **Score 1–2** → Requiere Atención ⚠️
- **Score 3** → Operador Crítico 🔴 (cumple los 3 criterios)

---

## ✅ Resultados

### Clasificación de Operadores

| Categoría | Operadores | % del Total |
|-----------|-----------|-------------|
| ✅ Eficaces | 897 | 81.5% |
| ⚠️ Ineficaces (Score 1–2) | 159 | 14.4% |
| 🔴 Críticos (Score 3) | 43 | **3.9%** |
| **Total ineficaces** | **202** | **18.5%** |

### Hallazgos Clave

- **Distribución de llamadas:** 97.5% externas (client-facing), 42.6% salientes
- **Duración de llamadas:** Media ~257s (~4 min) · Mediana ~169s — distribución sesgada a la derecha con cola larga
- **Correlación positiva confirmada:** mayor tiempo de espera → más llamadas perdidas

### 📦 Observaciones del Boxplot — Tiempo de Espera por Grupo

| | Eficaces 🟢 | Ineficaces 🔴 |
|---|---|---|
| **Mediana** | ~10–20 s | ~60–80 s (≈ 4× mayor) |
| **IQR (50% central)** | Muy compacto (~0–40 s) | Amplio (~0–160 s) |
| **Bigote superior** | ~40 s | ~160 s |
| **Outliers** | Mínimos (~1) | Numerosos y extremos (>400 s, hasta ~1000 s) |
| **Variabilidad** | Baja — grupo homogéneo | Alta — grupo heterogéneo |

**Conclusiones visuales:**
- Los operadores ineficaces tienen una mediana de tiempo de espera **≈ 4× mayor** que los eficaces.
- La caja de ineficaces es notablemente más ancha, indicando **alta dispersión interna** dentro del grupo.
- Se observan **outliers extremos** (>400 s, con máximos cerca de 1000 s) exclusivamente en el grupo ineficaz.
- El grupo eficaz presenta una distribución muy concentrada cerca de 0, lo que indica consistencia en la atención rápida.

### Pruebas de Hipótesis (Mann-Whitney U)

| Prueba | Métrica | Resultado | Significancia |
|--------|---------|-----------|---------------|
| Prueba 1 | Tiempo de espera | Rechazamos H0 | p < 0.05 ✅ |
| Prueba 2 | Tasa de llamadas perdidas | Rechazamos H0 | p < 0.05 ✅ |

> Las diferencias entre grupos NO son producto del azar — son estadísticamente significativas.

---

## 💡 Recomendaciones

### 🔴 Inmediato (0–30 días)
- **Capacitación intensiva** para los 202 operadores ineficaces
- **Auditoría técnica** de los 43 operadores críticos (Score 3)
- **Programa de mentorship** con top performers
- Inversión estimada: **$404K** | ROI esperado: **$750K ahorro/año**

### 🟡 Mediano Plazo (30–90 días)
- Sistema de incentivos por mantener tasa de pérdidas < 5%
- Dashboard en tiempo real para supervisores

### 🔵 Largo Plazo (90+ días)
- Mejora del proceso de selección de operadores
- Automatización IVR para reducir carga operativa
- Re-evaluación mensual de métricas

### 💰 Impacto Económico

| Indicador | Valor |
|-----------|-------|
| Costo actual de ineficiencia | $1.5M USD/año |
| Ahorro proyectado | $750K USD/año |
| Break-even | 6–8 meses |

---

## 📈 Dashboards y Presentación

- 📄 [Presentación PDF completa](https://drive.google.com/file/d/1nOZfl1tiLiW-CCoa8Gz1ekqo9DYc8E9e/view?usp=sharing)
- 📊 [Dashboard Tableau — Análisis de Duración de Llamadas](https://public.tableau.com/views/AnlisisdeDuracindeLlamadasporDireccin/Dashboard1AnlisisdeDuracin?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
- 📊 [Dashboard Tableau — Volumen Diario de Llamadas](https://public.tableau.com/views/AnlisisdeVolumendeLlamadasporDa/Dashboard2AnlisisdeVolumenDiario?:language=es-ES&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

---

## 🚀 Cómo ejecutar

```bash
# 1. Clonar el repositorio
git clone https://github.com/Baltazardv/telecom-operator-analysis.git
cd telecom-operator-analysis

# 2. Instalar dependencias
pip install pandas numpy matplotlib seaborn scipy

# 3. Ejecutar el notebook
jupyter notebook Proyecto_Final_Operadores_Ineficaces.ipynb
```

> La primera celda del notebook verifica e instala las dependencias automáticamente.

---

## 📁 Estructura del Proyecto

```
telecom-operator-analysis/
│
├── Proyecto_Final_Operadores_Ineficaces.ipynb  # Análisis completo (33 celdas)
├── README.md
├── operadores_ineficaces.csv                   # Resultado exportado
└── datasets/
    ├── telecom_dataset_new.csv                  # Dataset principal de llamadas
    └── telecom_clients.csv                      # Dataset de clientes
```

---

## 👤 Autor

**Baltazar Dimayuga Vega**  
Data Analyst Jr | Ingeniería en Computación — UAGro  
[LinkedIn](https://linkedin.com/in/baltazar-dimayuga) · [GitHub](https://github.com/Baltazardv) · [Portfolio](https://baltazardv.github.io)
