---
title: Armado de un vehículo aéreo no tripulado open-source
date: 2021-08-19
categories: 
- postscyt
layout: archive
collection: postscyt
---

El presente escrito es la versión practica de [Alternativas Dronecode](toopazo.github.io/postscyt/Alternativas-Dronecode), un articulo anterior acerca de las alternativas open-source disponibles para volar un vehículo aéreo no tripulado. En particular explicaremos los componentes principales de todo multirotor, y nuestra experiencia al armar un kit [S500 v2](https://shop.holybro.com/s500-v2-kitmotor2216-880kv-propeller1045_p1153.html) de la empresa Holybro. Más que el armado en si mismo trataremos de contar la estructura general de todo vehículo así como los pasos que seguimos para verificar su correcto armado y vuelo.

<figure>
  <img src="https://toopazo.github.io/images/holybro_S500_v2.jpg" style="width:25%" alt="alt_text" title="image_tooltip" /> 
  <img src="https://toopazo.github.io/images/holybro_S500_v2_juan2.jpeg" style="width:30%" alt="alt_text" title="image_tooltip" /> 
  <img src="https://toopazo.github.io/images/holybro_S500_v2_juan3.jpeg" style="width:30%" alt="alt_text" title="image_tooltip" />
  <figcaption> Kit S500 de Holybro, armado por Juan Cespedes, Agosto 2021 </figcaption>
</figure> 


|El siguiente es un artículo escrito con fines de divulgación y hecho con el mayor cuidado. Sin embargo no puedo garantizar que todo lo expuesto sea 100% correcto. Si encuentra algún error por favor háganmelo saber.|
|--|
|Historial de actualizaciones <ul><li>20-08-2021</li><li>21-08-2021</li></ul>|


## 1) Componentes principales de un multirotor

Para los amantes del concepto open-source existen en la actualidad una seria de Kit de vehículos multirotor que pueden ser adquiridos y armados por alguien con relativamente poca experiencia. La idea de este escrito es reducir esa barrera aún más. Desde el punto de vista constructivo, es posible separar un multirotor en 3 componentes: un esqueleto, actuadores y un computador de vuelo. 

El tamaño, peso, geometría y numero de brazos del esqueleto viene definido por el tipo de misión que queremos lograr con el vehículo. ¿Necesitamos portar un sensor de gases en una industria?, entonces lo más probable es que necesitemos una estructura de tamaño medio, robusta y de peso medio alto. ¿Queremos transmitir vídeo en vivo?, es probable entonces que una estructura pequeña y liviana nos baste. ¿Necesitamos cargar cajas de un lado a otro?, es probable que necesitemos más hélices de lo usual y una estructura más pesada. 

Una simple búsqueda en la web por "frame kit multirotor drone" nos arroja una serie de alternativas para distintos propósitos: fotografía, carrera de Drones, filmación, carga, etc. 

<figure>
  <img src="https://toopazo.github.io/images/uav_frame_kit.png" style="width:80%" alt="alt_text" title="image_tooltip"/> 
  <figcaption> Ejemplo de kits disponibles para armado de Drones, precio en USD </figcaption>
</figure> 

Los actuadores son la suma de ESC + Motores + Hélices. Los controladores de motor son conocidos en inglés como [Electronic Speed Controllers (ESC)](https://en.wikipedia.org/wiki/Electronic_speed_control) y son los encargados de regular su velocidad de giro. Para multirotores casi siempre se utilizan [motores de corriente continua y sin escobilla (burshless DC motors)](https://en.wikipedia.org/wiki/Brushless_DC_electric_motor). Finalmente las hélices son las laminas diseñadas para rotar y desplazar el aire circundante de manera de generar empuje. 

<figure>
  <img src="https://toopazo.github.io/images/multirotor_actuator_v1.jpg" style="width:70%" alt="alt_text" title="image_tooltip" />
  <figcaption> Ejemplo de ESC + motor + hélice usados comúnmente </figcaption>
</figure> 

Finalmente tenemos el computador de vuelo, el cual es el encargado de estimar la posición y orientación del vehículo usando sensores inerciales, GPS y otros. Es además el encargado de coordinar los sistemas de radio, control remoto y demás electrónica. El computador en si mismo contiene los sensores más todo el software de vuelo.

<figure>
  <img src="https://toopazo.github.io/images/pixhawk4_wiring_overview.png" style="width:70%" alt="alt_text" title="image_tooltip" />
  <figcaption> Ejemplo de computador de vuelo, fuente: <a href="https://docs.px4.io/master/en/assembly/quick_start_pixhawk4.html">docs.px4.io</a> </figcaption>
</figure> 

## 2) Armado mecánico y configuración inicial

Este paso depende en un 100% del modelo que se haya comprado. Cada fabricante cuenta (o debería contar) con un manual de instrucción en que detalle este proceso. En nuestro caso Holybro provee de [este enlace al manual](http://www.holybro.com/manual/S500_V2_Kit_AssemblyManual.pdf). Con casi el mismo contenido Px4 también provee instrucciones de armado en [este otro enlace](https://docs.px4.io/master/en/frames_multicopter/holybro_s500_v2_pixhawk4.html).

Existen además múltiples tutoriales y una muy buena serie de videos en Youtube acerca del armado del kit S500. 
- [Pixhawk 4 Flight Controller ULTIMATE Tutorial | Pixhawk 4 + S500 Drone Build Tutorial | Part 1](https://www.youtube.com/watch?v=wsrRNqihjE4)
- [S500 Quadcopter Frame Guided Assembly | Pixhawk 4 + S500 Drone Build Tutorial | Part 2](https://www.youtube.com/watch?v=dDOQI5NoNP0)
- [Pixhawk 4 Receiver and Transmitter Configuration | Pixhawk 4 + S500 Drone Build Tutorial | Part 3](https://www.youtube.com/watch?v=w40P1CQQ9Ls)
- [Pixhawk 4 Setup and Calibration with QGroundControl | Pixhawk 4 + S500 Drone Build Tutorial | Part 4](https://www.youtube.com/watch?v=BNzeVGD8IZI)

Más abajo se puede ver una sucinta serie de fotos de nuestra experiencia en el armado

<figure>
  <img src="https://toopazo.github.io/images/holybro_S500_v2_juan4.jpeg" style="width:30%" alt="alt_text" title="image_tooltip" /> 
  <img src="https://toopazo.github.io/images/holybro_S500_v2_juan7.jpeg" style="width:30%" alt="alt_text" title="image_tooltip" /> 
  <img src="https://toopazo.github.io/images/holybro_S500_v2_juan9.jpeg" style="width:30%" alt="alt_text" title="image_tooltip" />
  <figcaption> Kit S500 de Holybro, armado por Juan Cespedes, Agosto 2021 (parte 1) </figcaption>
</figure> 
<figure>
  <img src="https://toopazo.github.io/images/holybro_S500_v2_juan1.jpeg" style="width:30%" alt="alt_text" title="image_tooltip" /> 
  <img src="https://toopazo.github.io/images/holybro_S500_v2_juan2.jpeg" style="width:30%" alt="alt_text" title="image_tooltip" /> 
  <img src="https://toopazo.github.io/images/holybro_S500_v2_juan3.jpeg" style="width:30%" alt="alt_text" title="image_tooltip" />
  <figcaption> Kit S500 de Holybro, armado por Juan Cespedes, Agosto 2021 (parte 2) </figcaption>
</figure> 


## 3) Verificación y evaluación

Luego del armado mecánico es imprescindible realizar una verificación tanto de actuadores, conexiones eléctricas y del computador de vuelo. Ahora detallamos los pasos seguidos para corregir los errores que encontramos en nuestra experiencia.

Paso 1:  Verificar que la señal de control de cada motor este correctamente conectada con el computador de vuelo, en posición y polaridad.

<figure>
  <img src="https://toopazo.github.io/images/holybro_S500_v2_armado_1.png" style="width:60%" alt="alt_text" title="image_tooltip" />
  <figcaption> Orden y polaridad de conexión entre computador de vuelo y motores </figcaption>
</figure> 

Paso 2: Verificar que la ubicación de cada motor en el esqueleto corresponda con el tipo de "airframe" indicado, en nuestro caso "Quadrotor x" (SYS_AUTOSTART = 4015).

<figure>
  <img src="https://toopazo.github.io/images/holybro_quadrotor_x.png" style="width:30%" alt="alt_text" title="image_tooltip" />
  <figcaption>Airframe <a href="https://docs.px4.io/master/en/airframes/airframe_reference.html#quadrotor-x">Quadrotor x (SYS_AUTOSTART = 4015)</a></figcaption>
</figure>

Paso 3: Verificar que el sentido de giro de los motores coincidan con el tipo de "airframe" indicado, en nuestro caso "Quadrotor x" (SYS_AUTOSTART = 4015).

Tal como lo muestra la imagen del paso anterior, los motores deben girar en una determinada dirección. En caso de que el motor no gire en el sentido correcto la solución más sencilla es intercambiar dos de los tres cables que van desde el ESC al motor. 

<figure>
  <img src="https://toopazo.github.io/images/holybro_S500_spin_swap.jpeg" style="width:70%" alt="alt_text" title="image_tooltip" />
  <figcaption> Intercambio de cables necesarios entre el ESC y el motor para cambiar el sentido de giro </figcaption>
</figure> 

Paso 4: Verificar que el sentido de giro de las hélices coincidan con el tipo de "airframe" indicado, en nuestro caso "Quadrotor x" (SYS_AUTOSTART = 4015).

Las hélices empleadas en estos kits y todas las comercializadas en el segmento multirotor vienen en dos tipos: horarias y anti-horarias. Cada hélice está diseñada para girar en un solo sentido. Si se gira en sentido contrario pasan dos cosas: 1) trabajará de manera ineficiente y produciendo una fuerza en sentido contrario (hacia abajo) y 2) si es de tipo "rosca" terminara desprendiéndose del eje del motor. 

<figure>
  <img src="https://toopazo.github.io/images/helices_rotacion.png" style="width:70%" style="text-align: center;" alt="alt_text" title="image_tooltip" />
  <figcaption> Hélice de rotación ani-horario (tapa negra) y horaria (tapa plateada) y su vista lateral </figcaption>
</figure> 

Existen además diferentes tipos de mecanismos de sujeción entre la hélice y el motor. Las hélices más avanzadas tienden a tener adaptadores









