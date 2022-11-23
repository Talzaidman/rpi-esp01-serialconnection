### Raspberry-Pi ESP01 serial connection

We're going to connect the ESP-01 wifi module with the raspberry pi 3, via serial port UART communication, and communicate with it with AT commands.  

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

The ESP-01 is a microcontroller chip that includes onboard Wi-Fi. Originally intended as a UART to WiFi adaptor, allowing other micro-controllers to connect to a Wi-Fi network and make simple TCP/IP connections. The ESP-01 quickly became popular as a stand-alone micro-controller because of its low price point.

![image of ESP01 and rpi 3](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/esp-01.jpg)

Technically, RPI 3 has wifi capability, thus eliminating the need to add the ESP-01 as a wifi add-on. But neither less, we can still use the RPI to configure and control the ESP-01.

### How it works

Connecting the ESP-01 to the RPI is implemented via the serial port by connecting the ESP-01 to the UART pins on the RPI.
By adjusting the RPI configurations, we're able to screen the serial port and communicate with the ESP-01 with AT commands.

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

The ESP-01 chip is supposed to come with the software pre-installed straight from the manufacturer. Thus, technically we don't need to do anything with it.
If for some reason, your ESP-01 is not responsive, there are three options for what the problem might be:
1) you've done something incorrectly - the connection with the ESP-01 isn't established.
2) your ESP-01's software is corrupted, and you should consider flashing new software on it.
3) your ESP-01 is damaged. Luckily, it's not expensive...

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

Most of the work is configuring the RPI OS.

First of all, if your RPI is brand new, install the image manager from the official RPI site on your windows pc, and flash the rasberian OS on a flash drive. Afterward, boot the RPI and go threw the first data/keyboard/etc configuration.

Once your RPI is ready, we're starting with the configuration of the OS itself. Serial interfaces have long been Linux's and before this Unix's way of connecting to the outside world. As a result, there is support for serial consoles built into the Kernel. When Linux boots up Linux generally configures at least one serial interface to work as a console.

It means that your program cannot simply connect to the serial interface because Linux is already using it. 

So now we're going to stop Linux from using the serial port which we'll use.

Open your rpi terminal and type

`sudo raspi-config`

The raspberry pi software configuration window will appear.

![image of the interface](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/interface.jpeg)

Next, scroll to interface options and press enter.

![image of the serial select](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/serial.jpeg)

Scroll to serial port and press enter.

![image of the interface choise](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/shell.jpeg)

Select NO and press enter.

![image of the hardware choise](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/hardware.jpeg)

Select YES and press enter.

Now, when that's done reboot the RPI. Now we need to configure some OS files. Nothing too complicated.

First, go to the terminal and enter 

`sudo vim /boot/config.txt` or `sudo nano /boot/config.txt` (depends what editing utility you prefer)

Add to the end of the file:
`dtoverlay=pi3-miniuart-bt`
(the reason for doing it, is that Due to the RPI 3's support for Bluetooth, the full serial interface is now used by the built-in Bluetooth device. Therefore, we need to disable it)

After rebooting, you can notice that when listing the /dev directory(ls -l /dev/) the following is listed:

`serial0 -> ttyAMA0
serial1 -> ttyS0`

In the rsp 3, the full UART is on ttyAMA0 and the serial0 is the one we are going to use. so now we can continue with our configurations. 

Our next step is to open a terminal threw the serial0 port. We'll do so by using the minicom utility.
Install it with:

`sudo apt install minicom`

Then, type:

`sudo minicom -s`

Now the minicom utility opened with it's setting configuration window.
Go to "serial port settings" as seen in the image below:

![image of the minicom settings](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/config.jpeg)

Inside, you'll need to change the Hardware flow control to 'off'.
Also, change the port to /dev/serial0.

![image of the serial port settings](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/flow.jpeg)

Press 'esc' and then "save setup as dfl". then exit.

That's it! you now should be able to use the minicom terminal to talk with the esp-01 with AT commands.

To do so, in the shell terminal, type:

`sudo minicom -b 115200 -o -D /dev/serial0` 

In the minicom terminal, type AT commands followed by ctrl-m and then ctrl-j. You should get the OK reply, meaning that the ESP-01 got the command and replied.

That's it. enjoy!



