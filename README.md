# **Proyecto Final de Ciencia de Datos: Seguridad Vial en CABA**
### **Programa EnergIA Digital — Fundación YPF**
**Docente:** Gabriel Acosta | **Tutora:** Belén Laresca  
**Grupo 8:** Diego Bianqui, Luida Medina, Matías Lorenzo  

---

## 📌 **Origen del Dataset**
Los datos fueron obtenidos del portal oficial de datos abiertos del Gobierno de la Ciudad Autónoma de Buenos Aires (**[BA Data](https://data.buenosaires.gob.ar/)**), provistos por el *Observatorio de Movilidad y Seguridad Vial (OMSV)* perteneciente a la Secretaría de Transporte y el Ministerio de Infraestructura y Movilidad de la Ciudad. 

El conjunto está integrado por dos recursos relacionales:
- `siniestros_viales_hechos.csv`: **65.818 hechos** registrados con variables temporales, geográficas y dinámicas de colisión.
- `siniestros_viales_victimas.csv`: **75.197 víctimas** con atributos demográficos y desenlace médico auditado en los 14 hospitales de agudos y seguimiento a 30 días posteriores al siniestro.

---

## 📖 **Resumen del Proyecto**
Este proyecto aborda la problemática de la seguridad vial urbana en la Ciudad Autónoma de Buenos Aires entre los años 2019 y 2025 desde un enfoque integral de Ciencia de Datos. Mediante técnicas de limpieza, normalización de variables, ingeniería de características espaciotemporales, integración relacional y visualizaciones con storytelling, se analizan los patrones de siniestralidad para caracterizar los perfiles de mayor riesgo y sentar las bases de modelos predictivos y de segmentación no supervisada que sirvan de insumo para la toma de decisiones en políticas públicas de tránsito y respuesta de emergencias médicas (SAME).

---

## 🎯 **Objetivo Propuesto**
El objetivo central consiste en responder:  
> *¿Qué factores temporales, territoriales y de interacción vehicular explican y anticipan la gravedad de los siniestros viales en CABA, y cómo podemos identificar agrupamientos naturales (clusters) de accidentes para intervenir preventivamente?*

Para ello, el proyecto se estructura en etapas:
1. **Pre-Entrega 2:** Análisis Exploratorio de Datos (AED), limpieza, ingeniería de features y visualizaciones.
2. **Pre-Entrega 3:** Modelo de clasificación multiclase supervisado para predecir la gravedad del hecho sin incurrir en *data leakage*.
3. **Pre-Entrega 4:** Modelo de clustering no supervisado para segmentar tipologías de siniestralidad.

---

## 📊 **Hallazgos Principales del Análisis Exploratorio (AED)**
1. **Dinámica Temporal y Resiliencia:** Tras una caída del 40,3% en 2020 por las restricciones de la pandemia COVID-19, la siniestralidad creció sostenidamente hasta registrar 11.752 hechos anuales en 2025, mientras que la mortalidad se mantuvo estable en ~100–110 muertes anuales.
2. **Patrón Horario Bimodal y Nocturno:** Los días hábiles concentran picos en horarios de traslado laboral (07:00–10:00 y 17:00–20:00 hs), mientras que los fines de semana la criticidad relativa se traslada a la madrugada (03:00–07:00 hs).
3. **Contraste Territorial y Vías Críticas:** La Comuna 1 concentra el mayor volumen absoluto de colisiones, pero las Comunas del Sur (Comunas 4, 8 y 9) registran las tasas más elevadas de siniestros graves y mortales. Entre los hechos con tipología de vía informada, las **Avenidas** concentran el **57,3%** de los choques (46,4% sobre el total general); sin embargo, las vías rápidas (**Autopistas** y **Av. General Paz**) triplican la tasa de mortalidad por cada 1.000 siniestros.
4. **Vulnerabilidad Extrema de Usuarios Ligeros:** La interacción vehicular más frecuente es **Moto vs. Auto** (>13.000 hechos). Los peatones presentan la mayor tasa de víctimas mortales por siniestro asociado (2,90%), seguidos por los motociclistas (1,17%).
5. **Fuerte Desbalance de la Variable Objetivo:** La severidad se distribuye en **94,46% Leves**, **4,49% Graves** y **1,05% Mortales**, justificando el uso de técnicas de remuestreo/pesado (`class_weight='balanced'`) y métricas especializadas (Macro F1-score, PR-AUC).

---

## 🤖 **Detalle de los Modelos (Hoja de Ruta Pre-Entregas 3 y 4)**
- **Modelo Supervisado (Pre-Entrega 3 - Clasificación de Severidad):** Se entrenará un modelo multiclase para predecir `gravedad_siniestro` comparando `DummyClassifier` (baseline), `LogisticRegression`, `DecisionTree`, `RandomForest` y `HistGradientBoostingClassifier`. Se aplica una política estricta de prevención de **Data Leakage**, excluyendo variables conocidas exclusivamente post-siniestro.
- **Modelo No Supervisado (Pre-Entrega 4 - Clustering de Siniestros):** Se aplicarán algoritmos `K-Means` y `DBSCAN` sobre características ex-ante (horario, día de semana, tipo de arteria, comuna y actores vehiculares involucrados), utilizando el Método del Codo y el Coeficiente de Silueta para descubrir perfiles de riesgo vial.

---

## 📂 **Estructura del Repositorio**
```text
DATA_SCIENCE_GROUP_8/
│
├── data/
│   ├── siniestros_viales_hechos.csv                    # Dataset oficial de Hechos (OMSV)
│   └── siniestros_viales_victimas.csv                  # Dataset oficial de Víctimas (OMSV)
│
├── Entrega_2_Analisis_Exploratorio_Siniestros_Viales_CABA.ipynb  # Notebook principal de la 2ª Pre-entrega
├── requirements.txt                                    # Dependencias y librerías del proyecto
├── .gitignore                                          # Exclusiones de Git (.venv, checkpoints, etc.)
├── revision.md                                         # Auditoría técnica, diagnósticos y checklist de mejoras
└── README.md                                           # Documentación general y guía de ejecución
```

---

## ⚡ **Instalación y Configuración del Entorno Virtual**

Para garantizar la reproducibilidad del proyecto se proveen dos alternativas de instalación: la opción **ultrarrápida con `uv`** (recomendada) y la opción tradicional con **Python `venv` + `pip`**.

### **Opción 1: Instalación Ultrarrápida con `uv` (Recomendada)**

[`uv`](https://github.com/astral-sh/uv) es un administrador y resolutor de paquetes de Python desarrollado en Rust, entre 10 y 100 veces más rápido que `pip`.

#### 1. Instalar `uv` (si aún no lo tienes instalado):
- **Windows (PowerShell):**
  ```powershell
  powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
  ```
- **macOS / Linux:**
  ```bash
  curl -LsSf https://astral.sh/uv/install.sh | sh
  ```
- **Alternativa con pip:**
  ```bash
  pip install uv
  ```

#### 2. Clonar el repositorio y crear el entorno:
```bash
# 1. Navegar a la carpeta del proyecto
cd DATA_SCIENCE_GROUP_8

# 2. Crear el entorno virtual con uv
uv venv .venv

# 3. Activar el entorno virtual:
# En Windows (PowerShell):
.venv\Scripts\Activate.ps1
# En Windows (CMD):
.venv\Scripts\activate.bat
# En Linux / macOS:
source .venv/bin/activate

# 4. Instalar todas las dependencias en segundos:
uv pip install -r requirements.txt
```

---

### **Opción 2: Instalación Estándar con `venv` y `pip`**

Si prefieres usar únicamente las herramientas nativas de Python:

```bash
# 1. Navegar a la carpeta del proyecto
cd DATA_SCIENCE_GROUP_8

# 2. Crear el entorno virtual nativo
python -m venv .venv

# 3. Activar el entorno:
# En Windows (PowerShell):
.venv\Scripts\Activate.ps1
# En Windows (CMD):
.venv\Scripts\activate.bat
# En Linux / macOS:
source .venv/bin/activate

# 4. Actualizar pip e instalar requerimientos
python -m pip install --upgrade pip
pip install -r requirements.txt
```

---

## 🧩 **Extensiones Recomendadas para Visual Studio Code**

Para trabajar de forma óptima con este proyecto en **VS Code**, se sugiere instalar las siguientes extensiones desde el panel de extensiones (`Ctrl + Shift + X` o `Cmd + Shift + X`):

| Extensión | ID de Extensión | Descripción |
| :--- | :--- | :--- |
| **Python** | `ms-python.python` | Soporte para autocompletado, linting, formateo y depuración de Python |
| **Jupyter** | `ms-toolsai.jupyter` | Visualización y ejecución interactiva de archivos `.ipynb` dentro del editor |
| **Jupyter Keymap** | `ms-toolsai.jupyter-keymap` | Atajos de teclado estándar de Jupyter (Modo Comando/Edición) |
| **Jupyter Cell Tags** | `ms-toolsai.vscode-jupyter-cell-tags` | Gestión de etiquetas y metadatos en celdas de Jupyter |
| **Pylance** | `ms-python.vscode-pylance` | Motor de análisis estático inteligente y tipado para Python |

---

## 🚀 **Cómo Correr el Código y Notebooks**

### **A. Ejecución en Visual Studio Code (Recomendado)**

1. Abre la carpeta del repositorio en VS Code (`File > Open Folder...` y selecciona `DATA_SCIENCE_GROUP_8`).
2. Abre el archivo [`Entrega_2_Analisis_Exploratorio_Siniestros_Viales_CABA.ipynb`](file:///c:/Users/Matias/Desktop/CURSOS%202026/ciencia_datos/DATA_SCIENCE_GROUP_8/Entrega_2_Analisis_Exploratorio_Siniestros_Viales_CABA.ipynb).
3. En la esquina superior derecha del notebook, haz clic en **Select Kernel** (o `Cambiar Kernel`).
4. Selecciona **Python Environments...** y elige el entorno virtual recién creado: **`.venv` (Python 3.x)**.
5. Ejecuta las celdas secuencialmente presionando `Shift + Enter` o haz clic en **Run All** en la barra superior del notebook.

### **B. Ejecución en Jupyter Lab / Notebook Clásico (Navegador Web)**

Si prefieres trabajar desde la interfaz web de Jupyter:

```bash
# 1. Asegúrate de tener el entorno activado
.venv\Scripts\activate

# 2. Iniciar Jupyter Lab
jupyter lab

# O iniciar Jupyter Notebook clásico
jupyter notebook
```
El navegador se abrirá automáticamente en `http://localhost:8888`. Abre el archivo `Entrega_2_Analisis_Exploratorio_Siniestros_Viales_CABA.ipynb` y ejecuta las celdas.

---

## 📋 **Control de Calidad y Validaciones**
El archivo [`revision.md`](file:///c:/Users/Matias/Desktop/CURSOS%202026/ciencia_datos/DATA_SCIENCE_GROUP_8/revision.md) contiene la auditoría técnica detallada del trabajo práctico, incluyendo el diagnóstico de integridad referencial, consistencia de datos, prevención de data leakage y la lista de control de mejoras implementadas.
