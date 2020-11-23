### welcome!
we are going to connect the amazing esp-01 wifi module with the raspberrypi 3, via serial port UART communication, and communicate with it with AT commands. 
![image of esp01 and rpi 3](https://raw.githubusercontent.com/Talzaidman/rpi-esp01-serialconnection/main/images/rpi-esp01.jpg)
### Contents
- [Introduction](#Introduction)
  - [What it is](#what-it-is)
  - [How it works](#how-it-works)
  - [The benefits of adding Wi-Fi](#the-benefits-of-adding-wi-fi)
- [Disclaimer](#disclaimer)
- [Installation](#installation)
  - [Preparation](#preparation)  
  - [ESP8266](#esp8266)
  - [Arduino ATmega32u4](#arduino-atmega32u4)
  - [Wire everything up](#wire-everything-up)
  - [Update ESP8266 over the Webinterface](#update-esp8266-over-the-webinterface)
- [How to use it](#how-to-use-it)
- [Improvements](#improvements)
- [License](#license)
- [Sources and additional Links](#sources-and-additional-links)

## Introduction ##

### What it is
first of all we need to download and flash rpi software to our rpi 3. easiest way to do so is to download the raspberry pi imager from the raspberry pi main site.
after you install the imager, connect your flash drive to your computer and flash the latest recommended raspbian software.
eject it and insert it to the rpi 3, then boot.
### How it works
next stage is to set up our
