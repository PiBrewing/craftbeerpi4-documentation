# Example System: MQTTDevice V4 based Fermentation system

- This setup is based on the MQTTDevice V4 which is using an ESP8266 as central unit. The ESP has to be flashed with Innuendos [firmware](https://innuendopi.github.io/MQTTDevice4/). With this setup, you can read the sensors hooked up to the MQTTdevice via the mqtt sensor included in cbpi and you can control the heater / cooler for eacxh femrenter via standard cbpi mqtt actors.

## Hardware requirements 

-  It is recommended to use the PCB described in the aforementioned link. 
- You will also need to connect DS18B20 Onewire sensors (one per fermenter) for temperature measurement to the device.
- 2 Relais (5 Volt) per Fermenter are required (one for cooling, one for heating per fermenter)
- One 5 Volt supply unit for the ESP8266 is required.
- Plug connectors (in my case 230 Volt) are required.
- Connectors for the Sensors are recommended.
- A drip water safe case.
- Magnetic valves to open and close the cooling loop for the fermenter
- Heating pad (25 Watts for 20-30 Liter Fermenter is sufficient). This may vary for your fermenters.
- One Chiller (in my example it is a Lindr AS-40 Glycol which has the advantage that you won't need a bypass valve in case the cooling valves are all closed) 
- Thats about it for the hardware part.

The image below shows the drip water safe case from the outside:

<img src="../../.gitbook/assets/cbpi4-mqttfermenter-mqttdevicebox.jpg" height="600" alt="Drip water safe box">
<img src="../../.gitbook/assets/cbpi4-mqttfermenter-mqttdevice-inside.jpg" height="600" alt="Box inside">

The next image shows the box from the inside equipped with the ESP8266, the power supply and the relais.




{% hint style="warning" %}
Only the pump plug has a ground connection on the Braumeister side. I strongly recommend to add a ground connection from the heater plug (Braumeister side) to the Braumeister Kettle (Yellow/Green Cable in the image above)! You will also need to ensure ground connection from your controller side!
{% endhint %}

### Optional Hardware requirement
- You can add a magnetic valve to the inlet of your colling jacket as this can be triggered during the cooldown step. It'll open, if you configure a corresponding actor as cooldown valve and set it up in your settings. When the cooldown steps starts, the valve opens and your cooling water starts to flow until your pre-configured temprature is reached. Then the valve is closing. An example picture is shwon below:

![Cooldown Valve](../../.gitbook/assets/cbpi4-Example1-Cooldown-Valve.jpg)

- In particular the older Braumeister models have some issues with the accurate temperature readings when cooling down your wort. Theerefore, I recommend to add another temp sensor (e.g. OneWire) to your braumeister during cooldown. The second sensor for cooldown  can be configured in the CraftbeerPi 4 settings. You just need a thermowell and some parts to fix your thermowell and sensor to the middle column of your Braumeister prior ro the cooldown step. 

An example for the parts is shown here:

![Thermowell parts](../../.gitbook/assets/cbpi4-Example1-Thermowell-parts.jpg)

The placement of the Thermowell with Sensor after cooldown is shown here:

![Cooldown Example](../../.gitbook/assets/cbpi4-Example1-CooldownProbe.jpg)

## CraftbeerPi 4 software requirements
- You need an installation of Craftbeerpi4 with some additional plugins.
- You will need the [**cbpi4-pt100X plugin**](https://github.com/PiBrewing/cbpi4-pt100x) to read the temeprature values from the PT1000 and configure it to PT1000.
- You will also need a [**logic plugin**](https://github.com/PiBrewing/cbpi4-BM_PID_SmartBoilWithPump) that covers pump pause, pump stop @ 88°C, PID temperature control and much more 
- The PID settings have to be optimized for your kettle with the [**PIDAutotune**](https://github.com/PiBrewing/cbpi4-PID_AutoTune) plugin.
- PID control switches off at 88°C and boiling will be done with reduced heater power which can be defined in the logic settings (mine is running at 85% power during boil)
- I do recommend to install and use also the [**Pushover Plugin**](https://github.com/PiBrewing/cbpi4-PushOver) to recieve push notifications when you need to add or remove the malt pipe or add hops. Therefore, you need to buy the [**PushOver APP**](https://pushover.net/) for Android or IOS
- I also recommend to install the [**Kettle Sensor Plugin**](https://github.com/PiBrewing/cbpi4-KettleSensor) if you want to monitor some more information.
- If you have a buzzer connected to your system or if you are using an extension board with a buzzer, you should install and configure the [**Buzzer plugin**](https://github.com/PiBrewing/cbpi4-buzzer) accordingly


## Setup your Braumeister hardware in CraftbeerPi 4

{% hint style="info" %}
Details on how to setup your software are not ashown here, as this is already described in the other chapters. GPIO settings depend on your individual controller build. ALso the inverted setting depends on your controller hardware configuration.
{% endhint %}

### Actors
First you should define your actors for the Braumeister. You need one for the heating element and one for the pump. In casde you want to run a magnetic valve for the automated cooldown, You need to add another actor for the magnetic valve.

![Braumeister Actors](../../.gitbook/assets/cbpi4-Example1-BM-Actors.png)

For the heating element you can use a PWM type actor. As frequency, you can choose something between 0.1 and 0.5 Hz. Higher frequencies are not recommended and can cause also issues in your house (e.g. flickering lights). To be on the safe side, choos 0.1 Hz.

![Heating Actor](../../.gitbook/assets/cbpi4-Example1-BM-Actor-Heat.png)

For the pump select a regular GPIOActor as it will be only switched on or off once in a while. You can leave the sampling time empty as it is not relevant for this actor.

![Pump Actor](../../.gitbook/assets/cbpi4-Example1-BM-Actor-Pump.png)

In case you are using a cooldown valve, you can configure it in the same way as the pump.

### Sensors
Now you need to define your sensors. As mentioned, the Braumeister comes with a 2 wire PT1000. As mentioned above, you need to have a corresponding board connected to your Pi and you need to install the required plugin as mentoined above.

![PT1000 Sensor](../../.gitbook/assets/cbpi4-Example1-BM-Sensor_PT1000.png)

Configure your sensor and coose 1000 as resistivity value and 4300 as resistivity for your reference sensor (You need to buy the correct max board for the PT1000 and not one for a PT100). Choose 2 or 4 wires in the sensor settings. The other parameters are described in detail on the plugin page.

If you want to use an additional one wire sensor for cooldown, you need to set up this sensor as well. Below is an example:

![Cooldown Sensor](../../.gitbook/assets/cbpi4-Example1-BM-Sensor_cooldown.png)

The advantage is that you measure a more realistic temeprature of your wort during cooldown as the PT1000 sensor is covered and 'isolated' with trub. Below you can see the difference for the PT1000 and the Cooldown sensor.

![Cooldown Sensor Data](../../.gitbook/assets/cbpi4-Example1-BM-CooldownSensor.png)

Optionally, you can also install the KettleSensor plugin and add two additional virtual sensors for your Braumeister: Target Temperature and Power. These sensors can be used to display both parameters.

![TargetTemp Sensor](../../.gitbook/assets/cbpi4-Example1-BM-Sensor_TargetTemp.png)
![Power Sensor](../../.gitbook/assets/cbpi4-Example1-BM-Sensor_Power.png)

In case you are using influxdb and Grafana integration, you can display the relevant parameters for your brewing session including the target temperatures, the heating power and the measured temepratures.

![Brewing Parameters](../../.gitbook/assets/cbpi4-Example1-BM_Brewing.png)


### Kettle configuration

Finally, you need to define a Kettle with the required logic plugin mentioned above. Select the installed BM_PID_SmartBilWithPump plugin as logic and define a Name for your Kettle. Select the Actor you have defined as Hater for your Kettle Heater and the Pump actor as Agitator. As sensor, select the PT1000 sensor you have defined. The PID parameters need to be derived via the PID Autotune Plugin. Details on how to run the tunig are described [here](https://github.com/PiBrewing/cbpi4-PID_AutoTune).

The max Pump temperature defines, when the pump will be switched off to prevent pump failure. 88C is recommended and alinged with the original Braumeister controller settings. The may bvoil output defines the power that is used for boiling. For my Braumeister 20 L , 85% is suffucuent, but you can adat this to your needs for a good boil. Max Boil Temp is the temeprature, when the max boil output will be used. Max PID temp is the temperature, until the PID settings are used. Above this temeperature, the max output is used. The rest intervall is the intervall when the pump is paused. Default is 600 (= 10 Minutes like in te braumeister controller). The Rest time is the time in seconds for how long the pump is resting. The standard Braumeister setting is 1 Minute.

![Braumeister Kettle](../../.gitbook/assets/cbpi4-Example1-BM-Kettle-Config.png)

### Global CraftbeerPi4 server settings

Finally, you need to controll / adapt some settings on the server settings page.

You need to set the 'Add Mashin Step' setting to yes for automated recipe creation from Kleiner Brauhelfer, MuMM or Brewfather. In this case, the system will add a first step with a target temp of the mash step and holds the system once the target temp is reached anbd stops the pump and heating. It'll will notify you to add the malt pip/malt and hit next

You should also enable the automode to start and stop the logic automatically after each step.

![Global Settings 1](../../.gitbook/assets/cbpi4-Example1-BM-Settings-1.png)

You don'T need to select a kettle for the boilkettle as your Braumeister is a one Kettle system. But it would not hurt, if you would select the Braumeister as Boilkettle.

![Global Settings 2](../../.gitbook/assets/cbpi4-Example1-BM-Settings-2.png)

For the Mash_Tun you need to select your Braumeister Kettle.

![Global Settings 3](../../.gitbook/assets/cbpi4-Example1-BM-Settings-3.png)

In the steps settings, you need to select the correspondign steps you want to run for your device (You could also create your own steps via a plugin). 

- Boil should be clear.
- The boil temp can be defined and is filled in for the boilstep during the automated recipe creation.
- If you are using a cooldown sensor, select your sensr here. Otherwise the default kettle sensor will be used if no sensor is selected.
- If you are using a colldown valve, you need to select the corresponding actor that will be used for this step.
- The mash step should be also clear as well as the nashin step.

![Global Settings 4](../../.gitbook/assets/cbpi4-Example1-BM-Settings-4.png)

{% hint style="info" %}
Details for all steps are also described in the corresponding section of this documentation
{% endhint %}