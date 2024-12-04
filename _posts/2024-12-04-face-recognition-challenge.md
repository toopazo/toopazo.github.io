---
title: Reconocimiento facial
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

El sueño de este proyecto es el siguiente. Imaginemos que ponemos una cámara en pleno Paseo Ahumada, y que grabamos los rostros de todos los que pasan por ahí. Detectamos a cada persona que transita, obtenemos la representación de sus rostros en un [vector de espacio latente](https://hackernoon.com/lang/es/espacio-latente-visualizacion-aprendizaje-profundo-bits-2-bd09a46920df) y los guardamos en una base de datos de gran tamaño. Es decir, muchos millones de secuencias, donde cada secuencia contiene el rostro de una persona a medida que se mueve en la escena. Un par de días después y en el otro extremo de la ciudad, activamos el sistema en un Mall concurrido. Allí realizamos la misma operación comparando a los transeúntes. 


<center>
<figure>
  <img src="/assets/images/face_recognition_dream.png" style="width:100%" alt="alt_text" /> 
</figure> 
</center>

## Los ingredientes tecnológicos

En este proyecto he ocupado un video 

