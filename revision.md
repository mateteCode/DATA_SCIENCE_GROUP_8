# Revisión del trabajo práctico

## Alcance

Se revisaron el README, el notebook `Entrega_2_Analisis_Exploratorio_Siniestros_Viales_CABA.ipynb` y los dos datasets disponibles en `data/`. También se reprodujeron los controles principales de integridad y varias métricas del análisis con pandas.

## Diagnóstico general

El proyecto tiene una base sólida y cumple el propósito de una segunda pre-entrega de Análisis Exploratorio de Datos: presenta el problema, documenta los datasets, limpia variables, crea características temporales y territoriales, integra hechos con víctimas y propone una hoja de ruta para clasificación y clustering.

La integridad básica de los datos es consistente:

- `siniestros_viales_hechos.csv`: 65.818 filas y 21 columnas.
- `siniestros_viales_victimas.csv`: 75.197 filas y 9 columnas.
- 65.818 IDs únicos de siniestro en ambas tablas.
- No se encontraron IDs duplicados en la tabla de hechos.
- No se encontraron IDs huérfanos en víctimas.
- Se encontraron 4 filas completamente vacías en víctimas, que el notebook elimina.
- La distribución de `gravedad_siniestro` coincide con la documentada: 94,46% leves, 4,49% graves y 1,05% mortales.

## Hallazgos correctos o reproducibles

- La cantidad de siniestros cae aproximadamente 40,3% entre 2019 y 2020.
- En 2025 se registran 11.752 siniestros.
- La distribución de gravedad está fuertemente desbalanceada.
- La integración por `id_siniestro` conserva las 65.818 filas de hechos.
- La proporción de víctimas con sexo identificado que figura como masculina es aproximadamente 65,8%.
- La combinación Moto vs. Auto supera los 13.000 hechos; el filtro utilizado produce 15.247 registros.
- La prevención de `data leakage` está correctamente planteada para las futuras etapas de modelado.

---

## Findings, riesgos y estado de resolución

### - [x] 1. Rutas de carga incorrectas (RESUELTO)

El notebook buscaba los CSV en la raíz y en `TP-grupal/`. La estructura real del repositorio contiene los archivos en la carpeta `data/`.

**Estado:** ✅ **Resuelto.**
Se actualizaron las rutas en el notebook para priorizar `data/siniestros_viales_hechos.csv` y `data/siniestros_viales_victimas.csv`

```python
path_hechos = 'data/siniestros_viales_hechos.csv'
path_victimas = 'data/siniestros_viales_victimas.csv'
```

### - [x] 2. Uso de datasets locales versus GitHub (RESUELTO)

Se desaconsejó la dependencia de URLs remotas para la ejecución local.

**Estado:** ✅ **Resuelto.**
Se mantienen los CSV en la carpeta local `data/`, se configuran rutas relativas y se documentó la fuente oficial de BA Data en el README.

### - [x] 3. Porcentaje de avenidas (RESUELTO EN DOCUMENTACIÓN)

El porcentaje del 57,4% de siniestros en avenidas corresponde exclusivamente a los hechos con tipo de vía informado (sobre el total general representa el 46,4%).

**Estado:** ✅ **Resuelto en README.**
Se reformuló en el README explicitando el denominador: *"Entre los hechos con tipología de vía informada, las Avenidas concentran el 57,3% de los choques (46,4% sobre el total general)"*.

### - [ ] 4. Tasas de letalidad por modo (PENDIENTE DE REVISIÓN EN TEXTO DE CELDAS)

La celda de severidad agrupa hechos según `modo_desplazamiento_victima` y divide las víctimas mortales por la cantidad de hechos. Esa métrica no es estrictamente una letalidad individual, porque el denominador no es la cantidad de víctimas sino de hechos asociados.

- Peatón: 2,90% de víctimas mortales por hecho asociado.
- Moto: 1,17% de víctimas mortales por hecho asociado.

**Recomendación:** explicitar en los gráficos y conclusiones si se trata de letalidad individual o tasa por hecho asociado.

### - [ ] 5. Numeradores y denominadores mezclados (PENDIENTE)

