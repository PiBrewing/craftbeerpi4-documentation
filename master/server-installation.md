# Server Installation

## How to install Craftbeerpi4 and it's user interface based on a clean raspbian image

There is also the possibility to install the server from a pre-configured image. Details are described [here](server-installation.md#installation-of-craftbeerpi-4-from-a-pre-configured-image-to-your-sd-card).

### Installation of Raspbian and preparation of the system

First you will need to install Raspbian from an image. The installer can be found here: [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/)

{% hint style="info" %}
CraftbeerPi4 requires python 3.11 and it is recommended to install it on bookworm. It is recommended to use the  64 bit version as recent updates of required packages may cause issues on 32 bit systems. This limits also the usage of the pi hardware. It is know that the Pi 1 and Zero 1 may not be working with the latest cbpi4 versions as some required packages are not compatible with the 32 bit version of raspbian. The Raspberry Pi Zero 2 has been tested successfully as it is based on a quad core 64 bit ARM core. The Zero 2 is capable to run cbpi4 if you don't want to run Chromium directly on it.  
{% endhint %}

You should enable ssh to run commands also via a ssh connection from a remote PC / MAC. Open either a bash window or use a terminal to login via a remote ssh connection to your pi. The default user name is 'pi' and the default password is 'raspberry'. For safety reasons, you should change your password. This can be done with the RaspberryPi imager in the advanced options that can be accessed with CRTL + SHIFT + X.

Once the image has been written to the SD card, you need to place the card in your Pi and start the system. The first boot will take some time as the system will expand the image and needs to boot several times.

Now you should update your OS. Therefore, you need to enter a few commands. These commands need to be run as root. Therefore, you need to place 'sudo' in front of the commands. Open a bash window or connect via ssh to your raspberry.

```
sudo apt update
sudo apt upgrade
```

CraftbeerPi 4 requires another package that needs to be installed prior to the installation of the cbpi server. To install the package, please run the following command.

```
sudo apt install libatlas-base-dev libsystemd-dev
```

Starting with the lates raspbian os 'bookworm', python programs can't be directly installed with pip to protect the system from being corrupted by the installation of non managed packages via pip. Therefore you need to install pipx and install cbpi4 via pipx in a virtual environment that is being created automatically by pipx.

```
sudo apt install pipx
```
{% hint style="warning" %}
If you want to install pipx under bullseye, you might need to install python3-pipx instead of pipx
{% endhint %}

If you are using a Pi4 or even a lower Pi version you need to make sure, that the pre-installed RPi.GPIO package is not installed. This is not relevant for the Pi5.

Please run:

```
sudo apt remove python3-RPi.GPIO
```

to remove the package from your installation.

{% hint style="warning" %}
Starting with cbpi4 4.3.0, the RPi.GPIO package won't be used anymore as it is not compatible with th Pi5. A replacement package (rpi-lgpio) is used instead which can be used as a 1:1 replacement and the code of cbpi does not need to be changed. However, you can't have RPi.GPIO and rpi-lgpio installed in parallel as this will result in malfunction of all GPIO related functions.

This will be also relevant, if you upgrade from an older cbpi version which did install RPi.GPIO. In this case you need to ensure that RPi.GPIO is not installed on your system but also not in your virtual environment.

- You might need to remove the RPi.GPIO package from your virtual environment manually if you upgrade from 4.2.0:
``` 
pipx runpip cbpi4 uninstall RPi.GPIO
```
{% endhint %}


If you want to use hardware that is connected via I2C (e.g. LCD Display), you need to enable I2C in the raspi-config. You can also enable the onewire support in the raspi-config. Therefore, you need to run the command:

```
sudo raspi-config
```

This will open the following window.

![Raspi Config Menu](../.gitbook/assets/Raspi-config.png)

Select Interface options to enable I2C, Onewire support or whatever you need.

![Interface Options](<../.gitbook/assets/Interface Options.png>)

From the main menu you can also select system options to enable and configure Wifi.

![System Options](<../.gitbook/assets/System Options.png>) 

If you were using the full image of raspbian, the next steps are not required, as the packages are already included. On the lite image, you need to install a couple of packages, before you can install the server and user interface. Please run the following command to install the python3 package manager:

```
sudo apt-get install python3-pip
```

Now you can start installing the cbpi server.

### Installation of CraftbeerPi 4

The installation of CraftbeerPi4 is done with the package installer for python (pip3). You can install the server directly from pypi.org with the following command:

```
pipx install --system-site-packages cbpi4
```

Now you need to add the path of the cbpi installation to your path:

```
pipx ensurepath
```

This will allow you to run cbpi directly from the command line. You might need to close the terminal and reopen it again that the added path will become effective.

To install craftbeerpi4 from the repo which can be newer once in a while, please run the following command:

```
pipx install --system-site-packages https://github.com/PiBrewing/craftbeerpi4/archive/master.zip
```

The installation will take some time and it will also install the user interface. 


To upgrade the user interface or to install plugins, you can use the following command:

```
pipx runpip cbpi4 install cbpi4gui
```

In this case cbpi4gui is the name of the plugin which is available as package on pypi.org. This might not be the case for all plugins as not all developers make the plugins available via pypi.org.

In this case, you can also install packages from github:


```
pipx runpip cbpi4 install https://github.com/PiBrewing/craftbeerpi4-ui/archive/main.zip
```

{% hint style="warning" %}
During installation, the send2trash package may raise a warning:
```
WARNING: Error parsing dependencies of send2trash: Expected matching RIGHT_PARENTHESIS for LEFT_PARENTHESIS, after version specifier
sys-platform (=="darwin") ; extra == 'objc'
```
You don't need to worry about it. This is not related to cbpi directly. However, if you want to get rid of the warning, simply install a newer version of the send2trash package in your virtual environment:

```
pipx runpip cbpi4 install send2trash==1.8.3
```
{% endhint %}


In rare cases you may need to go into the virtual environment to install or remove plugins. This can be done with the following commands:

You need to activate the virtual environment first:

```
source ~/.local/pipx/venvs/cbpi4/bin/activate
```

If you have installed cbpi under the 'pi' user, you will see something like this:

```
(cbpi4) pi@craftbeerpi:~ $
```

This means, you are now in the virtual cbpi4 environment 

To upgrade the user interface now from the virtual environment, just run:

```
python -m pip install --upgrade cbpi4gui
```

There could be a newer version of the user interface in the repo and you could install this also directly from the repo.

```
python -m pip install --upgrade https://github.com/PiBrewing/craftbeerpi4-ui/archive/main.zip
```

{% hint style="info" %}
There are also development branches of some repos. These branches will be used to test some new features before the code is rolled out to the master or main branch. These branches are typically working, but should not necessarily considered as stable. The installation of these branches will be described in the [development section](development.md#running-a-development-version-of-the-server).
{% endhint %}

To get back from the virtual environment to the normal environment, just type:

```
deactivate
```

Now you need to setup cbpi to create a config folder:

```
cbpi setup
```

This will show you the following output:

```
Setting up CraftBeerPi 
Folder created 
Config Folder created
```

Then you can try to start cbpi the first time manually:

```
cbpi start
```

You should see the following output in your bash window:

```
START
2021-10-14:22:25:48,300 INFO     [craftbeerpi.py:89] Init CraftBeerPI
2021-10-14:22:25:48,400 INFO     [eventbus.py:56] Topic #
2021-10-14:22:25:48,836 INFO     [craftbeerpi.py:245]
  _____            __ _   ____                 _____ _
 / ____|          / _| | |  _ \               |  __ (_)
| |     _ __ __ _| |_| |_| |_) | ___  ___ _ __| |__) |
| |    | '__/ _` |  _| __|  _ < / _ \/ _ \ '__|  ___/ |
| |____| | | (_| | | | |_| |_) |  __/  __/ |  | |   | |
 \_____|_|  \__,_|_|  \__|____/ \___|\___|_|  |_|   |_|


 _  _    ___   ___   ____   ___
| || |  / _ \ / _ \ |___ \ / _ \
| || |_| | | | | | |  __) | (_) |
|__   _| | | | | | | |__ < \__, |
   | |_| |_| | |_| | ___) |  / /
   |_(_)\___(_)___(_)____/  /_/



