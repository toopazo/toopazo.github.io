---
title: Protocolos de control ESC
layout: post
mathjax: true
categories: media
---

El presente escrito es un reporte acerca de los diferentes protocolos de control usados entre un ESC y un computador de vuelo. Todo esto en el contexto de vehículos autónomos no tripulados. Todos estos protocolos cumplen la misma función, indican cuan rapido o lento deben girar los motores. Pero una serie de caracteristicas tecnicas hacen que unos protocolos sean mejores que otros para ciertos vehículos. 

<center>
<figure>
  <img src="https://raw.githubusercontent.com/toopazo/imgodg/main/uavcan_esc.png" style="width:45%" alt="alt_text" /> 
  <img src="https://raw.githubusercontent.com/toopazo/imgodg/main/castle_phoenix_edge_hv_80.jpg" style="width:25%" alt="alt_text" /> 
  <img src="https://raw.githubusercontent.com/toopazo/imgodg/main/kde-uas85uvc.jpg" style="width:25%" alt="alt_text" />
  <figcaption> Diferentes modelos de ESC usados en multirotores: Orel y Kotleta (UAVCAN), Phoenix Edge y KDE-UAS </figcaption>
</figure> 
</center>


En general, para poder controlar un multirotor pequeño y ágil (e.g. Drone de carrera, menor a 5 kg) necesitamos cambiar con más frecuencia la velocidad a la que giran las hélices que en el caso uno más grande (más de 30 cm de largo y 5 kg de peso). Por el contrario si queremos controlar un avión de alas fijas -a menos que sea muy pequeño (menor a 10 cm y 1 kg)- una tasa de 50 Hz nos basta y sobra. 

## 1) Señales analógicas (método clásico)

El modo clásico para controlar la velocidad de giro de un motor DC es usando una señal PWM analógica. Esta, por lo general varia de 0V a 5V con una frecuencia de 20 ms (50 Hz) y es enviada al ESC el cual se encarga de hacer girar el motor DC. La misma señal es usada en servo-motores pero es interpretada de distinta manera, en este caso no se necesita de un ESC si no que la señal es enviada directamente al dispositivo. Esta forma de onda fue durante años la manera estándar de control disponible. El ancho de cada pulso indica al ESC si el motor debía estar: 
- detenido, pero en espera (900 us)
- girando a mínima velocidad (1000 us) 
- girando a máxima velocidad (2000 us)

La siguiente imagen del sitio web [howtomechatronics.com](https://howtomechatronics.com/wp-content/uploads/2019/02/Brushless-motor-control-signal-50hz-PWM-same-as-servo-motor.png) ilustra la forma de onda. 

<center>
<figure>
  <img src="https://raw.githubusercontent.com/toopazo/imgodg/main/esc_pwm_howtomechatronics.png" style="width:90%" alt="alt_text" />
  <figcaption> Forma de onda de la señal de control PWM con la que un ESC controla la velocidad de un motor </figcaption>
</figure>
</center>

Debido a la aparición de vehículos cada vez más pequeños y ágiles, con el tiempo fue necesario incrementar la frecuencia con la que el ESC realizaba cambios de velocidad en el motor. Fue así como surgieron señales PWM de 100 Hz, 200 Hz hasta llegar a los 400 Hz que es común encontrar hoy en día (2021) en muchos ESC comerciales. 

<center>
<figure>
  <img src="https://raw.githubusercontent.com/toopazo/imgodg/main/esc_pwm_50Hz_400Hz.jpg" style="width:90%" alt="alt_text" />
  <figcaption> Forma de onda de la señal de control PWM a 50 Hz y a 400 Hz </figcaption>
</figure>
</center>

|Esta frecuencia NO debe confundirse con la frecuencia eléctrica a la que operan los motores (del orden de 10 kHz), si no que es la frecuencia con la que se actualiza la velocidad del motor.|
|--|


Si quisiéramos llegar a tasas de 1 kHz o más necesitaremos reducir el tamaño del pulso, pasando de un rango [1 ms - 2 ms] a rangos de [125 us - 250 us] y menores. Esto se logra con los protoclos: OneShot125, OneShot42 y similares. Que son iguales su principio de funcionamiento a la señal PWM tradicional, pero que operan con anchos de pulso menores. 

Una clara desventaja todas las señales anteriores es que son susceptibles al ruido y a variaciones de fase en el pulso, por ejemplo debido al tipo y largo de cables usados. Para subsanar esto es que muchas veces se hace necesario hacer una calibración individual para cada ESC. 

## 2) Señales digitales (métodos nuevos)

Evidentemente la mejor solución para lograr tasas de actualización del orden de 10 kHz y reducir errores, es pasar de una señal análoga a una digital. Esto se puede hacer usando los mismos protocolos disponibles para microcontroladores.

- UART
- I2C
- SPI
- CAN
- Ethernet


### UAVCAN

El protocolo UAVCAN es el estandar promovido por Droncode Foundation, su pagina web y documentación es https://uavcan.org/. Lamentablemente los ESC que incorporan esta tecnología son de muy bajo amperaje y no son compatibles con el motor que yo tenía dispoible para esta seria de pruebas. Queda entonces para más adelante. 

|El siguiente es un artículo escrito con fines de divulgación y hecho con el mayor cuidado. Sin embargo no puedo garantizar que todo lo expuesto sea 100% correcto. Si encuentra algún error por favor háganmelo saber.|
|--|
|Historial de actualizaciones <ul><li>21-10-2021</li></ul>|
