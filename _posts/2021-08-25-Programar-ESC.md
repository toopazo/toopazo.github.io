---
title: Como programar un ESC para obtener datos en linea
date: 2021-08-25
categories: 
- postscyt
layout: archive
collection: postscyt
---

El presente escrito es un reporte acerca de mi experiencia programando un ESC para obtener datos en linea. Un ESC es un controlador para motores sin escobilla que aumenta o disminuye la potencia inyectada al motor para así regular su velocidad de giro. Obtener datos instantáneos como voltaje, corriente y sobretodo velocidad del motor son de vital importancia para estimar el consumo de cada rotor y pueden ser usado incluso como parte del controlador de vuelo.

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

El ESC exacto en ser probado corresponde al modelo [Phoenix Edge HV 80 AMP ESC](https://www.castlecreations.com/en/commercial/phoenix-edge-hv80-esc-010-0105-00), el cual es programado usando el adaptador [Castle Link adapter](https://www.castlecreations.com/en/castle-link-v3-usb-programming-kit-011-0119-00), este corresponde a un simple y ordinario conversor serial FTDI, pero que venden por unos 20 USD, una triquiñuela para sacar algo de dinero extra. Pero bueno, eso es tema para otro día. 
Una vez teniendo el ESC y el adaptador USB debemos descargar el programa [Castle Link Software](https://home.castlecreations.com/download-castle-link/). Esta disponible para Windows solamente, pero podemos instalar una maquina virtual si es que estamos en Linux.

<figure>
  <img src="https://toopazo.github.io/images/castle_enable_live_link.png" style="width:100" alt="alt_text" />
  <figcaption> Pasos 1, 2 y 3 para habilitar el "Live Link" en los ESC de Castle Creations </figcaption>
</figure> 

In case we want to get live telemetry, then we need the [Castle Serial Link](https://www.castlecreations.com/en/serial-link-010-0121-00).

### Castle Serial Link library
https://github.com/math2peters/CastleSerialLink

## 2) Telemetría usando el protocolo UAVCAN

El protocolo UAVCAN es el el estandar promovido por Droncode Foundation, su pagina web y documentación es https://uavcan.org/

## 3) Telemetría usando el modelo [KDE-UAS85UVC](https://www.kdedirect.com/collections/uas-multi-rotor-electronics/products/kde-uas85uvc) 

|**Nota: Aún no logro hacer funcionar ningún modelo de la marca KDE**|
|:-:|
  
### Plataforma de pruebas

Los ESC de la empresa KDE usando el protocolo CAN propietario de empresa llamado [KDECAN](https://cdn.shopify.com/s/files/1/0496/8205/files/KDECAN_Bus_Protocol_1.0.0.pdf?11895809671989535815) para transmitir telemetría, es por ello que para estas pruebas pruebas se usó una [Raspbnerry Pi](https://www.raspberrypi.org/) y la placa montable [PICAN2 board](https://copperhilltech.com/pican-2-can-bus-interface-for-raspberry-pi/). Esta última placa provee a la Raspberry de una interfaz CAN que no tiene por defecto. Para esto se emplea el puerto SPI de las Raspberry usando un chip modelo Microchip MCP2515 y otro llamado  MCP2551. Esta placa montable fue diseñada por [Skpang ](http://skpang.co.uk/catalog/pican2-canbus-board-for-raspberry-pi-2-p-1475.html). Se puede adquirir a través de [Copperhill](https://copperhilltech.com).  La comunicación se puede establecer usando Python y la librería python-can

- https://github.com/hardbyte/python-can
- https://python-can.readthedocs.io/en/stable/index.html
    
Copperhill ofrece lectores CAN similares en la pagina web [alternative product](https://copperhilltech.com/can-bus-plus-rs485-hat-for-raspberry-pi/)

### ¿Como hacer funcionar la placa PICAN2?

```bash
pip install python-can
pip install can-utils

sudo /sbin/ip link set can0 down
sudo /sbin/ip link set can0 up type can bitrate 500000

sudo ip link set can0 down
sudo ip link set can0 up type can bitrate 500000 restart-ms 100

sudo ifconfig can0 txqueuelen 1000
```
This last lines can be added to "/etc/rc.local" file

But first we can check if the system works in loopback mode by executing
```
sudo /sbin/ip link set can0 down
sudo ip link set can0 up type can bitrate 500000 loopback on
```
This can be done by opening a parallel terminal and running the following
| `cansend can0 160#0123456789ABCDEF`  | `candump can0`|
|--|--|
| (no return)  | `can0  160   [8]  01 23 45 67 89 AB CD EF` <br /> `can0  160   [8]  01 23 45 67 89 AB CD EF` |

We can check the status of the can0 interface by typing 
`ip -det link show can0`

### ¿Como funciona CAN?

Para entender como funciona el protocolo CAN están dos referencias fueron útiles para mi
- https://copperhilltech.com/blog/controller-area-network-can-bus-bus-arbitration/
- https://www.ni.com/en-my/innovations/white-papers/06/controller-area-network--can--overview.html

### Software
La libreria ```python-can``` esta disponible a través de ```pip```. El codigo y la documentación se puede visitar en 
- https://python-can.readthedocs.io/en/stable/
- https://github.com/hardbyte/python-can

## ¿Como es el protocolo KDECAN?
The documents describing the protocol can be found in KDE's website https://www.kdedirect.com/pages/resource-center under the section "Electronics"

    CAN Bus Standard Frame Structure (CAN 2.0A)    
    1bit source 
    5bit ESC ID
    5bit object
    1 54321 54321

    CAN Bus Extended Frame Structure (CAN 2.0B)
    5bit priority 
    8bit source 
    8bit destination 
    8bit object
    54321 87654321 87654321 87654321

    Examples from can-test_pi2/candump can0
    can0  18042701   [2]  56 69
    11000 00000100 00100111 00000001

    can0  01000401   [8]  B1 49 84 14 06 00 00 C8 
    00001 00000000 00000100 00000001

### Probando la comunicación con el ESC 
In binary format
|source|esc id|object|11-bit message|
|--|--|--|--|
|0 | 01011|00000|00101100000|
|From master to ESC|send to ID 11 (0B hex and 01011 bin)|Receive ESC Programming Information|Complete message|
![kdecan_pinout](https://raw.githubusercontent.com/toopazo/imgodg/main/uas_technology/kdecan_pinout.png)

Using `dmesg` we can check the USB metadata
```
[237987.452387] usb 1-2: new full-speed USB device number 64 using xhci_hcd
[237987.601553] usb 1-2: New USB device found, idVendor=10c4, idProduct=ea60
[237987.601557] usb 1-2: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[237987.601558] usb 1-2: Product: CP2102 USB to UART Bridge Controller
[237987.601560] usb 1-2: Manufacturer: Silicon Labs
[237987.601561] usb 1-2: SerialNumber: 0001
[237987.603667] cp210x 1-2:1.0: cp210x converter detected
[237987.605343] usb 1-2: cp210x converter now attached to ttyUSB0

```

