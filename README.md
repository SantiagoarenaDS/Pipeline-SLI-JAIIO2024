# Pipeline SLI - JAIIO / ASAID 2024

Pipeline de machine learning para la deteccion del Trastorno Especifico del Lenguaje (SLI) a partir de transcripciones de narrativas espontaneas.

El trabajo fue publicado en JAIIO, Jornadas Argentinas de Informatica, Vol. 10 Num. 1 (2024), dentro del Simposio Argentino de Inteligencia Artificial y Ciencia de Datos (ASAID), paginas 209-222.

Articulo oficial: https://revistas.unlp.edu.ar/JAIIO/article/view/17908

Autores del paper:

- Santiago Arena, Pontificia Universidad Catolica Argentina.
- Antonio Quintero-Rincon, Pontificia Universidad Catolica Argentina.

Ademas, en agosto de 2024 participe de la instancia JAIIO junto a Ricardo Di Pasquale, director de la carrera de Ciencia de Datos de la UCA y ML Engineering Associate Director en Accenture.

## Resumen

El SLI es un trastorno que afecta la comunicacion y puede comprometer tanto la comprension como la expresion. Este proyecto propone un pipeline de tres etapas en cascada para detectar SLI en ninos usando 1063 transcripciones de narrativas espontaneas.

La propuesta combina reduccion de dimensionalidad, seleccion de variables explicables y clasificacion supervisada. El objetivo es evitar depender de variables subjetivas complejas y centrar el analisis en metricas cuantitativas directamente relacionadas con el desempeno narrativo del nino.

## Resultados principales

- 1063 entrevistas analizadas.
- Ingesta inicial de 62 variables.
- Limpieza y seleccion manual hasta 43 variables cuantitativas.
- Reduccion de dimensionalidad de 43 a 11 caracteristicas usando Random Forest y correlacion Spearman.
- Seleccion final de 6 caracteristicas mediante regresion logistica iterativa.
- Clasificacion final con modelo 14-NN.
- Precision reportada: 97.13%.
- F1-score: 98.74%.
- Sensibilidad: 98.71%.
- Especificidad: 95.06%.

Matriz de confusion final del 14-NN con 6 caracteristicas:

```text
                Predicho P   Predicho N
P real              238           5
N real                4          67
```

## Pipeline metodologico

El pipeline se organiza en tres etapas:

### 1. Extraccion y reduccion de dimensionalidad

Se parte de variables linguisticas extraidas de narrativas espontaneas. Luego de la limpieza inicial se conservan 43 variables cuantitativas relacionadas con la narrativa.

La primera reduccion combina:

- Random Forest.
- Importancia por Mean Decrease Gini.
- Correlacion Spearman contra la variable objetivo.

El criterio usado en el paper selecciona variables con:

```text
Gini > 6
|correlacion| > 0.1
```

Esta etapa reduce el conjunto de 43 a 11 caracteristicas.

### 2. Seleccion explicativa con regresion logistica

Sobre las 11 caracteristicas seleccionadas, se aplica regresion logistica de forma iterativa. En cada ciclo se descartan variables que no cumplen significancia estadistica.

El criterio de permanencia es:

```text
p-value < 0.05
```

El proceso finaliza con 6 variables.

### 3. Clasificacion con 14-NN

Las 6 variables finales alimentan un clasificador k-NN. El trabajo utiliza una particion aleatoria de entrenamiento y prueba:

```text
train: 70%
test: 30%
```

El valor de `k` se valida experimentalmente, y el modelo final reportado es 14-NN.

## Variables finales

Las 6 variables finales reportadas en el paper son:

- Verbos sin declinar.
- Morfemas por oracion.
- Errores de palabra.
- Promedio de silabas por palabra.
- Frecuencia de tipos de palabras.
- Uso regular del pasado.

Estas variables permiten sostener un balance importante entre capacidad predictiva e interpretabilidad. Esto es clave porque el usuario final potencial puede ser clinico, investigador del lenguaje o equipo interdisciplinario.

## Stack

- R
- R Markdown
- caret
- randomForest
- ggplot2
- ROCR
- dplyr
- tidyverse
- FSelector
- CORElearn

## Archivos

- `PIPE SLI SANTIAGO ARENA.Rmd`: notebook principal del pipeline.
- `all_data_R.csv`: dataset de trabajo.
- `LICENSE`: licencia del repositorio.

## Sobre Ricardo Di Pasquale

Ricardo Di Pasquale es director de la carrera de Ciencia de Datos de la UCA y ML Engineering Associate Director en Accenture. Su perfil combina direccion academica, machine learning aplicado e ingenieria de modelos en contextos productivos.

LinkedIn: https://www.linkedin.com/in/ricardodipasquale/

## Cita

Arena, S., & Quintero-Rincon, A. (2024). Pipeline para la deteccion del trastorno especifico del lenguaje (SLI) a partir de transcripciones de narrativas espontaneas. JAIIO, Jornadas Argentinas de Informatica, 10(1), 209-222.
