---
description: Information on how to install Plugins
---

# Plugin installation

## Plugin installation process

Craftbeerpi4 comes with very basic funstionality but has the possibility to use plugins provied by the community. Therefore, it has a huge flexibility as Sensors, Actors Controllers, Recipe creation plugins but also Push Messages and Displays can be added to each users need (The user just needs to write his plugin or needs to find someone who is providing it).

CraftbeerPi4 comes for isntance with a Onewire sensor, but some users prefer a PT100/PT1000 sensor.  Therefore, a variety of plugins is already available and below is an example on how to install a PT100/PT1000 sensor plugin.&#x20;

Some plugins have been made available via pypi.org and can be installed directly from there. Other plugins need to be installed directly from GitHub. The cbpi4-pt100X plugin is available on both platforms. Typically, the latest version is available on GitHub and will be released a bit later via pypi.org as package

Installation from pypi.org is quite simple. Just type the following command:

```
sudo pip3 install cbpi4-pt100x
```

The plugin and it's dependent packages will be installed on your system but still needs to be activated in cbpi.

The other way to install the plugin is directly from GItHub:

```
sudo pip3 install https://github.com/avollkopf/cbpi4-pt100x/archive/main.zip
```

Also here, the plugin and the dependent packages will be installed automatically.

Whether you have installed the plugin via pypi.org or from GitHub, you will need to activate the plugin afterwards in cbpi with the following command:

```
cbpi add cbpi4-pt100x
```

Once this is completed, you need to restart cbpi or reboot your server.

{% hint style="info" %}
To get some detailed information about the plugin configuration and how to connect/install your sensor, you should have always a look at the plugin page on GitHub or pypi.org. I always try to add the most important inforation in the correspondig README.