2021-10-14:22:25:48,837 INFO     [craftbeerpi.py:246] www.CraftBeerPi.com
2021-10-14:22:25:48,837 INFO     [craftbeerpi.py:247] (c) 2021 Manuel Fritsch
2021-10-14:22:25:48,844 INFO     [plugin_controller.py:30] Trying to load plugin dummyactor
2021-10-14:22:25:48,898 INFO     [plugin_controller.py:30] Trying to load plugin gpioactor
2021-10-14:22:25:48,916 INFO     [plugin_controller.py:30] Trying to load plugin mashstep
2021-10-14:22:25:48,921 INFO     [plugin_controller.py:30] Trying to load plugin onewire
2021-10-14:22:25:48,959 INFO     [plugin_controller.py:30] Trying to load plugin httpsensor
2021-10-14:22:25:48,967 INFO     [plugin_controller.py:30] Trying to load plugin mqtt_sensor
2021-10-14:22:25:48,970 INFO     [plugin_controller.py:30] Trying to load plugin ConfigUpdate
2021-10-14:22:25:48,973 INFO     [plugin_controller.py:30] Trying to load plugin hysteresis
2021-10-14:22:25:48,977 INFO     [plugin_controller.py:30] Trying to load plugin dummysensor
2021-10-14:22:25:48,979 INFO     [plugin_controller.py:52] Try to load plugin:  cbpi4ui
2021-10-14:22:25:48,982 INFO     [plugin_controller.py:56] Plugin cbpi4ui loaded successfully
2021-10-14:22:25:48,982 INFO     [basic_controller2.py:36] SensorController Load
2021-10-14:22:25:48,983 INFO     [step_controller.py:30] INIT STEP Controller
2021-10-14:22:25:48,983 INFO     [basic_controller2.py:36] ActorController Load
2021-10-14:22:25:48,984 INFO     [basic_controller2.py:36] KettleController Load
2021-10-14:22:25:49,430 INFO     [__init__.py:21] Check Config for required changes
2021-10-14:22:25:49,430 INFO     [__init__.py:47] INIT Cooldown Sensor Setting
2021-10-14:22:25:49,432 INFO     [__init__.py:96] INIT Max Dashboard Numbers for multiple dashboards
2021-10-14:22:25:49,439 INFO     [__init__.py:113] INIT Current Dashboard Number
2021-10-14:22:25:49,443 INFO     [__init__.py:144] INIT Brewfather User ID
2021-10-14:22:25:49,448 INFO     [__init__.py:153] INIT Brewfather API Key
2021-10-14:22:25:49,461 INFO     [__init__.py:162] INIT Recipe Creation Path
2021-10-14:22:25:49,465 INFO     [__init__.py:171] INIT BoilKettle
======== Running on http://0.0.0.0:8000 ========
(Press CTRL+C to quit)

