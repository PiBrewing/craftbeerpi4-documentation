## Examples for sensor plugins

I want to show you some examples on how to write a sensor plugin. I will start with the example of the dummysensor which is included in the server itself and is very simple.

### Dummy sensor example

First, you will need to import the python packages you require for your plugin. As plenty of functions need to be processed asynchronus, you will always need to import asyncio. You will also need to import the CBPiSensor from the cbpi api.

```
# -*- coding: utf-8 -*-
import asyncio
import random

from cbpi.api import parameters, CBPiSensor
```

As already described in the [plugin properties section](development.md#-plugin-properties)

```
@parameters([])
class CustomSensor(CBPiSensor):

    def __init__(self, cbpi, id, props):
        super(CustomSensor, self).__init__(cbpi, id, props)
        self.value = 0
```

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


