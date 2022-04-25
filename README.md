# Modbus_PLC
Data exchange between Raspberry and PLC via Modbus

In this article I want to talk about how to create connections via modbus protocol between PLC and Raspberry Pi

# What will be used #
  >1.Everything will work on the local network, so it will be necessary to teach Raspberry how to create a wi-fi network. For this we will use Network Manager</br>
  >2.To work and configure the PLC, we will use Codesys software</br>
  >3.The user interface will be shaped by creating a web page. Let's create it using the Django python framework. To exchange data via Modbus, we will use the EasyModbus library for python</br>
  
# What will be needed #
  >1.Raspberry Pi (I used 3B+)</br>
  >2.HDMI screen with HDMI cable</br>
  >3.MicroSD adapter</br>
  >4.MicroSD memory card 16 GB</br>
  >5.USB Power Adapter with microUSB cable</br>
  >6.Ethernet cable</br>
  >7. Mouse and Keyboard</br>

# Raspbian installation #
First of all follow the link https://downloads.raspberrypi.org/raspbian/images/raspbian-2019-07-12/ and download "2019-07-10-raspbian-buster.zip"</br>
Unzip this file, insert a microSD card into the adapter, connect it to your PC and install the image using Win32 Disk Imager or any other software for writing images to SD cards</br>
When the recording is finished, Remove the sd card from the adapter.

# First Start. VNC setup. Connecting to WI-Fi #
Insert the SD card into the Raspberry Pi, connect the power screen,mouse and keyboard to the Raspberry.</br>
Open terminal and use command "sudo raspi-config"</br>
Then go to point 5."Interfacing options"</br>
![image](https://user-images.githubusercontent.com/104362972/165131675-28c59b48-e3ec-45ed-823e-e4c96a2ad5c0.png)</br>
Then go to point 3."VNC"</br>
![image](https://user-images.githubusercontent.com/104362972/165131752-32e8ee74-0196-4189-b35a-19f3dda2f4d6.png)</br>
Confirm enabling the VNC server by pressing enter</br>
Open terminal again and use command "sudo raspi-config", then go to point 1 to change password.</br>
Next, in the console, enter the password for your Raspberry pi twice</br>
After that, click on the internet icon, select your country and connect to your WI-FI by entering the password</br>
![image](https://user-images.githubusercontent.com/104362972/165131465-489cd597-6a11-452d-a099-8876c3d336e1.png)</br>

Now it remains only to open the console, enter the "ifconfig" command and see the ip of the device</br>

![image](https://user-images.githubusercontent.com/104362972/165132322-654513b1-56f4-4efc-8f4e-16c4c3086c63.png)</br>
# Connecting to Raspberry Pi from Computer via VNC #
