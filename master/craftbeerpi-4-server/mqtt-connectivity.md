# MQTT connectivity

Craftbeerpi4 has the possibility to communicate with mqtt brookers/devices.

To activate the mqtt service within cbpi, you need to modify the config.yaml filw inside the config folder manually.

```
index_url: /cbpi_ui/static/index.html
mqtt: True
mqtt_host: IPofYourMQTTServer
mqtt_password: YourMQTTPassword
mqtt_port: 1883
mqtt_username: YourMQTTUser
name: CraftBeerPi
password: 123
plugins:
- cbpi4ui
- cbpi4-FermenterHysteresis
- cbpi4-BM_PID_SmartBoilWithPump
- cbpi4-PID_AutoTune
port: 8000
username: cbpi
version: 4.0.8

```

You need to set mqtt to True and adapt the other settings to your mqtt infrastructure. Once this has been changed and saved, you need to restart cbpi.

### Using CraftbeerPi to read / trigger external MQTT devices

When mqtt is enabled and running, you will see also a mqtt sensor in the sensor hardware.

![MQTT Sensor is available, when mqtt is activated in cbpi](../../.gitbook/assets/mqtt\_sensor.png)

You need to specify the topic and you can define a payload dictionary/filter. If the mqtt sensor is providing the sensor value e.g. as {"name":  "MyMQTTSensor" , "value": 123.4} you should enter value into the dictionary to filter out the sensor value.

In addition to the sensor, a mqtt actor is also available, when mqtt is activated.

![MQTT actor is available, when mqtt is activated in cbpi](../../.gitbook/assets/mqtt\_actor.png)

Switching the actor on will send the following value to mqtt under actor/speidelheat:

```
{
  "state": "on",
  "power": 100
}
```

Youi can also change the power via the actor button actions menu for the mqtt actor which will change the power value of the json string:

```
{
  "state": "on",
  "power": 70
}
```

{% hint style="info" %}
The power of the mqtt actor can be changed while it is on or off. The power value will be always updated via mqtt, but the state of the actor will remain unchanged (on or off).
{% endhint %}

The existing kettle logic plugins (eg. PIDBoil) will also work with mqtt actors and sensors. They will set the power for the mqtt actor.

### Other MQTT Actor types

In case want to switch other types of devices with CraftBeerPi via MQTT that expect a different message format you can choose other actor types that may be a better fit.

#### MQTT Actor (Tasmota)

To easily switch Tasmota devices on or off, there is a specific actor for this in CraftBeerPi. It just sends `on` or `off` to the specified topic. Use the `MQTT Actor (Tasmota)` if you want this behaviour.

#### MQTT Actor (Generic)

There may be other devices that expect other message formats to be switched on or off. In those cases you can use the `MQTT Actor (Generic)` to use a custom message format, that contains placeholders that are replaced with actual values before sending the message.

The available placeholders are:

| Placeholder      | replaced with (on)       | replaced with (off)      |
| ---------------- | ------------------------ | ------------------------ |
| `{switch_onoff}` | `on`                     | `off`                    |
| `{switch_10}`    | `1`                      | `0`                      |
| `{power}`        | power level (e.g. `100`) | power level (e.g. `100`) |

The message template uses standard python string formatting to replace the values so if your message template should contain curly braces `{}` for example to create a json message template, you need to escape them with double braces. So to get one `{` you need to write `{{` in the message template.

| Message template                                                     | after replace (on)                                     |
| -------------------------------------------------------------------- | ------------------------------------------------------ |
| `SET someswitch={switch_onoff}`                                      | `SET someswitch=on`                                    |
| `{{"name": "some json template", "switch_state": "{switch_onoff}"}}` | `{"name": "some json template", "switch_state": "on"}` |

### Trigger CraftbeerPi actors via MQTT

You also can change the status of CraftbeerPi actors via mqtt. There are currently three different mqtt 'commands' for an actor you can send to Craftbeerpi:

1. Switch an Actor on
2. Switch an Actor off
3. Set the power for an Actor

To switch a CraftbeerPi Actor on, you just need to publish a topic via your mqtt brooker:

```
cbpi/actor/QD7WjbFHWyGKMCCFai4uSW/on
```

This will switch actor `QD7WjbFHWyGKMCCFai4uSW` on. If you want to switch the actor off, you need to replace the `on` with an `off`:

```
cbpi/actor/QD7WjbFHWyGKMCCFai4uSW/off
```

If you want to change the power for an actor, the `on` or `off` needs to be replaced with `power`:

```
actor/QD7WjbFHWyGKMCCFai4uSW/power
```

You also need to send a power value between 0 and 100 as raw value in the payload. This will set the power for actor `QD7WjbFHWyGKMCCFai4uSW` to the desired value.

### CraftbeerPi is continuously updating all sensors and actors

Craftbeerpi is continuously sending status updates on all sensors and actors via mqtt

Sensordata is updated via:

```
cbpi/sensordata/2Sn46Fc6jRGvES9MuQXHdG
```

Each sensor is updated as separate topic where only the sensor ID is varying

This is an example for the cbpi sensor `2Sn46Fc6jRGvES9MuQXHdG`. The payload for a sensor update has the following content:

```
{
  "id": "2Sn46Fc6jRGvES9MuQXHdG",
  "value": 19.72
}
```

Status of actors are shown in this topic:

```
cbpi/actorupdate/QD7WjbFHWyGKMCCFai4uSW
```

Also here, each actor has it's own topic where only the actor id is different. The payload shows the current status of the actor:

```
{
  "id": "QD7WjbFHWyGKMCCFai4uSW",
  "name": "BM_Pump",
  "type": "GPIOActor",
  "props": {
    "GPIO": 25,
    "Inverted": "No"
  },
  "state2": "HELLO WORLD",
  "state": true,
  "power": 100
}
```

'state' represents the status of the actor. 'true' means that the actor is on, while 'false' represents the off status of an actor. 'power' shows the current power setting of the actor in %

Kettles can be seen under:

```
cbpi/kettleupdate/{kettleid}
''' 

for each kettle.

Fermenters are using this topic:

```
cbpi/fermenterupdate/{fermenterid}
''' 

Step information is updated under this topic:

```
cbpi/stepupdate/{stepid}
''' 

and notifications the the topic:

```
cbpi/notification
''' 

{% hint style="info" %}
Some things had to be changed as the payload size was just too large for smaller devices / libraries. Therefore, each sensor, actor, kettle, fermenter,.... has on topic under a main topic for sensorupdates, actorupdates, kettleupdates,...
This has been tested extensively with the so called [MQTTDeviceV2](https://innuendopi.github.io/MQTTDevice2/) but also [V4](https://innuendopi.github.io/MQTTDevice4/).
{% endhint %}