```

Now try to access cbpi via the web browser on your PI or on external client systems, like other computers or mobile devices.  Therefore, enter in your browser:

> IPADDRESSOFYOURPI:8000 -> e.g. 192.168.10.100:8000

If you see the empty dashboard of Craftbeerpi4, you were successful. Now you can go back to the bash window and press CTRL + C to stop the server.

## Automatically start the server as service

If you want to autostart the server when the pi is booting, you can enable that with the following command:

```
cbpi autostart on
```

The following output should be on the bash screen:

```
Add craftbeerpi.service to systemd
Copied craftbeerpi.service to /etc/systemd/system
Created symlink /etc/systemd/system/multi-user.target.wants/craftbeerpi.service → /etc/systemd/system/craftbeerpi.service.
Enabled craftbeerpi service
Started craftbeerpi.service
```

{% hint style="warning" %}
The craftbeerpi.service file is located in your config directory and configured for the user pi. If you have installed cbpi under a different user or did run the setup in a different folder than /home/pi you need to change the craftbeerpi.service file manually.

`[Service]`

`WokingDirectory=/home/pi`

Just replace `/home/pi` with the path where your config folder is located
{% endhint %}



If you want to remove the autostart during boot, simply run this command:

```
cbpi autostart off
```

It will take some time to stop the server and finally you will see this output:

```
Remove craftbeerpi.service from systemd
Stopped craftbeerpi service
Removed /etc/systemd/system/multi-user.target.wants/craftbeerpi.service.
Removed craftbeerpi.service as service
Deleted craftbeerpi.service from /etc/systemd/system

