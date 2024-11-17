---
title: Reconocimiento kinestécico (HAR)
layout: post
---
¿Fue penal?.

Dentro de la amplia gama de problemas que la visión computacional busca resolver, esta el reconocimiento de acciones dentro de un video (secuencia de imágenes). Reconocer cuando alguien se cae, cuando alguien esta saludando con su mano, cuando dos personas están discutiendo, cuando alguien toca el piano o canta. El nombre en inglés de este problema de visión computacional es Human Action Recognition (HAR), que podríamos traducir como reconocimiento de acciones humanas. Aunque a mi gusto una mejor traducción sería reconocimiento kinestécico.

La importancia de incluir la variable temporal dentro del problema es evidente para todo conocedor del futbol: un penal no se puede decidir en base a una foto, debemos considerar la secuencia. Entender si hubo carga previa, considerar la intención y trayectoria del balón, juzgar si había riesgo de gol, etc. El contacto solo no permite decidir falta. Eso mismo sucede en muchos otros campos en que el problema no se puede resolver analizando imagen a imagen de forma aislada. 

<center>
<figure>
  <img src="/assets/images/falta_penal.jpg" style="width:100%" alt="alt_text" /> 
</figure> 
</center>


En el problema clásico el conjunto de acciones es finito y conocido a-priori. Se plantea normalmente como un problema de clasificación donde debemos decir al finalizar su análisis a que categoría pertenece. El problema de detección de acciones dentro de un video más largo o dentro de porción de la imagen misma, es raramente abordado ya que es mucho más caro computacionalmente hablando (sobretodo al momento de entrenar). En este sentido uno podría esperar lo mismo que en imágenes estáticas, donde primero se resolvió la clasificación entre perros y gatos en imágenes donde aparecían estos animales en primer plano y sin elementos adicionales. Para luego con el tiempo mejorar la clasificación de imágenes más complejas y la detección dentro de una porción de la imagen y en simultáneo con otras clases.

En la página web [paperswithcode.com](https://paperswithcode.com/task/action-recognition-in-videos) dedicada a este problema existe una serie de datasets usados para evaluar el avance de los modelos de IA propuestos. [Something-Something V2](https://www.qualcomm.com/developer/software/something-something-v-2-dataset) es un ejemplo de esos dataset, consta de 220,847 videos repartidos en 174 clases. Cada video pertenece a una sola clase y dura entre 2 a 6 segundos. 

{% include youtube.html id="gO91EObaZzM" %}
<!-- {% include embed.html url="https://www.youtube.com/watch?v=gO91EObaZzM" %} -->
<!-- [https://youtu.be/gO91EObaZzM](https://youtu.be/gO91EObaZzM) -->
<!-- <iframe id="player" src="https://www.youtube.com/watch?v=gO91EObaZzM" frameborder="0"></iframe> -->


Si comparamos un video de Sth-Sth V2 de la clase 'Cerrar algo' versus un video de una falta penal en fútbol vemos una diferencia notable en cuanto a complejidad: la cantidad de elementos móviles, la interpretación de que sucede en la acción, la cantidad de conocimiento previo necesario para extender un juicio, etc. Existen por lo visto muchos niveles de complejidad en los que avanzar respecto a reconocimiento de acciones. Pero obviamente se debe comenzar de lo más simple a lo complejo. partir reconociendo que es un 'juego de futbol', luego que es una 'disputa de balón' y solo después tratar de entrenar un **arbitro IA** que decida si fue falta penal o no. 

<!-- ## Estrategias de resolución

Actualmente (2024) existen  -->