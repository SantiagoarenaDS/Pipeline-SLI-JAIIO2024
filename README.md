# Pipeline SLI - JAIIO 2024

Pipeline de machine learning para la deteccion del Trastorno Especifico del Lenguaje (SLI) a partir de transcripciones de narrativas espontaneas.

El proyecto fue presentado en agosto de 2024 en la 53 JAIIO. La presentacion fue realizada por Santiago Arena junto a Ricardo Di Pasquale, director de la carrera de Ciencia de Datos de la UCA y ML Engineering Associate Director en Accenture.

## Contexto

El Trastorno Especifico del Lenguaje puede observarse a partir de patrones linguisticos presentes en narrativas espontaneas. Este trabajo explora si variables morfosintacticas, lexico-semanticas y de error pueden ayudar a diferenciar casos SLI de desarrollo tipico.

La propuesta no busca reemplazar criterio clinico, sino construir un pipeline reproducible que permita:

- Limpiar y preparar datos linguisticos.
- Reducir dimensionalidad de variables.
- Seleccionar indicadores interpretables.
- Entrenar modelos de clasificacion.
- Evaluar desempeno con metricas claras.
- Comunicar resultados a una audiencia academica.

## Dataset

El archivo principal es:

```text
all_data_R.csv
```

Incluye variables asociadas a transcripciones, como:

- Conteos de palabras y silabas.
- Morfemas y medidas de longitud media.
- Errores de palabra.
- Variables gramaticales.
- Indicadores de verbos, auxiliares y articulos.
- Etiqueta objetivo `Y`, que distingue SLI y desarrollo tipico.

## Pipeline tecnico

El flujo se desarrolla en R Markdown:

```text
PIPE SLI SANTIAGO ARENA.Rmd
```

Etapas principales:

1. Instalacion y carga de librerias.
2. Lectura de datos desde CSV.
3. Limpieza de vacios, duplicados y variables con alta presencia de nulos.
4. Normalizacion de variables categoricas como sexo.
5. Seleccion inicial de variables relevantes.
6. Calculo de correlaciones Spearman contra la variable objetivo.
7. Entrenamiento de Random Forest para obtener importancia por Mean Decrease Gini.
8. Visualizacion de importancia vs correlacion.
9. Regresiones logisticas iterativas para depurar variables.
10. Entrenamiento de KNN con validacion cruzada.
11. Evaluacion con matriz de confusion, perdida cero-uno, ROC y AUC.

## Seleccion de variables

El trabajo parte de un conjunto amplio de variables y reduce el espacio de caracteristicas para priorizar interpretabilidad y capacidad predictiva.

Variables destacadas durante el analisis:

- `r_2_i_verbs`
- `mlu_morphemes`
- `word_errors`
- `regular_past_ed`
- `average_syl`
- `freq_ttr`

La seleccion combina importancia de Random Forest, correlacion con la variable target y significancia en regresion logistica.

## Modelos

### Random Forest

Se utiliza para analizar importancia de variables y estudiar el error OOB. El pipeline explora valores de `mtry` y usa importancia Gini para detectar atributos con mayor contribucion.

### Regresion logistica

Se entrena de forma iterativa para conservar variables estadisticamente utiles y explicables. Este paso aporta lectura interpretativa sobre que indicadores linguisticos contribuyen a la clasificacion.

### KNN

Se entrena un clasificador KNN con particion train/test y validacion cruzada para seleccionar `k`. El modelo se evalua con matriz de confusion, perdida cero-uno y curva ROC.

## Presentacion en JAIIO

La presentacion en JAIIO 2024 fue una instancia clave para comunicar el trabajo frente a una audiencia academica y tecnica. El foco estuvo en explicar:

- Como se transforma una narrativa espontanea en variables analizables.
- Como se seleccionan caracteristicas relevantes.
- Como se evalua un clasificador para SLI.
- Por que la interpretabilidad importa cuando el usuario final puede ser clinico o investigador del lenguaje.

## Sobre Ricardo Di Pasquale

Ricardo Di Pasquale es director de la carrera de Ciencia de Datos de la UCA y ML Engineering Associate Director en Accenture. Su perfil profesional combina direccion academica, machine learning aplicado e ingenieria de modelos en contextos productivos.

LinkedIn: https://www.linkedin.com/in/ricardodipasquale/

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

## Autor

Santiago Arena.
