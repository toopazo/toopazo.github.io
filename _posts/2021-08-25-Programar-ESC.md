---
title: Como programar un ESC para obtener datos en linea
date: 2021-08-25
categories: 
- postscyt
layout: archive
collection: postscyt
---

El presente escrito es un reporte acerca de mi experiencia programando un ESC para obtener datos en linea. Un ESC es un controlador para motores sin escobilla que aumenta o disminuye la potencia inyectada a éste para así regular su velocidad de giro. Obtener datos instantáneos como voltaje, corriente y velocidad del motor son de vital importancia para estimar el consumo de cada rotor y pueden ser usado incluso como parte del controlador de vuelo. Todo el software usado en este reporte se encuetra disponible en https://github.com/toopazo/live_esc

<figure>
  <img src="https://toopazo.github.io/images/uavcan_esc.png" style="width:45%" alt="alt_text" /> 
  <img src="https://toopazo.github.io/images/castle_phoenix_edge_hv_80.jpg" style="width:25%" alt="alt_text" /> 
  <img src="https://toopazo.github.io/images/kde-uas85uvc.jpg" style="width:25%" alt="alt_text" />
  <figcaption> Diferentes modelos de ESC usados en multirotores: Orel y Kotleta (UAVCAN), Phoenix Edge y KDE-UAS </figcaption>
</figure> 

|El siguiente es un artículo escrito con fines de divulgación y hecho con el mayor cuidado. Sin embargo no puedo garantizar que todo lo expuesto sea 100% correcto. Si encuentra algún error por favor háganmelo saber.|
|--|
|Historial de actualizaciones <ul><li>25-08-2021</li><li>26-08-2021</li><li>27-08-2021</li></ul>|

## 1) Telemetría usando el modelo [Phoenix Edge HV](https://www.castlecreations.com/en/phoenix-edge-hv)