For the cbpi4-pt100x plugin, you can find the information [here](https://github.com/avollkopf/cbpi4-pt100x) on GitHub or you search for cbpi4-pt100x on the pypi.org site.&#x20;
{% endhint %}

## How to remove a plugin?

{% hint style="info" %}
If you want to remove a plugin you need to be carefull. Before you remove the plugin from the server, you need to ensure that the plugin is not in use in any of your Hardware items and the dashboard as this may cause errors and can prevent the server from starting
{% endhint %}

To remove or better disable the plugin from the cbpi server, just type in case of the cbpi4-pt100x plugin:

```
cbpi remove cbpi4-pt100x
```

## How to show the active plugins?

If you want to see the active plugins, you have two possibilities. The first option is from the command line. Just type:

```
sudo cbpi plugins
```

This will show you a list of the installed plugins incl. the version:

```
--------------------------------------
List of active plugins
- (0.0.19)      cbpi4ui
- (0.0.3)       cbpi4-BM_PID_SmartBoilWithPump
- (0.0.9)       cbpi4-BM_Steps
- (0.0.2)       cbpi4-buzzer
- (0.0.3)       cbpi4-iSpindle
- (0.0.4)       cbpi4-PID_AutoTune
- (0.0.10)      cbpi4-pt100x
- (0.0.3)       cbpi4-PushOver
- (0.0.2)       cbpi4-FermenterHysteresis
- (0.0.1)       LCDisplay
- (0.0.2)       cbpi4-system
- (0.0.1)       cbpi4-RecipeImport
- (0.0.1)       cbpi4-GroupedActor
- (0.0.1)       cbpi4-scd30_CO2_Sensor
- (0.0.1)       cbpi4-hx711-loadcell
- (0.0.1)       cbpi4-DependentActor
- (0.0.2)       cbpi4-PIDBoil
- (0.0.1)       cbpi4-GPIODependentActor
- (0.0.1)       cbpi4-PIDHerms
- (0.0.1)       cbpi4-Flowmeter
--------------------------------------

```

The second option is directly from the user interface. Just click on the menu button on the top left and select Plugins. This will show this page which is still in development. However, you can see the active plugins and if the developer has added the corresponding information to his plugin, you can go directly to the corresponding webpage of the Plugin.

![CraftbeerPi4 Plugin Page](../.gitbook/assets/cbpi4-Plugins.png)

## Plugin List

The tables below shows the plugins that are currently available by type. At this point of time I cannot guarantee that the list os complete as I can only add the plugins to the list I am aware of.

### Sensors

| Name                   | Description                                    | Link                                                               |
| ---------------------- | ---------------------------------------------- | ------------------------------------------------------------------ |
| cbpi4-pt100x           | PT100/PT1000 Temp Sensor                       | [GitHub Link](https://github.com/avollkopf/cbpi4-pt100x)           |
| cbpi4-hx711-loadcell   | hx711 based Loadcell Sensor                    | [GitHub Link](https://github.com/avollkopf/cbpi4-hx711-loadcell)   |
| cbpi4-Flowmeter        | Hall Sensor based Flowmeter Sensor             | [GitHub Link](https://github.com/avollkopf/cbpi4-Flowmeter)        |
| cbpi4-system           | System Sensors: Temp, CPU load, Memory         | [GitHub Link](https://github.com/avollkopf/cbpi4-system)           |
| cbpi4-scd30-co2-sensor | Temp, Rel. Humidity and CO2 sensor (I2C based) | [GitHub Link](https://github.com/avollkopf/cbpi4-scd30-co2-sensor) |
| cbpi4-iSpindle         | Sensor that collects data from the iSpindle    | [GitHub Link](https://github.com/avollkopf/cbpi4-iSpindle)         |

### Actors

| Name                     | Description                                                        | Link                                                                 |
| ------------------------ | ------------------------------------------------------------------ | -------------------------------------------------------------------- |
| cbpi4-GoupedActor        | Allows to group Actors                                             | [GitHub Link](https://github.com/avollkopf/cbpi4-GroupedActor)       |
| cpbi4-DependentActor     | Allows to switch actors dependent on the state of other actors     | [GitHub Link](https://github.com/avollkopf/cbpi4-DependentActor)     |
| cbpi4-GPIODependentActor | Allows to switch actors dependent on the state GPIO Inputs (alpha) | [GitHub Link](https://github.com/avollkopf/cbpi4-GPIODependentActor) |

### Kettle Controller

| Name                               | Description                                                                                                                                       | Link                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| cbpi4-PIDBoil                      | Kettle controller with PID logic and Boil power parameter                                                                                         | [GitHub Link](https://github.com/avollkopf/cbpi4-PIDBoil)                    |
| cbpi4-PIDHerms                     | Kettle controller with PID logic, Boil power parameter and HLT temp sensor condition                                                              | [GitHub Link](https://github.com/avollkopf/cbpi4-PIDHerms)                   |
| cbpi4-BM\__PID\__SmartboilWithPump | Kettle controller with PID logic, Boil power parameter and automated Pump control / protection (can be used for instance for Speidel Braumeister) | [GitHub Link](https://github.com/avollkopf/cbpi4-BM\_PID\_SmartBoilWithPump) |
| cbpi4-PID\_Autotune                | Kettle controller that can be used to determine the PID parameters for the PID based Kettle controllers                                           | [GitHub Link](https://github.com/avollkopf/cbpi4-PID\_AutoTune)              |

### Fermenter Controller

|                           | Description                                                | Link                                                                  |
| ------------------------- | ---------------------------------------------------------- | --------------------------------------------------------------------- |
| cbpi4-FermenterHysteresis | Kettle Controller that allows to use a Kettle as Fermenter | [GitHub Link](https://github.com/avollkopf/cbpi4-FermenterHysteresis) |

### Display

| Name                 | Description                                                                           | Link                                                          |
| -------------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| cbpi4-LCDisplay      | Allows usage of LCD Display (I2C)                                                     | [GitHub Link](https://github.com/JamFfm/cbpi4-LCDisplay)      |
| cbpi4-LCDisplay      | Modded Fork that allows also Display of Fermentation with Fermenter Hysteresis Plugin | [GitHub Link](https://github.com/avollkopf/cbpi4-LCDisplay)   |
| cbpi4-NEXTIONDisplay | Use Nextion Display on CraftbeerPi4                                                   | [GitHub Link](https://github.com/JamFfm/cbpi4-NEXTIONDisplay) |

### Utilities (Messaging, Custom Recipe Creation / Steps)

| Name               | Description                                              | Link                                                           |
| ------------------ | -------------------------------------------------------- | -------------------------------------------------------------- |
| cbpi4-BM\_Steps    | Example Plugin for custom Mash steps                     | [GitHub Link](https://github.com/avollkopf/cbpi4-BM\_Steps)    |
| cbpi4-RecipeImport | Example Plugin to customize automated recipe creation    | [GitHub Link](https://github.com/avollkopf/cbpi4-RecipeImport) |
| cbpi4-buzzer       | Activates buzzer on cbpi4 messages                       | [GitHub Link](https://github.com/avollkopf/cbpi4-buzzer)       |
| cbpi4-PushOver     | Forwards cbpi4 messages to Pushover push message service | [GitHub Link](https://github.com/avollkopf/cbpi4-PushOver)     |
