<a name="readme-top"></a>

# Fake News Detection

> En este repositorio se adjunta un notebook donde se desarrolla un modelo de clasificación para detectar noticias falsas utilizando técnicas de procesamiento de lenguaje natural y modelos de machine learning.
> El proyecto utiliza el dataset **Fake and Real News Dataset**, compuesto por noticias falsas y noticias reales almacenadas en los archivos `Fake.csv` y `True.csv`.

## Objetivo del proyecto

El objetivo principal es construir y comparar modelos capaces de clasificar noticias como:

* `0`: Noticia real
* `1`: Noticia falsa

Para ello se aplicó limpieza de texto, eliminación de duplicados, vectorización con TF-IDF y entrenamiento de diferentes modelos supervisados.

## Built With

* Python
* Visual Studio
* pandas
* numpy
* matplotlib
* nltk
* scikit-learn
* xgboost

## Dataset

El dataset utilizado fue **Fake and Real News Dataset**, compuesto por dos archivos principales:

* `Fake.csv`: noticias falsas
* `True.csv`: noticias reales

## Flujo del proyecto

El notebook sigue el siguiente flujo de trabajo:

1. Carga de datos.
2. Asignación de etiquetas para noticias reales y falsas.
3. Unión de los datasets `Fake.csv` y `True.csv`.
4. Exploración inicial de datos.
5. Limpieza y normalización del texto.
6. Eliminación de textos duplicados.
7. División en datos de entrenamiento y prueba.
8. Vectorización del texto con TF-IDF.
9. Entrenamiento de modelos de clasificación.
10. Evaluación mediante Accuracy y ROC AUC.
11. Análisis de interpretabilidad con los coeficientes de Logistic Regression.
12. Identificación de posibles artefactos del dataset.

## Modelos utilizados

Se compararon tres modelos de clasificación:

* Logistic Regression
* Support Vector Classifier
* XGBoost Classifier

### Palabras asociadas a noticias falsas

Algunas palabras con coeficientes positivos altos fueron:

* `via`
* `imag`
* `gop`
* `american`
* `hillari`
* `obama`
* `america`

Estas palabras sugieren que el modelo identificó patrones frecuentes en las noticias falsas del dataset, especialmente relacionados con estilo de redacción, referencias a imágenes o temas políticos.

### Palabras asociadas a noticias reales

Algunas palabras con coeficientes negativos altos fueron:

* `reuter`
* `said`
* `wednesday`
* `washington`
* `tuesday`
* `thursday`
* `friday`
* `monday`

Estas palabras parecen estar asociadas al formato editorial de noticias reales, particularmente al estilo de agencia periodística.


## Autor

**Paul H. Roldan Hernandez**

<p align="right">(<a href="#readme-top">back to top</a>)</p>
