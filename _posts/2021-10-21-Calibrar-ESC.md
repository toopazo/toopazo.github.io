---
title: Protocolos de comunicación con un ESC
date: 2021-10-21
categories: 
- postscyt
layout: archive
collection: postscyt
---

El presente escrito es un reporte acerca de mi experiencia con los diferentes protocolos de comunicación entre un ESC y un computador de vuelo. Todo esto en el contexto de vehículos autónomos no tripulados.

<figure>
  <img src="https://toopazo.github.io/images/uavcan_esc.png" style="width:45%" alt="alt_text" /> 
  <img src="https://toopazo.github.io/images/castle_phoenix_edge_hv_80.jpg" style="width:25%" alt="alt_text" /> 
  <img src="https://toopazo.github.io/images/kde-uas85uvc.jpg" style="width:25%" alt="alt_text" />
  <figcaption> Diferentes modelos de ESC usados en multirotores: Orel y Kotleta (UAVCAN), Phoenix Edge y KDE-UAS </figcaption>
</figure> 

|El siguiente es un artículo escrito con fines de divulgación y hecho con el mayor cuidado. Sin embargo no puedo garantizar que todo lo expuesto sea 100% correcto. Si encuentra algún error por favor háganmelo saber.|
|--|
|Historial de actualizaciones <ul><li>21-10-2021</li></ul>|

## 1) Tipos de protocolo

## 2) Señal analógica


Uno de los aspectos complicados de este tipo de señal es que son susceptibles al ruido y a variaciones de voltaje debido a tipo y largo de cables usados. Para subsanar esto muchas veces es necesario hacer una calibración del ESC. Esto permite que este dispositivo conozca que valor de la señal debe estar asociado a tres puntos criticos

- motor en espera
- motor a mínima velocidad
- motor a máxima velocidad

En el caso de el modelo KDE-UAS85UVC podemos usar el software [Device Manager](https://www.kdedirect.com/products/kde-dms)

## 3) Dshot y sus variantes

## 4) Protocolo UAVCAN

El protocolo UAVCAN es el estandar promovido por Droncode Foundation, su pagina web y documentación es https://uavcan.org/. Lamentablemente los ESC que incorporan esta tecnología son de muy bajo amperaje y no son compatibles con el motor que yo tenía dispoible para esta seria de pruebas. Queda entonces para más adelante. 

