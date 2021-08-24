---
title: Armado de un vehículo aéreo no tripulado open-source
date: 2021-08-19
categories: 
- postscyt
layout: archive
collection: postscyt
---

El presente escrito es la versión practica de [Alternativas Dronecode](toopazo.github.io/postscyt/Alternativas-Dronecode), un articulo anterior acerca de las alternativas open-source disponibles para volar un vehículo aéreo no tripulado. En particular explicaremos como armar un kit [S500 v2](https://shop.holybro.com/s500-v2-kitmotor2216-880kv-propeller1045_p1153.html) de la empresa Holybro. Aquí detallaremos los componentes principales de este tipo de productos y como verificar su correcto armado.

<figure>
 <center>
  <img src="https://toopazo.github.io/images/holybro_S500_v2.jpg" style="width:25%" alt="alt_text" title="image_tooltip" /> 
  <img src="https://toopazo.github.io/images/holybro_S500_v2_juan2.jpeg" style="width:30%" alt="alt_text" title="image_tooltip" /> 
  <img src="https://toopazo.github.io/images/holybro_S500_v2_juan3.jpeg" style="width:30%" alt="alt_text" title="image_tooltip" />
  <figcaption>Kit S500 de Holybro, armado por Juan Cespedes, Agosto 2021 </figcaption>
 </center>
</figure> 


|El siguiente es un artículo escrito con fines de divulgación y hecho con el mayor cuidado. Sin embargo no puedo garantizar que todo lo expuesto sea 100% correcto. Si encuentra algún error por favor háganmelo saber.|
|--|
|Historial de actualizaciones <ul><li>20-08-2021</li><li>21-08-2021</li></ul>|


## 1) Componentes principales de un multirotor

Para los amantes del concepto open-source existen en la actualidad una seria de Kit de vehículos multirotor que pueden ser adquiridos y armados por alguien con relativamente poca experiencia. La idea de este escrito es reducir esa barrera aún más. Desde el punto de vista constructivo, es posible separar un multirotor en 3 componentes: un esqueleto, actuadores y un computador de vuelo. 

El tamaño, peso, geometría y numero de brazos del esqueleto viene definido por el tipo de misión que queremos lograr con el vehículo. ¿Necesitamos portar un sensor de gases en una industria?, entonces lo más probable es que necesitemos una estructura de tamaño medio, robusta y de peso medio alto. ¿Queremos transmitir vídeo en vivo?, es probable entonces que una estructura pequeña y liviana nos baste. ¿Necesitamos cargar cajas de un lado a otro?, es probable que necesitemos más hélices de lo usual y una estructura más pesada. 

Una simple búsqueda en la web por "frame kit multirotor drone" nos arroja una serie de alternativas para distintos propósitos: fotografía, carrera de Drones, filmación, carga, etc. 

<figure>
 <center>
  <img src="https://toopazo.github.io/images/uav_frame_kit.png" style="width:80%" alt="alt_text" title="image_tooltip"/> 
  <figcaption>Ejemplo de kits disponibles para armado de Drones, precio en USD</figcaption>
 </center>
</figure> 

Los actuadores son la suma de ESC + Motores + Hélices. Los controladores de motor son conocidos en inglés como [Electronic Speed Controllers (ESC)](https://en.wikipedia.org/wiki/Electronic_speed_control) y son los encargados de regular su velocidad de giro. Para multirotores casi siempre se utilizan [motores de corriente continua y sin escobilla (burshless DC motors)](https://en.wikipedia.org/wiki/Brushless_DC_electric_motor). Finalmente las hélices son las laminas diseñadas para rotar y desplazar el aire circundante de manera de generar empuje. 

<figure>
 <center>
  <img src="https://toopazo.github.io/images/multirotor_actuator_v1.jpg" style="width:70%" alt="alt_text" title="image_tooltip" />
  <figcaption>Ejemplo de ESC + motor + hélice usados comúnmente </figcaption>
 </center>
</figure> 

Finalmente tenemos el computador de vuelo, el cual es el encargado de estimar la posición y orientación del vehículo usando sensores inerciales, GPS y otros. Es además el encargado de coordinar los sistemas de radio, control remoto y demás electrónica. El computador en si mismo contiene los sensores más todo el software de vuelo, el 

<figure>
 <center>
  <img src="https://toopazo.github.io/images/pixhawk4_wiring_overview.png" style="width:70%" alt="alt_text" title="image_tooltip" />
  <figcaption>Ejemplo de computador de vuelo, fuente: [docs.px4.io](https://docs.px4.io/master/en/assembly/quick_start_pixhawk4.html) </figcaption>
 </center>
</figure> 

## 2) Ensamble mecánico

Este paso depende en un 100% del modelo que se haya comprado. Cada fabricante cuenta (o debería contar) con un manual de instrucción en que detalle este proceso. En nuestro caso Holybro provee de [este enlace al manual](http://www.holybro.com/manual/S500_V2_Kit_AssemblyManual.pdf). Con casi el mismo contenido Px4 también provee instrucciones de armado en [este otro enlace](https://docs.px4.io/master/en/frames_multicopter/holybro_s500_v2_pixhawk4.html).

## 3) Verificación

Luego del armado mecánico es imprescindible realizar una verificación tanto de actuadores, conexiones eléctricas y del computador de vuelo.

- Verificar que la señal de control de cada motor este correctamente conectada con el computador de vuelo, en posición y polaridad.

<figure>
 <center>
  <img src="https://toopazo.github.io/images/holybro_S500_v2_armado_1.png" style="width:70%" alt="alt_text" title="image_tooltip" />
  <figcaption>Orden y polaridad de conexión entre computador de vuelo y motores</figcaption>
 </center>
</figure> 

- Verificar que la ubicación de cada motor corresponda con el tipo de "airframe" sea la indicada, en en nuestro caso "Quadrotor x" (SYS_AUTOSTART = 4015).

<figure>
 <center>
  <img src="https://toopazo.github.io/images/holybro_quadrotor_x.png" style="width:70%" alt="alt_text" title="image_tooltip" />
  <figcaption>Airframe [Quadrotor x (SYS_AUTOSTART = 4015)](https://docs.px4.io/master/en/airframes/airframe_reference.html#quadrotor-x)</figcaption>
 </center>
</figure> 