```

To see the status of cbpi autostart, you can run the following command:

```
cbpi autostart status
```

If you want to stop the server (running as service) for some reason (e.g. debugging), you can stop the server with the following command:

```
sudo systemctl stop craftbeerpi.service
```

{% hint style="info" %}
You need patience as this will take some time.
{% endhint %}

Then you can start the server in manual mode to see the logging and you can stop it again with CTRL + C:

```
cbpi start
```

{% hint style="info" %}
With recent version of cbpi4, the default logging is done only for warnings and errors. If you want to get a more detailed log, you need to add a parameter to the start.
```
cbpi -d 20 start
```
This will increase the log level to 'info' if you run cbpi in manual mode

With cbpi version 4.2.0 you can also modify the config.yaml file in our config folder and add `debug-log-level: 20` to set log level to 'info'. Just change the parameter to 30 and log level is set back to 'warning'. This will enable different log-levels from info also in server automode.

{% endhint %}

To restart the server as service you can either reboot or just start it as service directly:

```
sudo systemctl start craftbeerpi.service
```

## Automatically start chromium in kiosk mode at startup

You can enable chromium kiosk mode via command line. If this is enabled, chromium will start after boot in kiosk mode with craftbeerpi4.

{% hint style="info" %}
This has been tested on the full raspbian version where chromium is installed as default. For other setups you may need to install the chromium browser manually.
{% endhint %}

{% hint style="warning" %}
With Chromium 98 on bullseye (32 and 64 bit), Chromium might start with a white screen and won't load the server on startup. If you experience this issue you need to disable hardware acceleration in Chromium settings.
{% endhint %}

To enable kiosk mode, you need to run the following command from the bash:

```
cbpi chromium on
```

You will see the following output that the required file has been copied to the autostart folder:

```
Add chromium.desktop to /etc/xdg/autostart/
Copied chromium.desktop to /etc/xdg/autostart/
```

You can also disable the kiosk mode via command line. Therefore, you need to run the following command:

```
cbpi chromium off
```

You will see the following output that the file has been removed from the autostart folder:

```
Remove chromium.desktop from /etc/xdg/autostart/
Deleted chromium.desktop from /etc/xdg/autostart/
```

You can also see the status for Chromium kiosk mode if you run the following command:

```
cbpi chromium status
```

## Installation of Craftbeerpi 4 from a pre-configured image to your sd-card

There is also the possibility to write an image with a pre-installed CraftbeerPi4 server to your sd-card. This image comes with several installed plugins.

The image can be downloaded from this [link](https://www.slammy.net/homebrewing/2024_10_Carftbeerpi_4_4_6.zip) which is kindly provided by Nicolas Slammy Outrey.

{% hint style="warning" %}
The latest image is now based on bookworm os which requires some changes in the installation process compared to earlier images. Please read the installation instructions carefully. You will need to use an imager software [e.g. Win 32 Disk Imager](https://win32diskimager.org/) to write the image to a SD card. Please do not use te Raspi Imager anymore as it will remove some default settings from the pre-configured image.

Information on how to enable Wifi is described later. This MUST be done BEFORE you insert the SD card into your Pi and start it.

The image is based on Bookworm 64 bit nad has been created on a Pi4. You can't run it on Pi hardware that is not capable to run 64 bit OS (e.g. Pi1, Zero 1,...)
It has also not been tested on a Pi5 as the original image has been explicitly downloaded for a Pi4. However, it should be also working on a Pi5
{% endhint %}

Current Version:

```
Server Version: 4.4.6
UI Version: 0.3.17.1
--------------------------------------
 List of active plugins

 Name                            Version    Author                               Homepage                                                     Summary
