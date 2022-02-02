# Sensor Plugin Example

I want to show you some examples on how to write a sensor plugin. I will start with the example of the dummysensor which is included in the server itself and is very simple.

## Dummy sensor example

First, you will need to import the python packages you require for your plugin. As plenty of functions need to be processed asynchronus, you will always need to import asyncio. You will also need to import the CBPiSensor from the cbpi api.

```
# -*- coding: utf-8 -*-
import asyncio
import random

from cbpi.api import parameters, CBPiSensor
```

As already described in the [plugin properties section](plugin\_development.md#plugin-properties), you should specify sensor properties if required in the `@parameter` part. Then, you define your sensor class by using the `CBPiSensor` class type.

The first function inside the class is the initialization of the sensor. You need to pay attentino the the name of your sensor class does match the name in the super `function`. If you change the name for your sensor class, you also need to change the name insode the `super` function. It is required, to use unique names for your sensor plugins. Defining two different classes with the same name will cause issues.

```
@parameters([])
class CustomSensor(CBPiSensor):

    def __init__(self, cbpi, id, props):
        super(CustomSensor, self).__init__(cbpi, id, props)
        self.value = 0
```

The next part in the sensor plugin is the `run` function which is always required as it continuously reads the sensor values and can write them to the log file or sends them to the user interface or mqtt brookers. The function has to be asynchronous as the server should not stop other tasks and wait for the sensor reading.

The `while` loop will ensure, that data is being read continuously. Sensor values have to be stored in `self.value` as other fucntions will use that variable. In this example, a random value between 10 and 100 will be generated.

The `self.log_data(self.value)` function will log data to a csv log file and / or to an influxdb. This depends on how you [configured your server](../craftbeerpi-4-server/datalogging.md).

The `self.push_update(self.value)` function is updating the web interface as well as mqtt sensordata messages if mqtt is enabled. 

{% hint style="info" %} 
Typically you send the update to both, the user interface (or websocket) and mqtt which is the default for the `self.push_upate()` function in the sensor class.

However, for some plugins it might be usefull to update the user interface with a higher freqency than mqtt. This is for instance used in the iSpindle plugin, where new values are retrieved for instance only every 15 mninutes. In this case, the web interface would be also updated only every 15 minutes when a new value has been received. If you reload the webpage, the sensor would remain empty until the next value has been send. This is ok for mqtt, but not for the web interface.

To avoid this, you can add a `False` to the function which will only update the web interface but not mqtt: `self.push_update(self.value, False)`. This ensures a frequent update of the web interface but reduces mqtt traffic significantly. Details will be shown in the another example. 
{% endhint %}

The last function is really important and should not be forgotten. otherwise you may end up in a sever that does not respond dur to 100% CPU load. `await asyncio.sleep(1)` ensures, that the fucntion will wait 1 second until it starts over.

```
    async def run(self):
        while self.running:
            self.value = random.randint(10, 100)
            self.log_data(self.value)

            self.push_update(self.value)
            await asyncio.sleep(1)
```

The `get_state` function is also essential as it might be used by oter sever routines. It returns the current sensor value and should not be removed.

```
    def get_state(self):
        return dict(value=self.value)
```

Finally, the plugin needs to be registered as it has been already described in the [plugin development part](plugin\_development.md#plugin-registration)

```
def setup(cbpi):
    '''
    This method is called by the server during startup
    Here you need to register your plugins at the server
    :param cbpi: the cbpi core
    :return:
    '''
    cbpi.plugin.register("CustomSensor", CustomSensor)
```

## HTTP Sensor example (iSpindle)

This is a more complex example as the iSpindle is sending data via http. In addition to the, an external application can retrieve data from craftbeerpi. Therefore, two `http_endpoints` have to registred in this plugin - one to retrieve data and one to send data.

As more fucntions are required, you need to import also more libraries in the beginning of this plugin. In particular for the http communication you will need to import the `web` part from `aiohttp`, as well as the `json` library, since the data is transferred in the json format. The `logging` module is also used, as it can be really helpfull for debugging.

In the beginning a global variable `cache`is defined as dict. this is used by all parts of the plugin.

```
# -*- coding: utf-8 -*-
import os
import logging
import asyncio
from aiohttp import web
from cbpi.api import *
import re
import time
import json

logger = logging.getLogger(__name__)

cache = {}
```

You can also specify and use fucntions outside of your sensor class. In this case, the gravity will be calulated by this fucntion. The function is called by the sensor class itself and returns the calulated gravity.

```
async def calcGravity(polynom, tilt, unitsGravity):
	if unitsGravity == "SG":
		rounddec = 3
	else:
		rounddec = 2

	# Calculate gravity from polynomial
	tilt = float(tilt)
	result = eval(polynom)
	result = round(float(result),rounddec)
	return result
```


This plugin uses more properties for the configuration and assignment of the iSpindle data. The user can also enter a polynomial fucntion that is used for the calculation of the gravity. Therefore, the text property is used. In addition to that the user can select a sensor. Data from this sensor can be retrieved by external sources, whenever the send a http request to a specified http endpoint.

```
@parameters([Property.Text(label="iSpindle", configurable=True, description="Enter the name of your iSpindel"),
             Property.Select("Type", options=["Temperature", "Gravity/Angle", "Battery", "RSSI"], description="Select which type of data to register for this sensor. For Angle, Polynomial has to be left empty"),
             Property.Text(label="Polynomial", configurable=True, description="Enter your iSpindel polynomial. Use the variable tilt for the angle reading from iSpindel. Does not support ^ character."),
             Property.Select("Units", options=["SG", "Brix", "°P"], description="Displays gravity reading with this unit if the Data Type is set to Gravity. Does not convert between units, to do that modify your polynomial."),
             Property.Sensor("FermenterTemp",description="Select Fermenter Temp Sensor that you want to provide to TCP Server")])
```

Once the properties have been defined, it is mandatory to initialize the sensor plugin. In this step, you should also define your variables for the properites with the `self.props.get("PROPERTY", DEFAULT_VALUE)` method. With `PROPERTY`, you specify / access the property, you specified in the `@parameters`section. If the property has not been defined, or the user has not entered a value in the sensor setup page, you can / should specify a default value in the function which is used if no value has been entered for this parameter.

```
class iSpindle(CBPiSensor):
    
    def __init__(self, cbpi, id, props):
        super(iSpindle, self).__init__(cbpi, id, props)
        self.value = 0
        self.key = self.props.get("iSpindle", None)
        self.Polynomial = self.props.get("Polynomial", "tilt")
        self.temp_sensor_id = self.props.get("FermenterTemp", None)
        self.time_old = 0
```

You will also need to add the run function as this is the function that is 'reading' your sensor data and is pushsing the data to the web interface or mqtt with `self.push_update(self.value)`. It can also log the data with the fucntion `self.log_data(self.value)`. More details on pushing the data are described above.

This plugin is a bit more complex as it reads the sensor data from a cache which is updated by another function. A timestamp is updated in the cache, whenever the cache is filled with new values. The `run` function is checking, if the timestamp has been updated. Whenever this is the case, it reads the new sensor value and will push it to the web socket and mqtt with the `self.push_update(self.value)` function. In addition, the nw value will be logged.

If the timestamp has not been updated, the push_update function will be only called with a `False` -> `self.push_update(self.Value, False)`. In this case, only the web interface will be updated, but not the mqtt broker. This is usefull for sensors like the iSpindle, as they report new values only every 15 minutes or so. If an existing value is available, the web interface will be updated frequently. Otherwise, the user would not see the current vlaue when pages are switched.

```
    async def run(self):
        global cache
        global fermenter_temp
        Spindle_name = self.props.get("iSpindle") 
        while self.running == True:
            try:
                if (float(cache[self.key]['Time']) > float(self.time_old)):
                    self.time_old = float(cache[self.key]['Time'])
                    if self.props.get("Type") == "Gravity/Angle":
                        self.value = await calcGravity(self.Polynomial, cache[self.key]['Angle'], self.props.get("Units"))
                    else:
                        self.value = float(cache[self.key][self.props.Type])
                    self.log_data(self.value)
                    self.push_update(self.value)
                self.push_update(self.value,False)
                
            except Exception as e:
                pass
            await asyncio.sleep(2)

    def get_state(self):
        return dict(value=self.value)
```

As mentioned before, the data for this sensor will be retrieved via http and a cahce is being updated whenever a new value is send to the server. Therefore, a http endpoint needs to be registered in the cbpi server. This is done with another CBPiExtension class. Also in this case, the CBPiExtension classe needs to be registered in addition to your sensor class at the end of the plugin.

```
class iSpindleEndpoint(CBPiExtension):
    
    def __init__(self, cbpi):
        '''
        Initializer
        :param cbpi:
        '''
        self.pattern_check = re.compile("^[a-zA-Z0-9,.]{0,10}$")
        self.cbpi = cbpi
        self.sensor_controller : SensorController = cbpi.sensor
        # register component for http, events
        # In addtion the sub folder static is exposed to access static content via http
        self.cbpi.register(self, "/api/hydrometer/v1/data")
```

```
 
    @request_mapping(path='', method="POST", auth_required=False)
    async def http_new_value3(self, request):
        import time
        """
        ---
        description: Get iSpindle Value
        tags:
        - iSpindle 
        parameters:
        - name: "data"
          in: "body"
          description: "Data"
          required: "name"
          type: "object"
          type: string
        responses:
            "204":
                description: successful operation
        """

        global cache
        try:
            data = await request.json()
        except Exception as e:
            print(e)
        logging.info(data)
        time = time.time()
        key = data['name']
        temp = round(float(data['temperature']), 2)
        angle = data['angle']
        battery = data['battery']
        try:
            rssi = data['RSSI']
        except:
            rssi = 0
        cache[key] = {'Time': time,'Temperature': temp, 'Angle': angle, 'Battery': battery, 'RSSI': rssi}
```


```
def setup(cbpi):
    cbpi.plugin.register("iSpindle", iSpindle)
    cbpi.plugin.register("iSpindleEndpoint", iSpindleEndpoint)
    pass

```