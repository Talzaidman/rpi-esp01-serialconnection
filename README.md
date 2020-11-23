### welcome!

we are going to connect the ESP-01 wifi module with the raspberry pi 3, via serial port UART communication, and communicate with it with AT commands.  

![image of ESP01 and rpi 3](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/rpi-esp01.jpg)

### Contents
- [Introduction](#Introduction)
  - [What it is](#what-it-is)
  - [How it works](#how-it-works)
- [Disclaimer](#disclaimer)
- [Installation](#installation)
  - [Preparation](#preparation)  
  - [ESP-01](#ESP-01)
  - [wiring](#wiring)
  - [Raspberry pi 3](#Raspberry-pi-3)



## Introduction ##

### What it is

The ESP-01 is a micro-controller chip that includes on-board Wi-Fi. Originally intended as a UART to WiFi adaptor, allowing other micro-controllers to connect to a Wi-Fi network and make simple TCP/IP connections. the ESP-01 quickly became popular as a stand alone micro-controller because of its low price point.

![image of ESP01 and rpi 3](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/esp01.jpg)

Technically, rpi 3 has wifi capability, thus eliminating  the need to add the ESP-01 as a wifi add on. but neither  the less, we can still use the rpi to configure and control the ESP-01.

### How it works

Connecting the ESP-01 to the rpi is implemented via the serial port by connecting the ESP-01 to the UART pins on the rpi.
By adjusting the rpi configurations, we're able to screen the serial port and communicating with the ESP-01 with AT commands.

### Disclaimer

Use it only for testing purposes on your own devices!  
I don't take any responsibility for what you do with this project. 

## Installation

### Preparation

what you'll need:
- **ESP-01 Wi-Fi chip** 
- **raspberry pi 3(I use the B+)** 
- **jumper wires**  
- **Some skill, knowledge and common sense on this topic**  

### ESP-01

the ESP-01 chip is supposed to come with the software pre-installed straight from the manufacturer. thus technically we don't need to do anything with it.
if for some reason your ESP-01 is not responsive there are three options for what the problem might be:
1) you've done something incorrectly - the connection with the ESP-01 isn't established.
2) your ESP-01's software is corrupted, and you should consider to flash new software on it.
3) your ESP-01 is damaged. luckly, it's not expensive...

### Wiring

connecting the ESP-01 to the rpi is fairly simple.

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

Most of the work is configuring the rpi OS.

First of all, if your rpi is brend new, install the image maneger from the official raspberry pi site on your windows pc, and flash the rasbian OS on a flash drive. afterwards, boot the rpi and go threw the first data/keyboard/etc configuration.

Once your rpi is ready, we're starting with the configuration of the OS it self. Serial interfaces have long been Linux's and before this Unix's way of connecting to the outside world. As a result there is support for serial consoles built into the Kernel. When Linux boots up Linux generally configures at least one serial interface to work as a console.

What this means is that your program cannot simply connect to the serial interface because Linux is already using it. 

So now we are going to stop linux from using the serial port which we'll use.

Open your rpi terminal and type

`sudo raspi-config`

the raspberry pi software configuration window will appear.

![image of the interface](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/interface.jpeg)

next, scroll to interface options and press enter.

![image of the serial select](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/serial.jpeg)

scroll to serial port and press enter.

![image of the interface choise](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/shell.jpeg)

select NO and press enter.

![image of the hardware choise](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/hardware.jpeg)

select YES and press enter.

Now, when that's done reboot the rpi. now we need to configure some OS files. nothing to complicated.

First, go to the terminal and enter 

`sudo vim /boot/config.txt` or `sudo nano /boot/config.txt` (depends what editing utility you prefer)

Add to the end of the file:
`dtoverlay=pi3-miniuart-bt`
(the reason for doing so, is that Due to the Pi 3's support for Bluetooth, the full serial interface is now used by the built-in Bluetooth device. therefore, we need to disable it)

After rebooting, you can notice that when listing the /dev directory(ls -l /dev/) the following is listed:

`serial0 -> ttyAMA0
serial1 -> ttyS0`

In the rsp 3, the full UART is on ttyAMA0 and the serial0 is the one we are going to use. so now we can continue with our configurations. 

Our next step is to open a terminal threw the serial0 port. we'll do so by using the minicom utility.
install it with:

`sudo apt install minicom`

Then, type:

`sudo minicom -s`

Now the minicom utility opened with it's setting configuration window.
go to "serial port settings" as seen in the image below:

![image of the minicom settings](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/config.jpeg)

Inside, you'll need to change the Hardware flow control to 'off'.
also, change the port to /dev/serial0.

![image of the serial port settings](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/flow.jpeg)

Press 'esc' and then "save setup as dfl". then exit.

That's it! you now should be able to use the minicom terminal to talk with the esp-01 with AT commands.

To do so, in the shell terminal, type:

`sudo minicom -b 115200 -o -D /dev/serial0` 

In the minicom terminal, type AT commands followed by ctrl-m and then ctrl-j. you should get the OK reply, meaning that the ESP-01 got the command and replied.

that's it. enjoy!