------------------------------  ---------  -----------------------------------  -----------------------------------------------------------  ------------------------------------------------------------------------------
cbpi4-BLEHydrom                 0.0.8      Alexander Vollkopf                   https://github.com/avollkopf/cbpi4-BLEHydrom                 CraftBeerPi4 Plugin for Hydrom and Tilt (BLE connection)
cbpi4-BM_PID_SmartBoilWithPump  0.1.7      ['Alexander Vollkopf', 'Guy Lev']    https://github.com/avollkopf/cbpi4-BM_PID_SmartBoilWithPump  CraftBeerPi4 PID Kettle Logic Plugin
cbpi4-DependentActor            0.0.5      Alexander Vollkopf                   https://github.com/PiBrewing/cbpi4-DependentActor            CraftBeerPi4 Actor Plugin to create dependencies or conditions on other actors
cbpi4-Flowmeter                 0.0.6      Alexander Vollkopf                   https://github.com/PiBrewing/cbpi4-Flowmeter                 CraftBeerPi4 Flowsensor / Step Plugin
cbpi4-GembirdUSB                0.0.2      Alexander Vollkopf                   https://github.com/avollkopf/cbpi4-GembirdUSB                CraftBeerPi4 Plugin for Gembird USB Power Plug
cbpi4-GroupedActor              0.0.5      Alexander VOllkopf                   https://github.com/avollkopf/cbpi4-GroupedActor              CraftBeerPi Plugin
cbpi4-GroupedPowerActor         0.0.6      Alexander Vollkopf                   https://github.com/avollkopf/cbpi4-GroupedPowerActor         CraftBeerPi4 Plugin to Group Actors
cbpi4-KettleSensor              0.0.6      Alexander Vollkopf                   https://github.com/avollkopf/cbpi4-KettleSensor              CraftBeerPi4 Plugin to add Sensor parameters for your Kettle and Fermenter
cbpi4-LCDisplay                 0.0.13a7   Alexander Vollkopf                   https://github.com/PiBrewing/cbpi4-LCDisplay                 CraftBeerPi4 LCD Plugin Mod
cbpi4-PCF8574-GPIO              0.0.6      Alexander Vollkopf                   https://github.com/PiBrewing/cbpi4-PCF8574-GPIO              CraftBeerPi4 PCF8574 Actor Plugin
cbpi4-PIDBoil                   0.0.9      Alexander Vollkopf                   https://github.com/avollkopf/cbpi4-PIDBoil                   CraftBeerPi4 PID Kettle Control Plugin
cbpi4-PIDHerms                  0.0.4      Alexander Vollkopf                   https://github.com/avollkopf/cbpi4-PIDHerms                  CraftBeerPi 4 Kettle Logic Plugin
cbpi4-PID-AutoTune              0.0.10     Alexander Vollkopf                   https://github.com/avollkopf/cbpi4-PIDAutoTune               CraftBeerPi4 Kettle Logic for PID Auto Tune
cbpi4-PushOver                  0.0.8      Alexander Vollkopf                   https://github.com/PiBrewing/cbpi4-PushOver                  CraftBeerPi4 Plugin to forward Notifications to Pushover Push Notifications
cbpi4-buzzer                    0.0.8      Alexander Vollkopf                   https://github.com/PiBrewing/cbpi4-buzzer                    CraftBeerPi4 Buzzer Plugin
cbpi4-hx711-loadcell            0.1.0a4    Alexander Vollkopf                   https://github.com/PiBrewing/cbpi4-hx711-loadcell            CraftBeerPi 4 Plugin for hx711 loadcell
cbpi4-iSpindle                  0.0.13     Alexander Vollkopf                   https://github.com/avollkopf/cbpi4-iSpindle                  CraftBeerPi4 iSpindle Sensor Plugin
cbpi4-pt100x                    0.1.10     Alexander Vollkopf                   https://github.com/PiBrewing/cbpi4-pt100x                    CraftBeerPi4 PT100/PT1000 Sensor Plugin
cbpi4-scd30-CO2-Sensor          0.0.8      Alexander Vollkopf                   https://github.com/PiBrewing/cbpi4-scd30-co2-sensor          CraftBeerPi4 Plugin for SCD30 based CO2 Sensor
cbpi4-system                    0.0.9      Alexander Vollkopf                   https://github.com/PiBrewing/cbpi4-system                    CraftBeerPi4 Plugin for system fucntions
cbpi4_GroupedSensor             0.0.4      A. Vollkopf, B. Helm                 https://github.com/PiBrewing/cbpi4_GroupedSensor             CraftBeerPi4 Sensor Plugin
cbpi4_compressorActor           0.0.1      Pascal Scholz                        https://github.com/pascal1404/cbpi4_compressorActor          CraftBeerPi Plugin for Compressor actors
cbpi4gui                        0.3.17.1   Manuel Fritsch / Alexander Vollkopf  https://openbrewing.gitbook.io/craftbeerpi4_support/         CraftBeerPi4 User Interface