La lista de materiales para esta prueba fue:
- [Castle Creation: Phoenix Edge HV 80](https://www.castlecreations.com/en/phoenix-edge-hv/phoenix-edge-hv80-esc-010-0105-00)
- [Castle Creation: USB programmer](https://www.castlecreations.com/en/castle-link-v3-usb-programming-kit-011-0119-00)  
- [Castle Creation: Serial Link](https://www.castlecreations.com/en/serial-link-010-0121-00)
- [HolyBro: Pixhawk 4](http://www.holybro.com/product/pixhawk-4/)
- [KDE Direct: KDE 6213XF-185 motor](https://www.kdedirect.com/products/kde6213xf-185)
- [SparkFun: Serial (ttl) to USB](https://www.sparkfun.com/products/15096)
- Laptop con python 3
- Baterías de 4S2P  (dos baterías 4S en serie => 29.6 V)
- Un regulador de voltaje de 5V para energizar la linea de alimentación del Pixahwk

El ESC exacto en ser probado corresponde al modelo [Phoenix Edge HV 80 AMP ESC](https://www.castlecreations.com/en/commercial/phoenix-edge-hv80-esc-010-0105-00), el cual es programado usando el adaptador [Castle Link v3 USB programming kit](https://www.castlecreations.com/en/castle-link-v3-usb-programming-kit-011-0119-00), este corresponde a un simple y ordinario conversor serial FTDI, pero que venden por unos 20 USD, una triquiñuela para sacar algo de dinero extra. Pero bueno, eso es tema para otro día. 

<figure>
  <img src="https://toopazo.github.io/images/castle_link_programmer.jpeg" style="width:90%" alt="alt_text" />
  <figcaption> Conexión entre el programador USB (Castle Link v3 USB programming kit) y el ESC </figcaption>
</figure> 

Una vez teniendo conectado el ESC al programador USB debemos descargar el programa [Castle Link Software](https://home.castlecreations.com/download-castle-link/). Esta disponible para Windows solamente, pero podemos instalar una maquina virtual si es que estamos en Linux.

<figure>
  <img src="https://toopazo.github.io/images/castle_enable_live_link.png" style="width:90%" alt="alt_text" />
  <figcaption> Pasos 1, 2 y 3 para habilitar el "Castle Link Live" en los ESC de Castle Creations </figcaption>
</figure> 

Una vez habilitada la comunicación por protocolo [Castle Link Live](https://www.castlecreations.com/castle-link-live) debemos hacernos de otro adaptador, esta vez llamado [Castle Serial Link](https://www.castlecreations.com/en/serial-link-010-0121-00). Este si es un poco más interesante en el sentido que nos permite establecer un bus de datos al cual conectar los múltiples ESC que tengamos en un multirotor. La conexión hacia el computador se puede hacer con un USB Ftdi como se ve en la figura de más abajo. El voltaje en el BUS del Castle Serial Link debe ser de +5V en caso de que el ESC lo requiera, que es justamente lo que sucede con el modelo Phoenix Edge HV.

<figure>
  <img src="https://toopazo.github.io/images/castle_usbFtdi_serialLink.png" style="width:90%" alt="alt_text" />
  <figcaption> Conexión entre conversor USB Ftdi y Castle Serial Link </figcaption>
</figure> 

Luego de esta configuración inicial podemos empezar a desarrollar un programa para comunicarnos con los ESC y recopilar datos. Sin embargo en vez de reinventar la rueda ocuparemos un código en Python alojado en https://github.com/math2peters/CastleSerialLink

```
git clone https://github.com/math2peters/CastleSerialLink.git
cd CastleSerialLink
```

Luego debemos simplemente usar el código de ejemplo y modificarlo para nuestros propósitos. En nuestro caso telemetría en tiempo real de las variables del rotor. El código se encuentra dentro de la carpeta ```phoenix_edge_hv_80/CastleSerialLink/```. El archivo ```read_esc_data.py``` se conecta con el ESC (a través del conversor serial FTDI y el Serial Link) y guarda los datos en un archivo .pkl. Este archivo es luego leido por ```plot_esc_data.py``` el cual produce una imagen como la observada más abajo.

<figure>
  <img src="https://toopazo.github.io/images/read_esc_data.py" style="width:90%" alt="alt_text" />
  <figcaption> Telemtria en vivo obtenida a través del Serial Link </figcaption>
</figure> 

The code works well, but I have not being able to get the ```TTL Serial (with PPM Input)``` mode to work.
Connecting the Pixhawk's port```IO PWM out``` for motor 1 (it could have been any) into pin ```D``` of the Serial Link produces no response from the ESC. There seems to be a problem -on the Castle Creation side- intepreting the throttle signal.       



## 2) Telemetría usando el modelo [KDE-UAS85UVC](https://www.kdedirect.com/collections/uas-multi-rotor-electronics/products/kde-uas85uvc) 
 
La lista de materiales para esta prueba fue:
- [KDE Direct: KDE-UAS85UVC](https://www.kdedirect.com/collections/uas-multi-rotor-electronics/products/kde-uas85uvc)
- [KDE Direct: Device Manager (software)](https://www.kdedirect.com/collections/uas-multi-rotor-electronics/products/kde-dms?page=specs) 
- [HolyBro: Pixhawk 4](http://www.holybro.com/product/pixhawk-4/)
- [KDE Direct: KDE 6213XF-185 motor](https://www.kdedirect.com/products/kde6213xf-185)
- [Inno-Maker: USB CAN Module](https://www.inno-maker.com/product/usb-can/)
- Laptop con python 3
- Baterías de 4S2P  (dos baterías 4S en serie => 29.6 V)
- Un regulador de voltaje de 5V para energizar la linea de alimentación del Pixahwk

Los ESC de la empresa KDE usan el protocolo CAN para transmitir datos con una serie de comandos llamados [KDECAN](https://cdn.shopify.com/s/files/1/0496/8205/files/KDECAN_Bus_Protocol_1.0.0.pdf?11895809671989535815). Para transmitir telemetría usó un adaptador [Inno-Maker: USB CAN Module](https://www.inno-maker.com/product/usb-can/).  La comunicación se puede establecer mediante Python y la librería python-can

Para entender como funciona el protocolo CAN están dos referencias fueron útiles para mi
- https://copperhilltech.com/blog/controller-area-network-can-bus-bus-arbitration/
- https://www.ni.com/en-my/innovations/white-papers/06/controller-area-network--can--overview.html

La libreria ```python-can``` esta disponible a través de ```pip```. El codigo y la documentación se puede visitar en 
- https://python-can.readthedocs.io/en/stable/
- https://github.com/hardbyte/python-can

El protocolo de comandos está disponible en https://www.kdedirect.com/pages/resource-center bajo la sección "Electronics". Este fué implementado en Python 3 y se encuentra disponible en la carpeta ```kde_uas85uvc```

## 3) Telemetría usando el protocolo UAVCAN

El protocolo UAVCAN es el el estandar promovido por Droncode Foundation, su pagina web y documentación es https://uavcan.org/

