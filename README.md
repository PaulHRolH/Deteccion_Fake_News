<a name="readme-top"></a>

# Fake News Detection

> En este repositorio se adjunta un notebook donde se desarrolla un modelo de clasificación para detectar noticias falsas utilizando técnicas de procesamiento de lenguaje natural y modelos de machine learning.
> El proyecto utiliza el dataset **Fake and Real News Dataset**, compuesto por noticias falsas y noticias reales almacenadas en los archivos `Fake.csv` y `True.csv`.

## Objetivo del proyecto

El objetivo principal es construir, comparar e interpretar modelos capaces de clasificar noticias como:

* `0`: Noticia real
* `1`: Noticia falsa

Para ello se aplicó limpieza de texto, eliminación de duplicados, vectorización con TF-IDF y entrenamiento de diferentes modelos supervisados.

Además, se realizó un análisis de interpretabilidad con los coeficientes de **Logistic Regression** para identificar posibles palabras o patrones que estuvieran influyendo de forma importante en la clasificación.

## Built With

* Python
* Jupyter Notebook
* Visual Studio Code
* pandas
* numpy
* matplotlib
* plotly
* nltk
* scikit-learn
* xgboost
* BeautifulSoup

## Dataset

El dataset utilizado fue **Fake and Real News Dataset**, compuesto por dos archivos principales:

* `Fake.csv`: noticias falsas
* `True.csv`: noticias reales

Durante el preprocesamiento se identificaron **6,467 textos duplicados**, los cuales fueron eliminados antes de realizar la división entre entrenamiento y prueba. Esto se hizo para reducir el riesgo de fuga de información entre ambos conjuntos.

Después de eliminar duplicados, el dataset quedó compuesto por aproximadamente **38,431 registros**.

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
10. Evaluación mediante Accuracy y curvas ROC.
11. Análisis de interpretabilidad con los coeficientes de Logistic Regression.
12. Identificación de posibles artefactos del dataset.
13. Reentrenamiento eliminando palabras asociadas a fuente, formato o estructura editorial.

## Preprocesamiento de texto

El texto fue limpiado mediante diferentes transformaciones, entre ellas:

* Conversión a minúsculas.
* Eliminación de caracteres especiales.
* Eliminación de etiquetas HTML.
* Tokenización.
* Eliminación de stopwords.
* Aplicación de stemming con `PorterStemmer`.

Posteriormente, los textos fueron vectorizados con **TF-IDF**.

En la versión v2 se utilizó una configuración como la siguiente:

```python
vectorizer = TfidfVectorizer(
    stop_words=words_to_delete,
    min_df=2,
    max_df=0.95
)
```

Donde:

* `min_df=2`: elimina palabras que aparecen en menos de 2 documentos.
* `max_df=0.95`: elimina palabras que aparecen en más del 95% de los documentos.
* `stop_words=words_to_delete`: elimina palabras identificadas como posibles artefactos del dataset.

## Modelos utilizados

Se compararon tres modelos de clasificación:

* Logistic Regression
* Support Vector Classifier
* XGBoost Classifier

## Primera versión del modelo

En la primera versión, los modelos obtuvieron métricas muy altas. Sin embargo, al analizar los coeficientes de **Logistic Regression**, se identificaron palabras muy asociadas a cada clase que parecían estar relacionadas con el formato o la fuente de los textos.

### Palabras inicialmente asociadas a noticias falsas

Algunas palabras con coeficientes positivos altos fueron:

* `via`
* `imag`
* `gop`
* `american`
* `hillari`
* `obama`
* `america`

### Palabras inicialmente asociadas a noticias reales

Algunas palabras con coeficientes negativos altos fueron:

* `reuter`
* `said`
* `wednesday`
* `washington`
* `tuesday`
* `thursday`
* `friday`
* `monday`

Este análisis mostró que el modelo podía estar aprendiendo patrones propios del dataset, como fuente de la noticia, formato editorial, días de publicación o créditos de imagen, en lugar de aprender únicamente características generales de noticias falsas o reales.

## Identificación de artefactos del dataset

Se analizaron palabras que aparecían de forma muy desbalanceada entre noticias reales y falsas. Algunas de las palabras identificadas como posibles artefactos fueron:

* `reuter`
* `said`
* `monday`
* `tuesday`
* `wednesday`
* `thursday`
* `friday`
* `via`
* `imag`
* `image`
* `washington`

Estas palabras fueron eliminadas en la versión v2 para evaluar si el modelo seguía manteniendo un buen desempeño sin depender de señales demasiado específicas del dataset.

## Versión v2: modelo sin palabras artefacto

En la versión v2 se reentrenaron los modelos eliminando las palabras asociadas a fuente, formato o estructura editorial.

La lista de palabras eliminadas fue:

```python
words_to_delete = [
    "reuter", "said",
    "monday", "tuesday", "wednesday", "thursday", "friday",
    "via", "imag", "image", "washington"
]
```

Después de eliminar estas palabras, los modelos conservaron un desempeño alto, aunque menor que en la primera versión.

| Modelo              | Accuracy |
| ------------------- | -------: |
| Logistic Regression |   0.9690 |
| SVC                 |   0.9762 |
| XGBoost             |   0.9710 |

El modelo con mejor accuracy en esta versión fue **SVC**, seguido por **XGBoost** y **Logistic Regression**.

## Interpretabilidad en la versión v2

Después de eliminar las palabras artefacto, se volvió a analizar la importancia de las palabras usando los coeficientes de Logistic Regression.

Como las etiquetas fueron definidas como:

* `0`: Noticia real
* `1`: Noticia falsa

Entonces:

* Coeficientes positivos favorecen la clasificación como noticia falsa.
* Coeficientes negativos favorecen la clasificación como noticia real.

### Palabras asociadas a noticias falsas en v2

Algunas palabras con coeficientes positivos altos fueron:

* `gop`
* `even`
* `american`
* `hillari`
* `america`
* `mr`
* `like`
* `fact`
* `video`
* `wire`

Estas palabras sugieren que, después de eliminar artefactos editoriales, el modelo sigue capturando patrones de estilo, vocabulario y temas políticos frecuentes en las noticias falsas del dataset.

El término `gop` corresponde a **Grand Old Party**, apodo histórico del Partido Republicano de Estados Unidos. Su asociación con noticias falsas no implica que el término sea falso por sí mismo, sino que el dataset contiene una concentración temática de noticias falsas relacionadas con política estadounidense.

### Palabras asociadas a noticias reales en v2

Algunas palabras con coeficientes negativos altos fueron:

* `us`
* `dont`
* `statement`
* `presidenti`
* `spokesman`
* `minist`
* `im`
* `told`
* `nov`
* `comment`

En esta versión, la palabra `us` aparece como una señal importante hacia noticias reales. Esto puede deberse a que durante la limpieza expresiones como `U.S.` se transforman en `us`, por lo que todavía puede existir cierto efecto del estilo editorial de noticias relacionadas con Estados Unidos.


## Autor

**Paul H. Roldan Hernandez**

<p align="right">(<a href="#readme-top">back to top</a>)</p>

