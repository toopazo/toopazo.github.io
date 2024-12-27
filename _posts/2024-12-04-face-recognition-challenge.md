---
title: Reconocimiento facial a partir de videos
layout: post
---

Dentro de la amplia gama de problemas que la visión computacional busca resolver, el problema de reconocimiento facial no requiere de mucha introducción. Es fácil entender su gran impacto en aplicaciones de negocios, seguridad, legales, etc. Probablemente todos los que lean este articulo se habrán encontrado con alguna aplicación de celular que la requiera. Y hasta puede que se hayan preguntado por que su aplicación no es aún más masiva. 

En este articulo me gustaría presentar mi trabajo en una variante particular del problema de reconocimiento facial: <<el reconocimiento de rostros a partir de videos>>. Explicado en sencillo, el desafío es lograr reconocer a la mayor cantidad de personas que aparecen en un video a medida que avanza. Es decir, si al segundo 23 apareció Juan Perez y al segundo 43 Maria Sandoval, queremos que el sistema reconozca a cada uno de ellos durante todo el tiempo que permanezcan en escena. Al final de la escena queremos tener una secuencia de recortes del rostro de Juan y otra secuencia de recortes del rostro de María. Si más adelante en el video, Juan o María vuelven a aparecer, queremos re-conocerlos y saber que son ellos. Aunque tengan otra ropa o estén en otro lugar físico. 

<center>
<figure>
  <img src="/assets/images/facial_recongnition_seqs.png" style="width:100%" alt="alt_text" /> 
</figure> 
</center>


## El sueño

