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

# Installing virtual enviroment #
Use next command virtualenv:</br>
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

# PLC preparation #
Для начала скачаем ПО для настройки нашего PLC. Поскольку я буду использовать плк от компании OWEN, я буду использовать codesys. Скачаем его с оффициального сайта https://owen.ru/product/codesys_v3

![image](https://user-images.githubusercontent.com/104362972/168025530-3a6c86ff-2a39-4765-a04a-8a5915fdeda7.png)

Скачиваем необходимую версиую, я выбрал v14.

Установим и запустим его
вы увидите следующую картинку 

![image](https://user-images.githubusercontent.com/104362972/168026359-33f56c72-42f0-4fc3-b79f-4b52b7bf8699.png)

Зайдем на сайт кодесис и загрузим таргет файлы для нашей модели плк. Для этого найдем его через поиск и откроем вкладку ПО и т.д.

![image](https://user-images.githubusercontent.com/104362972/168026871-a6e3d833-e428-486d-9692-a02666295c0e.png)

![image](https://user-images.githubusercontent.com/104362972/168026925-6b9ead28-367c-43b8-a21f-9a39f0158c5d.png)

скачиваем таргет файл под версию кодесис

выбираем tools->package manager

![image](https://user-images.githubusercontent.com/104362972/168027233-6311e214-e9e4-4c47-88c1-773fe585a3bb.png)

Нажимаем Install и выбираем скачанный файл 

![image](https://user-images.githubusercontent.com/104362972/168027643-ebfc9b56-1051-4723-a6fd-1c0f53357f37.png)

Нажмем на new project и выберем нужную модель ПЛК, далее ok

![image](https://user-images.githubusercontent.com/104362972/168027727-9b5e632d-de9f-44d3-b558-26b2b83e6a1e.png)

По итогу видим следующую картинку:

![image](https://user-images.githubusercontent.com/104362972/168029060-7b221e8d-5a7f-480d-b44c-bd1c1849e558.png)

Теперь добавим протокол Modbus
Следуйте за картинками

![image](https://user-images.githubusercontent.com/104362972/168029258-6866e65b-d03e-4b10-97b2-bcb19e58bfb8.png)</br>
![image](https://user-images.githubusercontent.com/104362972/168029475-50e845d1-edd0-40bc-9b50-b03a77ebbc7a.png)</br>

Далее двойной клик на modbus slave, там открываем mapping и даем названия нескольким holding и input registers

![image](https://user-images.githubusercontent.com/104362972/168029884-b79321f3-0001-4576-b64a-6e06dcc4e85f.png)

Создадим переменные в PLC_PRG и присвоим им значения. Так же зададим условия включения первого койла, если hold1 > 50

![image](https://user-images.githubusercontent.com/104362972/168030390-eb9263dd-1ea4-4cd7-93cb-c61900069a2d.png)


 
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
Let's create a folder where the projects will be stored. To do this, use the next commands: </br>
>cd / </br>
>cd /share </br>
>sudo mkdir "FolderName" </br>
>cd /"FolderName" </br>

I named the folder "Django", I will use this name in the future</br>

![image](https://user-images.githubusercontent.com/104362972/166490758-f77b9a1f-4cfb-4fac-867d-e3fc6ecdb488.png)</br>


Now you need to set up the virtual environment. To do this, open the project folder and enter the next command.

>virtualenv env2 -p python3

![image](https://user-images.githubusercontent.com/104362972/167621437-bffbc529-cb3a-4b2f-b384-4f76ed3b5be1.png)

Let's start the virtual environment with the next command:</br>
>source env2/bin/activate

![image](https://user-images.githubusercontent.com/104362972/167622073-45fe1022-2cad-439c-ad42-5b0268780429.png)</br>

Now download the archive I have attached and move the "plc" folder to your projects folder. You can use any flash drive for this. In order to open the file manager with administrator rights, you can use the next command:</br>
>sudo pcmanfm

![image](https://user-images.githubusercontent.com/104362972/167622904-a0da1fb7-a8fd-4e3a-bee9-7146418faba6.png)


After that, the projects folder will look like this:</br>
![image](https://user-images.githubusercontent.com/104362972/167623133-4161526d-4f21-48b4-bf77-dd46307e0cbf.png)</br>

Next, install Django with the command:</br>
>pip install Django</br>

Then opem plc folder and install EasyModbus:
>pip3 install EasyModbus

# Setting up a Django project #

Now we have to configure the project for the corresponding PLC. Now we will be editing the Django project files, so it will be easier to use some editor. If you installed Raspbian with a desktop, then open the file manager with "sudo pcmanfm" and navigate to the project folder. My path looks like this: "/share/Django/plc". Open the main folder, and in it open the files "views.py" , "models.py" and "forms.py"  through the editor. I am using Geany editor. 

</br>
Let's start with the "models.py" file. There you will see 5 values for holding registers. If you have more than 5 holding registers when setting up PLC and Modbus, then add them in the same way. The result is the next file:</br>
![image](https://user-images.githubusercontent.com/104362972/167631447-18bc8acb-94fb-4f85-a269-c8b0a6d37feb.png)</br>

If you have added additional fields for registersthen you need to migrate the database. Use the next commands in the folder where the manage.py file is located:
>python manage.py makemigrations</br>
>python manage.py migrate</br>

Now let's move on to the "forms.py" file. If you haven't added anything to the "models.py" file, you can skip this step.</br>
Adding widgets to the form by analogy.</br>

![image](https://user-images.githubusercontent.com/104362972/167634831-4c48effc-1f5d-47cc-845c-1a26ab8937db.png)</br>

After that, go to the file "views.py"<br>
In the ip variable, enter the ip address that was specified when setting up the PLC via Codesys</br>
If you added the number of holding registers to model.py, then change the value 5 to the number of your registers</br>

![image](https://user-images.githubusercontent.com/104362972/167633366-f009c366-4d55-4da4-bd3b-eea1b14db231.png)</br>

Now let's work a little on the UI. To do this, open the templates folder, and in it open the file "index.html"</br>
If you added new registers, then copy and paste the next block by analogy with the rest, changing its number:</br>

![image](https://user-images.githubusercontent.com/104362972/167634119-f36d7ebb-3cb5-4af8-a897-94bae920096c.png)</br>

P.S. N in this case is the register number.


Now we can start the server with the command "python manage.py runserver 0.0.0.0:8000"

Using a device that is on the same subnet as the Raspberry Pi, open a browser and enter "ip:8000" in the search bar, where ip is the ip of the Raspberry pi.

If you did everything correctly, you will see the next picture:

![image](https://user-images.githubusercontent.com/104362972/167635753-87872fe9-9a12-4f1a-9bf5-068b0956ed31.png)
