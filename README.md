### welcome!
we are going to connect the amazing esp-01 wifi module with the raspberrypi 3, via serial port UART communication, and communicate with it with AT commands. 
![image of esp01 and rpi 3](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/rpi-esp01.jpg)

### Contents
- [Introduction](#Introduction)
  - [What it is](#what-it-is)
  - [How it works](#how-it-works)
- [Disclaimer](#disclaimer)
- [Installation](#installation)
  - [Preparation](#preparation)  
  - [ESP01](#esp01)
  - [wiring](#wiring)
- [How to use it](#how-to-use-it)
- [Improvements](#improvements)
- [Sources and additional Links](#sources-and-additional-links)

## Introduction ##

### What it is

The esp-01 is a micro-controller chip that includes on-board Wi-Fi. Originally intended as a UART to WiFi adaptor, allowing other micro-controllers to connect to a Wi-Fi network and make simple TCP/IP connections. the esp-01 quickly became popular as a stand alone micro-controller because of its low price point.

![image of esp01 and rpi 3](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/esp01.jpg)

technicaly, rpi 3 has wifi capability, thus eleminating the need to add the esp-01 as a wifi add on. but nether the less, we can still use the rpi to configure and control the esp-01.

### How it works

connecting the esp-01 to the rpi is implimented via the serial port by connecting the esp-01 to the UART pins on the rpi.
by adjusting the rpi configurations, we're able to screen the serial port and communicating with the esp-01 with AT commands.

### disclamer

Use it only for testing purposes on your own devices!  
I don't take any responsibility for what you do with this project. 

## Installation

### Preparation

what you'll need:
- **ESP-01 Wi-Fi chip** 
- **raspberry pi 3(I use the B+)** 
- **jumper wires**  
- **Some skill, knowledge and common sense on this topic**  

### esp-01

the esp-01 chip is supposed to come with the software pre-installed stright from the manufacturer. thus thechnicaly we don't need to do anything with it.
if for some reason your esp-01 is not responsive there are three options for what the problem might be:
1) you've done something incorectly - the connection with the esp-01 isn't astablished.
2) your esp-01's software is corupted, and you should concider to flash new software on it.
3) your esp-01 is dameged. luckly, it's not expencive...

### Wiring

connecting the esp-01 to the rpi is fairly simple.

Ground - connect to ground

TXO - the serial tx pin

GPIO2 - ignore

CHPD - chip enable connect to 3.3V

GPIO0 - ignore

RST - reset leave unconnected

RXI - the serial rx pin

VDD - Supply voltage connect to 3.3V


![image of the circuit](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/circuit.jpg)

### Raspberry pi 3

most of the work is configuring the rpi OS.

first of all, if your rpi is brend new, install the image maneger from the official raspberry pi site on your windows pc, and flash the rasbian OS on a flash drive. afterwards, boot the rpi and go threw the first data/keyboard/etc configuration.

once your rpi is ready, we're starting with the configuration of the OS it self. Serial interfaces have long been Linux's and before this Unix's way of connecting to the outside world. As a result there is support for serial consoles built into the Kernel. When Linux boots up Linux generally configures at least one serial interface to work as a console.

What this means is that your program cannot simply connect to the serial interface because Linux is already using it. 

so now we are going to stop linux from using the serial port which we'll use.

open your rpi terminal and type

`sudo raspi-config`

the raspberry pi software configuration window will apear.

![image of the interface](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/interface.jpeg)

next, scroll to interface options and press enter.

![image of the serial select](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/serial.jpeg)

scroll to serial port and press enter.

![image of the interface choise](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/shell.jpeg)

select no enter press enter.

![image of the hardware choise](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/hardware.jpeg)

select yes and press enter.

now, when that's done reboot the rpi. now we need to configure some OS files. nothing to complicated.

first, go to the terminal and enter 

`sudo vim /boot/config.txt` or `sudo nano /boot/config.txt` (depends what editing utility you prefer)

add to the end of the file:
`dtoverlay=pi3-miniuart-bt`
(the reason for doing so, is that Due to the Pi 3's support for Bluetooth, the full serial interface is now used by the built-in Bluetooth device. therefor, we need to disable it)

after rebooting, you can notice that when listing the /dev dirctory(ls -l /dev/) the following is listed:

`serial0 -> ttyAMA0
serial1 -> ttyS0`

in the rsp 3, the full UART is on ttyAMA0 and the serial0 is the one we are going to use. so now we can continue with our configurations. 

our next step is to open a terminal threw the serial0 port. we'll do so by usuing the minicom utility.
install it with:

`sudo apt install minicom`

then, type:

`sudo minicom -s`

now the minicom utility opened with it's setting configutartion window.
go to "serial port settings" as seen in the image below:

![image of the minicom settings](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/config.jpeg)

inside, you'll need to change the Hardware flow control to 'off'.
also, change the port to /dev/serial0.

![image of the serial port settings](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/flow.jpeg)

press 'esc' and then "save setup as dfl". then exit.

that's it! you now should be able to use the minicom terminal to talk with the esp-01 with AT commands.

to do so, in the shell terminal, type:

`sudo minicom -b 115200 -o -D /dev/serial0` 

in the minicom terminal, type AT commands followed by ctrl-m and then ctrl-j. you should get the OK reply, meaning that the esp-01 got the command ant replied.





