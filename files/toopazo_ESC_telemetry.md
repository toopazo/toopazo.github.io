Here we will test protocols for reading live telemetry from Electronic Speed Controllers (ESC). This is important in the context of UAV and Drone technology.  

# Live ESC telemetry
  
## Testing platform  
This was tested using a [Raspbnerry Pi](https://www.raspberrypi.org/) and a [PICAN2 board](https://copperhilltech.com/pican-2-can-bus-interface-for-raspberry-pi/)  

RaspberryPi does not have a native CAN peripheral, therefore  CAN to SPI bridge is necessary. This can be done with Microchip MCP2515 CAN controller and a MCP2551 CAN transceiver.

### Hardware
The hardware used is called PICAN2 and it was designed by [Skpang ](http://skpang.co.uk/catalog/pican2-canbus-board-for-raspberry-pi-2-p-1475.html). The seller/distributor of this product is [Copperhill](https://copperhilltech.com).  Python applications are run using python-can
- https://github.com/hardbyte/python-can
- https://python-can.readthedocs.io/en/stable/index.html
    
Copperhill also offers an [alternative product](https://copperhilltech.com/can-bus-plus-rs485-hat-for-raspberry-pi/)

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

### How CAN works
To understand how CAN works these two references are recommended
- https://copperhilltech.com/blog/controller-area-network-can-bus-bus-arbitration/
- https://www.ni.com/en-my/innovations/white-papers/06/controller-area-network--can--overview.html

### Software
The python-can library is available through pip. The code and documentation can be found in
- https://python-can.readthedocs.io/en/stable/
- https://github.com/hardbyte/python-can

## KDECAN protocol
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

### Checking ESC's 
In binary format

|source|esc id|object|11-bit message|
|--|--|--|--|
|0 | 01011|00000|00101100000|
|From master to ESC|send to ID 11 (0B hex and 01011 bin)|Receive ESC Programming Information|Complete message|

![kdecan_pinout](https://raw.githubusercontent.com/toopazo/imgodg/main/uas_technology/kdecan_pinout.png)

### KDE Device manager adapter
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

## UAVCAN protocol
There is also a UAVCAN format for communication with px4
- https://uavcan.org/
- https://uavcan.org/Specification/2._Basic_concepts/

## Phoenix Edge HV
In particular we tested the [Phoenix Edge HV 80 AMP ESC](https://www.castlecreations.com/en/commercial/phoenix-edge-hv80-esc-010-0105-00), which is programmed using a [Castle Link adapter](https://www.castlecreations.com/en/castle-link-v3-usb-programming-kit-011-0119-00) and the [Castle Link Software](https://home.castlecreations.com/download-castle-link/).

In case we want to get live telemetry, then we need the [Castle Serial Link](https://www.castlecreations.com/en/serial-link-010-0121-00).

### Castle Serial Link library
https://github.com/math2peters/CastleSerialLink


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzAxMTI5NjY4XX0=
-->