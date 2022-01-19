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

The first function inside the class is the initialization of the sensor. You need to pay attentino the the name of your sensor class does match the name in the super `function`. If you change the name for your sensor class, you also need to change the name insode the `super` fucntion. It is required, to use unique names for your sensor plugins. Defining two different classes with the same name will cause issues.

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

```
    async def run(self):
        while self.running:
            self.value = random.randint(10, 100)
            self.log_data(self.value)

            self.push_update(self.value)
            await asyncio.sleep(1)
```

```
    def get_state(self):
        return dict(value=self.value)
```

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