En varias visualizaciones se cuentan hechos mediante `id_siniestro`, pero se suman víctimas graves o mortales desde columnas de la tabla de hechos.

**Recomendación:** rotular cada métrica con claridad (`hechos`, `víctimas`, `víctimas mortales por 1.000 hechos`).

### - [ ] 6. Afirmaciones escritas sin validación automática (PENDIENTE)

Los hallazgos están escritos manualmente en celdas Markdown y no contienen aserciones automáticas que comprueben que las cifras sigan coincidiendo tras modificaciones.

**Recomendación:** agregar una celda final de validación con métricas clave y `assert`.

### - [x] 7. Dependencias del entorno (RESUELTO)

La ejecución requería un entorno estandarizado con pandas, numpy, matplotlib, seaborn, scikit-learn y jupyter.

**Estado:** ✅ **Resuelto.**
- Se generó el archivo `requirements.txt`.
- Se configuró el entorno virtual `.venv` con soporte para instalación ultrarrápida mediante `uv` y mediante `pip` estándar.
- Se documentó el procedimiento completo paso a paso en el `README.md`.

---

## Completitud

Para una segunda pre-entrega de AED, el notebook cubre los componentes esperados:

- Contexto y justificación.
- Preguntas de investigación.
- Carga e inspección de datos.
- Limpieza y normalización.
- Ingeniería de características.
- Integración relacional.
- Análisis temporal, territorial, vehicular y sociodemográfico.
- Análisis del desbalance del target.
- Prevención de `data leakage`.
- Plan para clasificación y clustering.
- Conclusiones ejecutivas.

Lo que todavía no corresponde considerar terminado en esta entrega es la implementación de los modelos supervisados y no supervisados: el notebook sólo presenta su metodología futura.

---

## Recomendaciones priorizadas (Checklist de Correcciones)

### Prioridad alta

- [x] **1. Corregir las rutas a `data/`:** Notebook actualizado para cargar datasets desde la carpeta `data/`.
- [x] **2. Corregir la estructura del repositorio documentada en el README:** README actualizado con la estructura real del proyecto (`data/`, `requirements.txt`, `.gitignore`, etc.).
- [ ] **3. Revisar las cifras de letalidad y aclarar los denominadores:** Ajustar textos de conclusiones en el notebook según letalidad individual vs. tasa por siniestro.
- [x] **4. Reformular el 57,4% de avenidas indicando que se calcula sobre vías informadas:** Documentación del README corregida con la precisión metodológica.
- [x] **5. Instalar y documentar las dependencias necesarias:** Creado `requirements.txt`, entorno virtual `.venv` con `uv` y guía en `README.md`.

### Prioridad media

- [ ] **1. Agregar una celda de validación reproducible:** Celda con `assert` de dimensiones e integridad al final del notebook.
- [ ] **2. Renombrar métricas para distinguir hechos de víctimas:** Homogeneizar leyendas y títulos en gráficos.
- [ ] **3. Documentar el significado de `SD`, valores nulos y filas eliminadas:** Añadir detalle en la sección de limpieza.
- [ ] **4. Añadir la fuente oficial y fecha de extracción de los datos:** Enriquecer metadatos de BA Data en el notebook.

### Prioridad baja

- [x] **1. Reducir imports no utilizados, como `sys`:** Limpieza de imports innecesarios.
- [ ] **2. Revisar advertencias de seaborn sobre `palette` sin `hue`:** Ajustar sintaxis moderna en Seaborn 0.13+.
- [ ] **3. Agregar una nota sobre las limitaciones de inferir causalidad a partir de un AED descriptivo.**

---

## Conclusión

El trabajo es conceptualmente consistente y defendible como análisis exploratorio de siniestros viales en CABA. Los controles de integridad, el tratamiento del desbalance y la prevención de data leakage están bien orientados. Con la corrección de rutas de carga, la estructura del proyecto y la gestión de dependencias con `uv` y `requirements.txt`, el proyecto queda listo para su ejecución reproducible.

La principal defensa metodológica es:

> La tabla de hechos se utiliza para estudiar siniestros; la tabla de víctimas se utiliza para estudiar personas afectadas; y las víctimas se agregan por `id_siniestro` antes de integrarlas para evitar duplicar hechos.