El sueño de este proyecto es el siguiente. Imaginemos que ponemos una cámara en pleno Paseo Ahumada, y que grabamos los rostros de todos los que pasan por ahí. Detectamos a cada persona que transita, obtenemos la representación de sus rostros en un [vector de espacio latente](https://hackernoon.com/lang/es/espacio-latente-visualizacion-aprendizaje-profundo-bits-2-bd09a46920df) y los guardamos en una base de datos de gran tamaño. Es decir, muchos millones de secuencias, donde cada secuencia contiene el rostro de una persona a medida que se mueve en la escena. Un par de días después y en el otro extremo de la ciudad, activamos el sistema en un Mall concurrido. Allí realizamos la misma operación comparando a los transeúntes. El resultado esperado es darnos cuenta que la misma persona que caminó por el Paseo Ahumada, caminó por el Mall un par de días después. 

**ACLARACIÓN**: En esta propuesta NO estamos buscando identificar a las personas (e.g. nombre, rut, domicilio), sino re-conocer que las vimos y que son la misma persona (e.g rostro 2345 de la base de datos). 


<center>
<figure>
  <img src="/assets/images/face_recognition_dream.png" style="width:100%" alt="alt_text" /> 
</figure> 
</center>

## Pero, ¿es aún un sueño?. ¿No ha sido ya alcanzado?




## Los ingredientes tecnológicos

Para poder realizar el reconocimiento facial se necesita de varios pasos (ver imagen). En este proyecto se trabajó con las siguientes herramientas tecnológicas:

<center>
<figure>
  <img src="/assets/images/face_rec_steps.png" style="width:100%" alt="alt_text" /> 
</figure> 
</center>


### Videos
Un conjunto de videos que muestre a personas y sus rostros. En este proyecto simplemente saqué un video de Youtube sobre la [inauguración de Metro](https://www.youtube.com/watch?v=VQRlASe5dQI). Como primera etapa haremos re-conocimiento dentro del mismo video. Pero luego queremos extender esto a un conjunto de miles de videos. 

### Detector de rostros
Acá no hay mucho misterio. Los detectores tienen una larga historia dentro de la visión computacional. Son el segundo caso más usado luego del clasificador de imágenes. En este proyecto se usó el modelo [insightface](https://github.com/deepinsight/insightface) para detectar rostros de personas. 

### Estimador de pose

Para esta parte también usamos [insightface](https://github.com/deepinsight/insightface) ya que además de ser un modelo detector, entrega por cada rostro un vector que representa si la persona está mirando de frente, de costado, arriba abajo. Este estimador es muy importante para procesar solo los recortes de rostros en que la persona está mirando de frente y por tanto podrá ser reconocida de buena manera. 

### Descriptor de rostros

Este problema es muy antiguo dentro del área de reconocimiento de rostros. Antes del uso masivo de redes neuronales se usaban detectores de keypoints (ojos, nariz, boca, orejas, mejillas, etc) para elaborar matemáticamente un vector descriptor. Un buen resumen de estos algoritmos se puede ver en [Gururaj Et al. 2024 Comprehensive](/assets/docs/A_Comprehensive_Review_of_Face_Recognition_Techniques_Trends_and_Challenges.pdf). Algunos de estos métodos clásicos son: HOG, Eigenface, Independent Component analysis, Scale-Invariant Feature Transform (SIFT), Gabor filter, Local Phase Quantization (LPQ), Linear Discriminant Analysis (LDA), entre otros. 

Sin embargo, todo esto fue reemplazado por el uso de redes neuronales de aprendizaje profundo que generan vectores descriptores de mucho mayor calidad. Diversos métodos de entrenamiento han sido propuestos para mejorar el desempeño de estas redes. Los dos más conocidas son aquellos derivados de la función softmax de clasificación y el de triple-loss. 

En este proyecto usamos el modelo [insightface](https://github.com/deepinsight/insightface) que además de ser un modelo detector de rostros, entrega por cada rostro un vector descriptor. Este vectors descriptor es el que usaremos para comprar rostros. En particular el modelo que usamos fue entrenado usando una función de perdida llamada [ArcFace](https://insightface.ai/arcface) que [ha tenido muy buen desempeño](https://paperswithcode.com/paper/arcface-additive-angular-margin-loss-for-deep).


### Base de datos
En este proyecto se uso [Postgres](https://www.postgresql.org/about/). Esta es una base da datos de alto desempeño y muy usada. Se interactúa con ella a través de SQL. Y en ella se almacena la información de cada rostros detectado, y su relación con el frame del video en que fue obtenido. 

Existe un complemento de Postgres llamado [pgvector](https://github.com/pgvector/pgvector/) que es clave para transformar a una base de datos clásica en una base de datos para IA. Existe toda una industria centrada en interactuar con datos no estructurados que basan su funcionamiento en [bases de datos de vectores](https://www.ibm.com/topics/vector-database). Estos vectores son una especia de <<tarjeta de presentación>> de los datos no estructurados (pdf, imágenes, audio, etc).

###  Comparación de vectores

Esta es un área de investigación muy activa y compleja, ya que el uso de vectores descriptores es la base sobre la que se sostienen muchas aplicaciones: búsqueda de imágenes, sugerencias de compra, minería de datos, etc. Además de esto, la industria de vectores descriptores está siendo usada para dar algo de orden (buscar, relacionas, entender) la gigante masa de datos no estructurados.

En el caso del reconocimiento de rostros. La forma más sencilla (pero no la más robusta) de reconocer rostros es comparar el vector latente del desconocido, contra los vectores de rostros de la base de datos. Hacer una resta y fijar un valor umbralo, bajo el cual diremos que es la misma persona y sobre el cual diremos que no lo es. Este método funciona, y si tenemos fotos muy bien tomadas, en alta resolución y buena iluminación si funciona. Pero es obviamente muy susceptible a errores cuando las condiciones no son las optimas (mala iluminación, baja resolución o mala calidad de la foto, la persona no está mirando de frente, uso de lentes, mascarilla, etc). En este proyecto usaremos un algoritmo basado en secuencias de rostros recortadas desde el video. El uso de una secuencia, en vez de solo un recorte, busca hacer mucho más robusto al algoritmo. 
 
<!-- ## Los desafíos -->

## El código

El código esta disponible para su uso libre y gratuito en [https://github.com/toopazo/airflow](https://github.com/toopazo/airflow). Allí mismo están todas las instrucciones de instalación y ejecución. 

El código hace uso de Docker para levantar una imagen de Postgres y una imagen de inferencia, ademas de otra para realizar el procesamiento de IA y el reconocimiento de rostros. 


## Resultados

### Resultados al dia 5 de Diciembre de 2024. 

Actualmente el código lee el video y procesa cada frame. Extrae de él los rostros y los almacena en las siguientes tablas de la base de datos

<center>
<figure>
  <img src="/assets/images/imaged_0041.png" style="width:100%" alt="alt_text" /> 
</figure> 
</center>


<center>
<figure>
  <img src="/assets/images/tables_in_database.png" style="width:100%" alt="alt_text" /> 
</figure> 
</center>

Luego el sistema consulta directamente a la base de datos para ejecutar los pasos:

- [Paso 1] Filtrado de los rostros: Se filtran los rostros detectados para solo utilizar aquellos que sean <<reconocibles>>. Es decir, que la resolución sea suficiente, que miren de frente, etc.
- [Paso 2] Detección de secuencias: Cada rostro detectado forma parte de una secuencia de imágenes en donde la persona está en escena. Toda la secuencia se detecta y se le asigna una ID a manera de tracking de rostros. Este ID es utilizado para luego re-conocer cuando ese rostros vuelva a aparecer en escena. 
- [Paso 3] Comparación de vectores: Acá se comparan secuencias de rostros actualmente visibles (ver ejemplo de la imagen de más abajo) contra todo lo almacenado en la base datos en búsqueda de coincidencias. 


<center>
<figure>
  <img src="/assets/images/facial_recongnition_seqs_v2.png" style="width:100%" alt="alt_text" /> 
</figure> 
</center>

{% include youtube.html id="heeUrgj27DI" %}