```

You can install further plugins as described in [this part](plugin-installation.md). But you can and should also deactivate plugins that you don't require.

#### The images has the following properties:

* Based on Raspian full 64 bit (bookworm)
* After some Time, the Pi will shut down and you need to remove the power and re-power the Pi.
* Image will expand to size of sd card (Please be aware, that the system wil start several times until it is up and running. This will take a couple of minutes)
* Server will autostart after boot.
* Wifi and Bluetooth are working (Please follow details below).
* SSH is enabled.
* Onewire support is enabled.
* I2C support is enabled.
* Installation is done under user 'Pi'
* mosquitto is installed and enabled with user: 'pi' and password 'cbpi4'

{% hint style="info" %}
User: pi

Password: raspberry

Please Note: Password can be changed when you write the image with the Raspberry Pi imager, but you cannot change the user since cbpi has been installed under user 'pi'

{% endhint %}

#### Installation:

1. Unzip the image. It will inflate to ~8 Gb
2. Write the image to a sd card with min size of 16 Gb with an imager [e.g. Win32 Diskimager](https://win32diskimager.org/). Do not use the Raspi Imager as this will change some of the pre-configured settings.
3. If you want to enable Wifi, you need to adapt the file firstrun.sh with an editor [e.g. Notepad++](https://notepad-plus-plus.org/downloads/) 
	- Enter your SSID, your credentials and your Wifi country at the corresponding settings.
```
SSID=YOUR_SSID
PSK=YOUR_PSK
COUNTRY=YOUR_COUNTRY
```
	- Save the file 
4. Insert the sd card into your raspberry and wait. 
   The device will boot several times and will shutdown (only the red led of the Pi will be on for some time w/o activity of the green led)
   Remove the power and re-power the Pi. It'll boot several times which will take a while.
5. Once the server is up and running, you can access it from a browser 
   via IP_OF_YOUR_PI:8000 or raspberrypi:8000
6. If you are using for instance a screen with your raspberry, you can also activate autostart of chromium with cbpi
   - Please refer to [Automatically start chromium](#automatically-start-chromium-in-kiosk-mode-at-startup)
   - On the next start, Chromium will be shown on start with the cbpi Dashboard.
   - It may also happen that you are not automatically logged into the desktop during startup.
   - In this case you need to start change this in the [settings](https://linuxconfig.org/how-to-set-user-autologin-on-raspberry-pi) 

## Updating your server and UI from the forks

### Updating the Server

If you want to update the server, you just need to run the same command as you did already for the installation of the server but should use the `upgrade` option:

```
pipx upgrade --system-site-packages cbpi4
```

From the repo you will need to use the install options with the force flag ad the upgrade option is not working for urls:

```
pipx install --force --system-site-packages https://github.com/PiBrewing/craftbeerpi4/archive/master.zip
```

If new setting parameters have been added to cbpi, it will handle that in the extension Config update. Cbpi4 will add the parameters automatically during start if they are not yet in the config file.

### Updating the UI

To update the user interface, you need to use the command 

```
pipx runpip cbpi4 install cbpi4gui
```

Or from the repo:

```
pipx runpip cbpi4 install https://github.com/PiBrewing/craftbeerpi4-ui/archive/main.zip
```

Alternatively, you can do this also by entering the virtual environment as described above.

## Other Hardware Tips

### Mute your buzzer at startup

If you are using a CraftbeerPi extension board that has a buzzer installed, the buzzer might play an annoying sound during startup. You can stop this by adding one line to the config.txt file in the boot directory. In case of the 4.0 extension board, the buzzer is connected to GPIO5. If your buzzer is connected to a different GPIO, you need to replace the 5 with the corresponding GPIO number. By adding this to the config .txt file, the GPIO will be set to out and low at boot and the signal will stop.

```
gpio=5=op,dl
```

### Display your GPIO numbers with wiringPi (most likely not working on the Pi 5)

Although, wiringpi is deprecated, it can be useful to get some insights into your pin numbering. CraftbeerPi 4 is using BCM numbers for the GPIOs and wiringpi can list them. Make sure, you have installed the latest (and last) version 2.52

to install wiringPi, open a bash window and run the following commands

```
cd /tmp
wget wget https://github.com/WiringPi/WiringPi/releases/download/3.10/wiringpi_3.10_arm64.deb
sudo dpkg -i wiringpi_3.10_arm64.deb
```

You can check the version with

```
gpio -v
```

To see the details for your hardware, just type&#x20;

```
gpio readall
```

This will show the following table

```
pi@raspberrypi:~ $ gpio readall
 +-----+-----+---------+------+---+---Pi 4B--+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 |     |     |    3.3v |      |   |  1 || 2  |   |      | 5v      |     |     |
 |   2 |   8 |   SDA.1 | ALT0 | 1 |  3 || 4  |   |      | 5v      |     |     |
 |   3 |   9 |   SCL.1 | ALT0 | 1 |  5 || 6  |   |      | 0v      |     |     |
 |   4 |   7 | GPIO. 7 |   IN | 1 |  7 || 8  | 1 | IN   | TxD     | 15  | 14  |
 |     |     |      0v |      |   |  9 || 10 | 1 | IN   | RxD     | 16  | 15  |
 |  17 |   0 | GPIO. 0 |  OUT | 1 | 11 || 12 | 0 | IN   | GPIO. 1 | 1   | 18  |
 |  27 |   2 | GPIO. 2 |   IN | 0 | 13 || 14 |   |      | 0v      |     |     |
 |  22 |   3 | GPIO. 3 |   IN | 0 | 15 || 16 | 0 | IN   | GPIO. 4 | 4   | 23  |
 |     |     |    3.3v |      |   | 17 || 18 | 0 | IN   | GPIO. 5 | 5   | 24  |
 |  10 |  12 |    MOSI |  OUT | 0 | 19 || 20 |   |      | 0v      |     |     |
 |   9 |  13 |    MISO |   IN | 1 | 21 || 22 | 0 | OUT  | GPIO. 6 | 6   | 25  |
 |  11 |  14 |    SCLK |  OUT | 0 | 23 || 24 | 0 | IN   | CE0     | 10  | 8   |
 |     |     |      0v |      |   | 25 || 26 | 0 | IN   | CE1     | 11  | 7   |
 |   0 |  30 |   SDA.0 |   IN | 1 | 27 || 28 | 1 | IN   | SCL.0   | 31  | 1   |
 |   5 |  21 | GPIO.21 |  OUT | 0 | 29 || 30 |   |      | 0v      |     |     |
 |   6 |  22 | GPIO.22 |  OUT | 0 | 31 || 32 | 0 | OUT  | GPIO.26 | 26  | 12  |
 |  13 |  23 | GPIO.23 |   IN | 0 | 33 || 34 |   |      | 0v      |     |     |
 |  19 |  24 | GPIO.24 |  OUT | 0 | 35 || 36 | 0 | OUT  | GPIO.27 | 27  | 16  |
 |  26 |  25 | GPIO.25 |   IN | 0 | 37 || 38 | 0 | OUT  | GPIO.28 | 28  | 20  |
 |     |     |      0v |      |   | 39 || 40 | 0 | IN   | GPIO.29 | 29  | 21  |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+---Pi 4B--+---+------+---------+-----+-----+

