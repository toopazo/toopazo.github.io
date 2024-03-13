---
title: Protocolos de comunicación ESC
layout: post
mathjax: true
categories: media
---

El presente escrito es un reporte acerca de mi experiencia programando un ESC para obtener datos en linea. Un ESC es un controlador para motores sin escobilla que aumenta o disminuye la potencia inyectada a éste para así regular su velocidad de giro. Obtener datos instantáneos como voltaje, corriente y velocidad del motor son de vital importancia para estimar el consumo de cada rotor y pueden ser usados incluso como parte del controlador de vuelo. El software descrito en este reporte se encuentra disponible en el repositorio https://github.com/toopazo/live_esc

<center>
<figure>
  <img src="https://toopazo.github.io/images/uavcan_esc.png" style="width:45%" alt="alt_text" /> 
  <img src="https://toopazo.github.io/images/castle_phoenix_edge_hv_80.jpg" style="width:25%" alt="alt_text" /> 
  <img src="https://toopazo.github.io/images/kde-uas85uvc.jpg" style="width:25%" alt="alt_text" />
  <figcaption> Diferentes modelos de ESC usados en multirotores: Orel y Kotleta (UAVCAN), Phoenix Edge y KDE-UAS </figcaption>
</figure>
</center> 


## 1) Como instalar el código
Las instrucciones para instalar el código son:
```shell
git clone https://github.com/toopazo/live_esc.git
cd live_esc/
. ./create_venv.sh
```
Es necesario asegurarse de que al final el script tengamos el entorno activo, esto es visible en el identificador del terminal
```(venv) user@host:```

En caso contrario simplemente hay que ejecutar
```shell
source venv/bin/activate
```
Para desactivar el entorno se debe ejecutar
```shell
deactivate
```

## 2) Telemetría usando el modelo [Phoenix Edge HV](https://www.castlecreations.com/en/phoenix-edge-hv)

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

<center>
<figure>
  <img src="https://toopazo.github.io/images/castle_link_programmer.jpeg" style="width:90%" alt="alt_text" />
  <figcaption> Conexión entre el programador USB (Castle Link v3 USB programming kit) y el ESC </figcaption>
</figure>
</center> 

