# Server Installation

## How to install Craftbeerpi4 and it's user interface

First you will need to install Raspbian from an image. The installer can be found here: [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/)

You should enable ssh to run commands also via a ssh connection from a remote PC. Open either a bash window or login via a ssh connection to your pi. The default user name is 'pi' and the default password is 'raspberry'. For safety reasons, you should change your password. This can be done with the RaspberryPi imager in the advanced options that can be accessed with CRTL + SHIFT + X.

Once the image has been written to the SD card, you need to place the card in your Pi and start the system. The first boot will take some time as the system will expand the image and needs to boot several times.

As a first step, you should update your os. Therefore, you need to enter a few commands. These commands need to be run as root. Therefore, you need to place 'sudo' in front of the commands. You need to open a bash window or connect via ssh to your raspberry.

```
sudo apt-get update
sudo apt-get upgrade
```

CraftbeerPi 4 requires another package that needs to be installed prior to the installation of the cbpi server. To install the package, please run the following command.

```
sudo apt-get install libatlas-base-dev
```

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



Now you can start installing the cbpi server. At this point of time, this guide will point to the forks of the server and user interface from Alexander Vollkopf, as they have the most recent code, fixes and features. You can find also plenty of plugins in the git space: [https://github.com/avollkopf?tab=repositories](https://github.com/avollkopf?tab=repositories).

To install craftbeerpi4 from the repo, please run the following command:

```
sudo pip3 install https://github.com/avollkopf/craftbeerpi4/archive/master.zip
```

The installation will take some time and it will also install the default user interface. However, to run the server with all the features, you will need to install also the latest user interface from the repo.

First you need to clone the repo to your local system:

```
git clone https://github.com/avollkopf/craftbeerpi4-ui/
```

Once cloned, you can install the user interface from the cloned directory:

```
sudo pip3 install ./craftbeerpi4-ui
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
sudo cbpi start
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

Now try to access cbpi via the webinterface.  Therefore, enter in your browser:

> IPADDRESSOFYOURPI:8000 -> e.g. 192.168.10.100:8000

If you see the empty dashboard of Craftbeerpi4, you were successful. Now you can go back to the bash window and press CTRL + C to stop the server.&#x20;

## Automatically start the server as service

If you want to autostart the server when the pi is booting, you can enable that with the following command:

```
sudo cbpi add autostart
```

The following output should be on the bash screen:

```
Add cradtbeerpi.service to systemd
Copied craftbeerpi.service to /etc/systemd/system
Created symlink /etc/systemd/system/multi-user.target.wants/craftbeerpi.service → /etc/systemd/system/craftbeerpi.service.
Enabled craftbeerpi service
Started craftbeerpi.service
```

If you want to remove the autostart during boot, simply run this command:

```
sudo cbpi remove autostart
```

It will take some time to stop the server and finally you will see this output:

```
Remove cradtbeerpi.service from systemd
Stopped craftbeerpi service
Removed /etc/systemd/system/multi-user.target.wants/craftbeerpi.service.
Removed craftbeerpi.service as service
Deleted craftbeerpi.service from /etc/systemd/system

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
sudo cbpi start
```

To restart the server as service you can either reboot or just start it as serivce directly:

```
sudo systemctl start craftbeerpi.service
```

## Updating your server and UI from the forks

### Updating the Server

If you want to update the server from my fork, you just need to run the same command as you did already for the installation of the server:

```
sudo pip3 install https://github.com/avollkopf/craftbeerpi4/archive/master.zip
```

If new settings parameters have been added to cbpi I will handle the in the extension Configupdate and cbpi4 will add the parameters automatically during start if they are not yet in the config file.

### Updating the UI

To update the user interface, you need to move into the directory of the cloned user interface and pull the new code with git:

```
cd craftbeerpi4-ui
git pull
```

Once this is done, move back up one directory and run again the command to install the user interface as done in the initial installation:

```
sudo pip3 install ./craftbeerpi4-ui
```

## Other Hardware Tips



If you are using a CraftbeerPi extension board that has a buzzer installed, the buzzer might play an annoying sound during startup. You can stop this be adding one line to the config.txt file in the boot directory. In case of the 4.0 extension board, the buzzer is connected to GPIO5. If your buzzer is connected to a different GPIO, you need to replace the 5 with the corresponding GPIO number. By adding this to the config .txt file, the GPIO will be set to out and low at boot and the signal will stop.

```
gpio=5=op,dl
```
