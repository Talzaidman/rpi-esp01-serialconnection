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

### Raspberry pi 3

most of the work is configuring the rpi OS.

first of all, if your rpi is brend new, install the image maneger from the official raspberry pi site on your windows pc, and flash the rasbian OS on a flash drive. afterwards, boot the rpi and go threw the first data/keyboard/etc configuration.

once your rpi is ready, we're starting with the configuration of the OS it self. Serial interfaces have long been Linux's and before this Unix's way of connecting to the outside world. As a result there is support for serial consoles built into the Kernel. When Linux boots up Linux generally configures at least one serial interface to work as a console.

What this means is that your program cannot simply connect to the serial interface because Linux is already using it. 

so now we are going to stop linux from using the serial port which we'll use.

open your rpi terminal and type

`sudo raspi-config`

the raspberry pi software configuration window will apear.



first of all we need to download and flash rpi software to our rpi 3. easiest way to do so is to download the raspberry pi imager from the raspberry pi main site.
after you install the imager, connect your flash drive to your computer and flash the latest recommended raspbian software.
eject it and insert it to the rpi 3, then boot.