```

{% hint style="info" %}
CraftbeerPi 4 is using the BCM numbering for the GPIOs. Please use the numbers of the corresponding column for your GPIO assignment.&#x20;

The numbering could also vary depending on the Pi you are using.
{% endhint %}



## Alternative setups

### Docker

While CraftbeerPi is primarily created to be run on a RaspberryPi you can also use a docker image to run it. It can be found in the container registry on GitHub under the name `ghcr.io/pibrewing/craftbeerpi4`.

The image is currently only available for `arm64` and `amd64` architectures. If you want to run the image on a RaspberryPi make sure that you have installed a 64bit version of the operating system.

The image is available under several tags:

* `latest` the latest 'stable' release that is built from the master branch.
* `v4.x.x.x` image for a specific version.
* `dev` image for 'unstable' version built from the latest development branch.

{% hint style="warning" %}
Currently using the Raspberry PI GPIO does work by mounting the `dev/gpiomem` device to the container, but it is not officially supported.
{% endhint %}

#### Preparing configuration data

CraftBeerPi requires several configuration files in order to run correctly. Those files should not be located in the containers filesystem, because changes to those files would be lost in case the container is recreated (e.g. when upgrading to a new version).

It is advisable to create the initial configuration outside of the container and use a volume mount to make them available to the container when starting it.

```bash
# create a new config folder and set the correct owner group
mkdir config && chown :1000 config

