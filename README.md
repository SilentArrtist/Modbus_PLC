# Modbus_PLC
Data exchange between Raspberry and PLC via Modbus

In this article I want to talk about how to create connections via modbus protocol between PLC and Raspberry Pi

What will be used:
  1.Everything will work on the local network, so it will be necessary to teach Raspberry how to create a wi-fi network. For this we will use Network Manager
  2.To work and configure the PLC, we will use Codesys software
  3.The user interface will be shaped by creating a web page. Let's create it using the Django python framework. To exchange data via Modbus, we will use the EasyModbus library for python
  
What will be needed:
  1.Raspberry Pi (I used 3B+)
  2.HDMI screen with HDMI cable
  3.MicroSD adapter
  4.MicroSD memory card 16 GB
  5.USB Power Adapter with microUSB cable
  6.Ethernet cable

Let's start
First of all follow the link https://downloads.raspberrypi.org/raspbian/images/raspbian-2019-07-12/ and download 2019-07-10-raspbian-buster.zip
Unzip this file, insert a microSD card into the adapter, connect it to your PC and install the image using Win32 Disk Imager or any other software for writing images to SD cards
When the recording is finished, insert the SD card into the Raspberry PI, connect the power and screen to the raspberry


