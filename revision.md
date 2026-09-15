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

## Findings y riesgos

### 1. Rutas de carga incorrectas

El notebook busca los CSV en la raíz y, como alternativa, en `TP-grupal/`:

```python
path_hechos = 'siniestros_viales_hechos.csv'
path_victimas = 'siniestros_viales_victimas.csv'

if not os.path.exists(path_hechos):
    path_hechos = 'TP-grupal/siniestros_viales_hechos.csv'
```

La estructura real es:

```text
data/siniestros_viales_hechos.csv
data/siniestros_viales_victimas.csv
```

Por lo tanto, el notebook no es reproducible desde la raíz del repositorio sin modificar las rutas.

**Recomendación:** usar rutas relativas a `data/`:

```python
path_hechos = 'data/siniestros_viales_hechos.csv'
path_victimas = 'data/siniestros_viales_victimas.csv'
```

También debe actualizarse la estructura indicada en el README.

### 2. Uso de datasets locales versus GitHub

No se recomienda que el notebook dependa de URLs de GitHub como mecanismo principal de carga. Los archivos ya forman parte del repositorio y las rutas relativas son más reproducibles, funcionan sin conexión y no dependen de cambios en la URL o en la rama.

La alternativa recomendada es:

- Mantener los CSV en `data/`.
- Cargarlos con rutas relativas.
- Documentar en el README el repositorio y la fuente original de BA Data.
- Opcionalmente, ofrecer una URL `raw.githubusercontent.com` como alternativa de descarga o respaldo.

Si los archivos fueran demasiado grandes para Git, habría que usar Git LFS, una descarga desde la fuente oficial o un almacenamiento de datos versionado. En el estado actual, los CSV ya están disponibles en el repositorio.

### 3. Porcentaje de avenidas

El README y el notebook indican que las avenidas concentran el 57,4% de los choques. Ese porcentaje sólo se obtiene cuando se excluyen los registros cuyo tipo de vía no está informado. Sobre el total de hechos, las avenidas representan aproximadamente 46,4%.

**Recomendación:** reformular como:

> Entre los siniestros con tipo de vía informado, las avenidas representan aproximadamente el 57,3%.

Debe explicitarse siempre el denominador.

### 4. Tasas de letalidad por modo

La celda de severidad agrupa hechos según `modo_desplazamiento_victima` y divide las víctimas mortales por la cantidad de hechos. Esa métrica no es estrictamente una letalidad individual, porque el denominador no es la cantidad de víctimas.

Con el cálculo actual se obtienen aproximadamente:

- Peatón: 2,90% de víctimas mortales por hecho asociado.
- Moto: 1,17% de víctimas mortales por hecho asociado.

El texto afirma 2,38% y 1,12%, valores que no coinciden con el código ni con la ejecución reproducida.

**Recomendación:** elegir una de estas alternativas:

1. Mantener el cálculo y llamarlo `tasa de víctimas mortales por siniestro asociado al modo`.
2. Calcular letalidad individual desde la tabla de víctimas, usando víctimas mortales de ese grupo dividido por el total de víctimas del grupo.

No deben presentarse ambas métricas con el mismo nombre.

### 5. Numeradores y denominadores mezclados

En varias visualizaciones se cuentan hechos mediante `id_siniestro`, pero se suman víctimas graves o mortales desde columnas de la tabla de hechos. Esto puede ser válido si se describe como una tasa de víctimas por hecho, pero no equivale a la proporción de hechos graves ni a la letalidad individual.

**Recomendación:** rotular cada métrica con claridad:

- `hechos` para cantidad de siniestros.
- `víctimas` para cantidad de personas.
- `víctimas mortales por 1.000 hechos` para tasas que mezclan ambos niveles.
- `proporción de hechos graves o mortales` cuando el numerador se convierta primero en indicador por hecho.

### 6. Afirmaciones escritas sin validación automática

Los hallazgos están escritos manualmente en celdas Markdown y no contienen aserciones o tablas de control que comprueben que las cifras siguen coincidiendo después de cambiar los datos.

**Recomendación:** agregar una celda final de validación con métricas clave y, cuando corresponda, `assert` para dimensiones, IDs y categorías esperadas.

### 7. Dependencias del entorno

La ejecución gráfica requiere pandas, numpy, matplotlib y seaborn. El entorno utilizado durante la revisión no tenía matplotlib instalado, por lo que no fue posible ejecutar todos los gráficos automáticamente.

**Recomendación:** incorporar un `requirements.txt` o documentar explícitamente la instalación de dependencias. Como mínimo:

```text
pandas
numpy
matplotlib
seaborn
jupyter
```

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

## Recomendaciones priorizadas

### Prioridad alta

1. Corregir las rutas a `data/`.
2. Corregir la estructura del repositorio documentada en el README.
3. Revisar las cifras de letalidad y aclarar los denominadores.
4. Reformular el 57,4% de avenidas indicando que se calcula sobre vías informadas.
5. Instalar y documentar las dependencias necesarias.

### Prioridad media

1. Agregar una celda de validación reproducible.
2. Renombrar métricas para distinguir hechos de víctimas.
3. Documentar el significado de `SD`, valores nulos y filas eliminadas.
4. Añadir la fuente oficial y fecha de extracción de los datos.

### Prioridad baja

1. Reducir imports no utilizados, como `sys`.
2. Revisar advertencias de seaborn sobre `palette` sin `hue`.
3. Agregar una nota sobre las limitaciones de inferir causalidad a partir de un AED descriptivo.

## Conclusión

El trabajo es conceptualmente consistente y defendible como análisis exploratorio de siniestros viales en CABA. Los controles de integridad, el tratamiento del desbalance y la prevención de data leakage están bien orientados. Sin embargo, antes de presentarlo como completamente correcto conviene corregir las rutas de datos, alinear las cifras escritas con los cálculos reales y hacer explícita la unidad de análisis de cada tasa.

La principal defensa metodológica debería ser:

> La tabla de hechos se utiliza para estudiar siniestros; la tabla de víctimas se utiliza para estudiar personas afectadas; y las víctimas se agregan por `id_siniestro` antes de integrarlas para evitar duplicar hechos.
