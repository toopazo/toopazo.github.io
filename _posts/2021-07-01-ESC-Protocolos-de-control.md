---
title: Protocolos de control ESC
date: 2021-07-01
categories: 
- postscyt
layout: archive
collection: postscyt
---

El presente escrito es un reporte acerca de los diferentes protocolos de control usados entre un ESC y un computador de vuelo. Todo esto en el contexto de vehículos autónomos no tripulados. Todos estos protocolos cumplen la misma función, indican cuan rapido o lento deben girar los motores. Pero una serie de caracteristicas tecnicas hacen que unos protocolos sean mejores que otros para ciertos vehículos. 

<figure>
  <img src="https://toopazo.github.io/images/uavcan_esc.png" style="width:45%" alt="alt_text" /> 
  <img src="https://toopazo.github.io/images/castle_phoenix_edge_hv_80.jpg" style="width:25%" alt="alt_text" /> 
  <img src="https://toopazo.github.io/images/kde-uas85uvc.jpg" style="width:25%" alt="alt_text" />
  <figcaption> Diferentes modelos de ESC usados en multirotores: Orel y Kotleta (UAVCAN), Phoenix Edge y KDE-UAS </figcaption>
</figure> 

|El siguiente es un artículo escrito con fines de divulgación y hecho con el mayor cuidado. Sin embargo no puedo garantizar que todo lo expuesto sea 100% correcto. Si encuentra algún error por favor háganmelo saber.|
|--|
|Historial de actualizaciones <ul><li>21-10-2021</li></ul>|



## 1) Señal analógica PWM


El modo clásico para controlar la velocidad de giro de un motor DC es usando una señal PWM analógica. Esta, por lo general varia de 0V a 5V con una frequencia de 20ms (50 Hz) y es enviada al ESC el cual se encarga de hacer girar el motor DC. La misma señal es usada en servo-motores pero es interpretada de distinta manera, en este caso no se necesita de un ESC si no que la señal es enviada directamente al dispositivo. Esta forma de onda fue durante años la manera estándar de control disponible. El ancho de cada pulso indica al ESC si el motor debía estar: 
- detenido, pero en espera (900 us)
- girando a mínima velocidad (1000 us) 
- girando a máxima velocidad (2000 us)

La siguiente imagen del sitio web [howtomechatronics.com](https://howtomechatronics.com/wp-content/uploads/2019/02/Brushless-motor-control-signal-50hz-PWM-same-as-servo-motor.png) ilustra la forma de onda. 

<figure>
  <img src="https://toopazo.github.io/images/esc_pwm_howtomechatronics.png" style="width:90%" alt="alt_text" />
  <figcaption> Forma de onda de la señal de control PWM con la que un ESC controla la velocidad de un motor </figcaption>
</figure>

Debido a la aparición de vehículos cada vez más pequeños y ágiles, con el tiempo fue necesario incrementar la frecuencia con la que el ESC realizaba cambios de velocidad en el motor. Fue así como surgieron señales PWM de 100 Hz, 200 Hz hasta llegar a los 400 Hz que es común encontrar hoy en día (2021) en muchos ESC comerciales. 

<figure>
  <img src="https://toopazo.github.io/images/esc_pwm_50Hz_400Hz.jpg" style="width:90%" alt="alt_text" />
  <figcaption> Forma de onda de la señal de control PWM a 50 Hz y a 400 Hz </figcaption>
</figure>

|Esta frecuencia NO debe confundirse con la frecuencia eléctrica a la que operan los motores (del orden de 10 kHz), si no que es la frecuencia con la que se actualiza la velocidad del motor.|
|--|

Uno de los aspectos complicados de este tipo de señal es que son susceptibles al ruido y a variaciones temporales del pulso debido a tipo y largo de cables usados. Para subsanar esto muchas veces se hacía (ya hace) necesario hacer una calibración individual para cada ESC. 

Pero además de problemas de calibración, este tipo de señal 



## 3) Dshot y sus variantes

## 4) Protocolo UAVCAN

El protocolo UAVCAN es el estandar promovido por Droncode Foundation, su pagina web y documentación es https://uavcan.org/. Lamentablemente los ESC que incorporan esta tecnología son de muy bajo amperaje y no son compatibles con el motor que yo tenía dispoible para esta seria de pruebas. Queda entonces para más adelante. 

