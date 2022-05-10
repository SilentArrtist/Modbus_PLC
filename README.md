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

![image](https://user-images.githubusercontent.com/104362972/165133199-26c27aa3-284c-4eb5-9659-80d924a4e6f1.png)</br>

Now it remains only to open the console, enter the "ifconfig" command and see the ip of the device</br>

![image](https://user-images.githubusercontent.com/104362972/165132322-654513b1-56f4-4efc-8f4e-16c4c3086c63.png)</br>

# Connecting to Raspberry Pi from Computer via VNC #
Download VNC Viewer and run it</br>
Right click and select new connection</br>
Enter the ip that you received in the "ifconfig", set the port to 5900</br>

![image](https://user-images.githubusercontent.com/104362972/165132823-92246c96-1792-4fee-9e40-310e5173a641.png)</br>

Username leave pi, as password enter your password</br>

![image](https://user-images.githubusercontent.com/104362972/165133428-db239a02-0f18-42ec-b42e-d68807ec4341.png)</br>


Once connected, you will be able to control your Raspberry from your PC</br>

![image](https://user-images.githubusercontent.com/104362972/165134047-df01eb30-7cf6-44ff-b2e3-102881888648.png)

# Installing some libraries #
Use next commands to instal EasyModbus and virtualenv.
>pip3 install EasyModbus </br>
>sudo pip install virtualenv</br>


# Installing the Network Manager #
We will now make your Raspberry Pi create a local Wi-Fi network. Сonnect to internet and open terminal.</br>
First, let's update obsolete packages in the system. Use the following command: "sudo apt update && sudo apt full-upgrade".</br>
Now install Network Manager using command "sudo apt install network-manager network-manager-gnome"</br>
Now open the NetworkManager.conf file with the command "sudo nano /etc/NetworkManager/NetworkManager.conf"</br>

Delete everything and add these lines.</br>
>[main]</br>
>plugins=ifupdown,keyfile</br>
>dhcp=internal</br>
></br>
>[ifupdown]</br>
>managed=true</br>

To save file use CTRL+S and CTRL+X to exit

Now open the dhcpcd.conf file with the command "sudo nano /etc/NetworkManager/NetworkManager.conf"</br>
###  Connect your mouse and keyboard, because the VNC connection will be disconnected after the reboot. </br> 
Add this line:</br>
>denyinterfaces wlan0

To reboot, use "sudo reboot"

After reboot click on the internet icon and connect to your WI-FI by entering the password</br>

![image](https://user-images.githubusercontent.com/104362972/166480970-bf679aff-be64-4737-9e33-11c8442d2cbf.png)</br>

# Installing Django and deploy server to local network #

To begin with, let's go to the root directory using the "cd /" command and look at the folders contained in it using the "ls" command. Remember these commands, we will use them often. </br>

![image](https://user-images.githubusercontent.com/104362972/166484786-6c2fa88e-a93d-4648-8ff8-ef96f9334533.png)</br>

Create two folders using the commands "sudo mkdir -m 1777 /share" and "sudo mkdir -m 1777 /shareclear"</br>

![image](https://user-images.githubusercontent.com/104362972/166486325-35d0e3fd-5418-4a18-bf76-67b44f8c6936.png)</br>

Now install samba using command "sudo apt-get install samba samba-common-bin"</br>
Agree and select "yes" in the dialog box </br>

![image](https://user-images.githubusercontent.com/104362972/166486572-50128eb5-cd35-43cb-9250-5f0f65b5e9e5.png)</br>

Open the samba .conf file with the command "sudo nano /etc/samba/smb.conf" </br>
Paste the following at the end of the file: </br>

>[share]</br>
>Comment = Pi shared folder</br>
>Path = /share</br>
>Browseable = yes</br>
>Writeable = Yes</br>
>only guest = no</br>
>create mask = 0777</br>
>directory mask = 0777</br>
>Public = yes</br>
>Guest ok = yes</br>
>force user = pi</br>
>force group = pi</br>

Save and exit by using CTRL+S and CTRL+X </br>

![image](https://user-images.githubusercontent.com/104362972/166488590-e68fbdc0-5a6e-4b64-8456-ddf2aab706c9.png) </br>

Now reboot the system </br>
Let's create a folder where the project will be stored. To do this, use the following commands: </br>
>cd / </br>
>cd /share </br>
>sudo mkdir "FolderName" </br>
>cd /"FolderName" </br>

I named the folder "Django", I will use this name in the future</br>

![image](https://user-images.githubusercontent.com/104362972/166490758-f77b9a1f-4cfb-4fac-867d-e3fc6ecdb488.png)</br>

