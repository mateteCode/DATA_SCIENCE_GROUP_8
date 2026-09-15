# **Proyecto Final de Ciencia de Datos: Seguridad Vial en CABA**
### **Programa EnergIA Digital — Fundación YPF | Docente: Gabriel Acosta | Tutora: Belén Laresca**

---

## **Origen del Dataset**
Los datos fueron obtenidos del portal oficial de datos abiertos del Gobierno de la Ciudad Autónoma de Buenos Aires (**BA Data**), provistos por el *Observatorio de Movilidad y Seguridad Vial (OMSV)* perteneciente a la Secretaría de Transporte y el Ministerio de Infraestructura y Movilidad de la Ciudad. El conjunto está integrado por dos recursos interrelacionados: `siniestros_viales_hechos.csv` (65.818 hechos con variables temporales, geográficas y dinámicas de colisión) y `siniestros_viales_victimas.csv` (75.197 víctimas con atributos demográficos y desenlace médico auditado en hospitales de agudos y seguimiento a 30 días).

---

## **Resumen del Proyecto**
Este proyecto aborda la problemática de la seguridad vial urbana en la Ciudad de Buenos Aires entre los años 2019 y 2025 desde un enfoque integral de Ciencia de Datos. Mediante técnicas de limpieza, ingeniería de características espaciotemporales, integración relacional y visualizaciones con storytelling, se analizan los patrones de siniestralidad para caracterizar los perfiles de mayor riesgo y sentar las bases de modelos predictivos y de segmentación no supervisada que sirvan de insumo para la toma de decisiones en políticas públicas de tránsito y respuesta de emergencias médicas (SAME).

---

## **Objetivo Propuesto**
El objetivo central consiste en responder: *¿Qué factores temporales, territoriales y de interacción vehicular explican y anticipan la gravedad de los siniestros viales en CABA, y cómo podemos identificar agrupamientos naturales (clusters) de accidentes para intervenir preventivamente?* Para ello, el proyecto articula un Análisis Exploratorio de Datos (Pre-entrega 2), un modelo de clasificación multiclase para predecir la gravedad del hecho sin incurrir en *data leakage* (Pre-entrega 3) y un modelo de clustering no supervisado para segmentar tipologías de siniestralidad (Pre-entrega 4).

---

## **Hallazgos Principales del Análisis Exploratorio (AED)**
1. **Dinámica Temporal y Resiliencia:** Tras una caída del 40% en 2020 por las restricciones de la pandemia COVID-19, la siniestralidad creció sostenidamente hasta superar los 11.700 hechos anuales en 2025, aunque la mortalidad se mantuvo estable en ~100-110 muertes/año.
2. **Patrón Horario Bimodal y Nocturno:** Los días hábiles concentran picos en horas laborales (08:00–10:00 y 17:00–19:00 hs), mientras que los fines de semana la criticidad se traslada a la madrugada (03:00–07:00 hs).
3. **Contraste Territorial y Vías Críticas:** La Comuna 1 concentra el mayor volumen absoluto de colisiones, pero las Comunas del Sur (4, 8 y 9) sufren la mayor tasa de siniestros graves y mortales; asimismo, las Avenidas concentran el 57.4% de los choques, pero las vías rápidas (Autopistas y Av. General Paz) triplican la tasa de mortalidad por cada 1.000 siniestros.
4. **Vulnerabilidad Extrema de Usuarios Ligeros:** La colisión más frecuente es Moto vs. Auto (>13.000 casos); los peatones (con pico en adultos mayores de 60 años) presentan la máxima letalidad individual (2.38%), seguidos por los motociclistas (1.12%).
5. **Desbalance de Gravedad:** La variable objetivo `gravedad_siniestro` presenta 94.46% de casos Leves, 4.49% Graves y 1.05% Mortales, lo que fundamenta el uso de métricas como Macro F1-score y ponderación de costos.

---

## 🤖 **Detalle de los Modelos (Hoja de Ruta Pre-Entregas 3 y 4)**
- **Modelo Supervisado (Pre-Entrega 3 - Clasificación de Gravedad):** Se ajustará un modelo de clasificación (`gravedad_siniestro`) evaluando `DummyClassifier` (baseline), `LogisticRegression`, `DecisionTree` y `RandomForest` / `HistGradientBoosting`. Se adopta una decisión metodológica estricta para prevenir **Data Leakage**, excluyendo variables que se conocen post-accidente (`numero_victimas_mortal_siniestro`, `fecha_fallecimiento_victima`), y se aplicará `class_weight='balanced'` y evaluación mediante Macro F1-score y PR-AUC.
- **Modelo No Supervisado (Pre-Entrega 4 - Clustering de Siniestros):** Se aplicará `K-Means` y `DBSCAN` sobre características ex-ante (horario, día, tipo de arteria, comuna y actores vehiculares involucrados) utilizando el Método del Codo y el Coeficiente de Silueta para descubrir perfiles de riesgo y evaluar a posteriori la severidad de cada cluster.

---

## 📂 **Estructura del Repositorio**
```text
TP-grupal/
│
├── siniestros_viales_hechos.csv                    # Dataset oficial de Hechos (OMSV)
├── siniestros_viales_victimas.csv                  # Dataset oficial de Víctimas (OMSV)
├── Entrega_2_Analisis_Exploratorio_Siniestros_Viales_CABA.ipynb  # Notebook completo de la 2ª Pre-entrega
└── README.md                                       # Resumen del proyecto y entregables
```