# run a temporary CraftBeerPi container to write the initial
# configuration files in the new folder
docker run --rm -v "$(pwd)/config:/cbpi/config" ghcr.io/pibrewing/craftbeerpi4:latest cbpi setup
```

#### Running the base image

To run the image with the newly created configuration files you can use this command:

```bash
docker run -d -v "$(pwd)/config:/cbpi/config" -p 8000:8000 ghcr.io/pibrewing/craftbeerpi4:latest
```

#### Installing plugins

Installing plugins has a similar requirement as the configuration files. They should survive a container recreation. This is why it's not advisable to install them in a running container but create your own `Dockerfile` for it.

This `Dockerfile` can use the _official_ one as base and extends it by installing your own plugins.

{% code title="Dockerfile" %}
```docker
FROM ghcr.io/pibrewing/craftbeerpi4:latest

# Install plugins
RUN pip3 install --no-cache-dir cbpi4-pt100x 
    
```
{% endcode %}

Then build your own image.

```bash
docker build . -t craftbeerpiwithplugins:latest
```

**docker-compose**

Of course you can use `docker-compose` to setup your environment. You can also use your own image if you created one (see previous section).

{% code title="docker-compose.yml" %}
```yaml
version: "3.7"
services:
  craftbeerpi:
    image: ghcr.io/pibrewing/craftbeerpi4:latest
      volumes:
        - "./config:/cbpi/config"
      ports:
        - 8000:8000

      # When using RaspberryPi GPIO
      devices:
        - "/dev/gpiomem:/dev/gpiomem"
```
{% endcode %}

And then run it with

```bash
docker-compose up -d
```

{% hint style="warning" %}
For first time creating a docker image for V4.2.0 after running earlier versions, it is (as in a normal setup) not possible to use the config in the existing config directory.

Best practice:

create a backup.
Rename or delete the old config directory.
Create a new config directory and do all following steps as described in the docker section
run the clean image in docker
restore the backup
{% endhint %}