Una vez teniendo conectado el ESC al programador USB debemos descargar el programa [Castle Link Software](https://home.castlecreations.com/download-castle-link/). Esta disponible para Windows solamente, pero podemos instalar una maquina virtual si es que estamos en Linux.

<center>
<figure>
  <img src="https://toopazo.github.io/images/castle_enable_live_link.png" style="width:90%" alt="alt_text" />
  <figcaption> Pasos 1, 2 y 3 para habilitar el "Castle Link Live" en los ESC de Castle Creations </figcaption>
</figure> 
</center>

Una vez habilitada la comunicación por protocolo [Castle Link Live](https://www.castlecreations.com/castle-link-live) debemos hacernos de otro adaptador, esta vez llamado [Castle Serial Link](https://www.castlecreations.com/en/serial-link-010-0121-00). Este si es un poco más interesante en el sentido que nos permite establecer un bus de datos al cual conectar los múltiples ESC que tengamos en un multirotor. La conexión hacia el computador se puede hacer con un USB Ftdi como se ve en la figura de más abajo. El voltaje en el BUS del Castle Serial Link debe ser de +5V en caso de que el ESC lo requiera, que es justamente lo que sucede con el modelo Phoenix Edge HV.

<center>
<figure>
  <img src="https://toopazo.github.io/images/castle_usbFtdi_serialLink.png" style="width:90%" alt="alt_text" />
  <figcaption> Conexión entre conversor USB Ftdi y Castle Serial Link </figcaption>
</figure>
</center> 

Luego de esta configuración inicial podemos empezar a desarrollar un programa para comunicarnos con los ESC y obtener datos en linea. Sin embargo en vez de reinventar la rueda ocuparemos un código en Python disponible en el Github  https://github.com/math2peters/CastleSerialLink. Lo que haremos es simplemente usar el código de ```mathpeters``` y modificarlo para nuestros propósitos de telemetría. El código se encuentra dentro de la carpeta dedicada a los modelos Phoenix Edge dentro del ya mencionado repositorio [live_esc](https://github.com/toopazo/live_esc). 

La instrucciones para ejecutar el código son:
```shell
cd phoenix_edge_hv_80/CastleSerialLink
python3 read_esc_data.py
python3 plot_esc_data.py
```

El primer script de python crea un archivo con los datos obtenidos desde el ESC, el segundo script de python lee esos datos y los grafica. El resultado puede ser observado más abajo.

<center>
<figure>
  <img src="https://toopazo.github.io/images/phoenix_edge_hv_telemtry_pandas.png" style="width:100%" alt="alt_text" />
  <figcaption> Telemetría en vivo obtenida a través del Serial Link </figcaption>
</figure>
</center> 

El motor fue hecho trabajar sin carga, por ende tanto la corriente como los efectos de ripple se esperaban como menores. Sin embargo al parecer el ESC no tiene una buena precisión para estas variables. Este fenómeno también ha sido observado con otras marcas de ESC. Al parecer ninguna de ellas entrega valores fidedignos de corriente consumida. Voltaje, rpm y otras variables parecen ser precisas. 

El modo ```TTL Serial``` de Castle Creations funcionó correctamente, tanto en leer variables como en comandar ```throttle``` al motor. Para volar un Drone en este modo se requeriría conectar ***de manera digital (es decir por software)*** la salida ```IO PWM out``` del Pixhawk con la escritura del registro de ```throttle``` recién mencionada.

Si quiseramos ocupar directamente la señal ```IO PWM out``` hay por ahora malas noticias. Pues en estas pruebas nunca se pudo hacer funcionar el modo ```TTL Serial (with PPM Input)``` de los ESC de Castle Creations. Esto ya que el adaptador Serial Link no reconocia la señal análoga desde el Pixahwk. En especifico, la señal desde el puerto ```IO PWM out``` del Pixhawk hacia el motor 1 (podria haber sido cualquier otro motor 2, 3 .. 8) fue conectada al pin ```D``` del dispositivo Serial Link, pero el motor no respondió. Todo parece indicar que el problema es con Serial Link y no con el ESC o el motor. 

## 3) Telemetría usando el modelo [KDE-UAS85UVC](https://www.kdedirect.com/collections/uas-multi-rotor-electronics/products/kde-uas85uvc) 
 
La lista de materiales para esta prueba fue:
- [KDE Direct: KDE-UAS85UVC](https://www.kdedirect.com/collections/uas-multi-rotor-electronics/products/kde-uas85uvc)
- [KDE Direct: Device Manager (software)](https://www.kdedirect.com/collections/uas-multi-rotor-electronics/products/kde-dms?page=specs) 
- [HolyBro: Pixhawk 4](http://www.holybro.com/product/pixhawk-4/)
- [KDE Direct: KDE 6213XF-185 motor](https://www.kdedirect.com/products/kde6213xf-185)
- [Inno-Maker: USB CAN Module](https://www.inno-maker.com/product/usb-can/)
- Laptop con python 3
- Baterías de 4S2P  (dos baterías 4S en serie => 29.6 V)
- Un regulador de voltaje de 5V para energizar la linea de alimentación del Pixahwk

Los ESC de la empresa KDE usan el protocolo CAN para transmitir datos con una serie de comandos llamados [KDECAN](https://cdn.shopify.com/s/files/1/0496/8205/files/KDECAN_Bus_Protocol_1.0.0.pdf?11895809671989535815). Para transmitir telemetría se usó un adaptador [Inno-Maker: USB CAN Module](https://www.inno-maker.com/product/usb-can/). La comunicación se puede establecer mediante Python y la librería ```python-can```. Para entender como funciona el protocolo CAN estás dos referencias son bastante útiles
- https://copperhilltech.com/blog/controller-area-network-can-bus-bus-arbitration/
- https://www.ni.com/en-my/innovations/white-papers/06/controller-area-network--can--overview.html

El protocolo de comandos de KDECAN está disponible en https://www.kdedirect.com/pages/resource-center bajo la sección "Electronics". Pero tambien se puede encontrar en [este link](https://github.com/toopazo/live_esc/blob/main/kde_uas85uvc/KDECAN_Bus_Protocol_1.0.3.pdf). El código fué implementado en Python 3 y se encuentra disponible en la carpeta de KDE que forma parte del ya mencionado repositorio [live_esc](https://github.com/toopazo/live_esc). 

La instrucciones para ejecutar el código son:
```shell
cd kde_uas85uvc
python kdecan_main.py
python kdecan_plot_log.py
```

El primer script de python ejecutado se comunica con el ESC y guarda los datos a un log. El segundo script se encarga de generar la imagen de más abajo.

<center>
<figure>
  <img src="https://toopazo.github.io/images/kdecan_telemtry_pandas.png" style="width:100%" alt="alt_text" />
  <figcaption> Telemetría en vivo obtenida a través de KDECAN </figcaption>
</figure>
</center> 

El software de los ESC de KDE no permiten, por ahora, comandar un determinado ```throttle``` al motor. Sin embargo podemos ocupar la salida analoga ```IO PWM out``` directamente desde el Pixhawk y así volar el vehículo. Esta es la situación opuesta al modelo de Caste Creations. 

## 4) Telemetría usando el protocolo UAVCAN

El protocolo UAVCAN es el estandar promovido por Droncode Foundation, su pagina web y documentación es https://uavcan.org/. Lamentablemente los ESC que incorporan esta tecnología son de muy bajo amperaje y no son compatibles con el motor que yo tenía dispoible para esta seria de pruebas. Queda entonces para más adelante. 


|El siguiente es un artículo escrito con fines de divulgación y hecho con el mayor cuidado. Sin embargo no puedo garantizar que todo lo expuesto sea 100% correcto. Si encuentra algún error por favor háganmelo saber.|
|--|
|Historial de actualizaciones <ul><li>25-08-2021</li><li>26-08-2021</li><li>27-08-2021</li><li>18-09-2021</li></ul>|
