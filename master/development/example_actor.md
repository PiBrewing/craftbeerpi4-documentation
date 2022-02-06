# Actor Plugin Example

In this section, I want to show you some examples on how to write an actor plugin. I will start with the example of the  GPIO actor which is included in the server itself and is very simple.

## GPIO Actor example

As already described for the sensor plugin, you need to import the required packages at the beginning of yot plugin. For this particular actor, thr `RPi.GPIO`package is required, which is only available on a Raspberry Pi. To avoid issues, if the system is installed on a different platform, functions of the unittest package are loaded that will emulate the `RPi.GPIO` as test. This will prevent errors, althoug an error message will be shown when the plugin is started.

```
import asyncio
import logging
from unittest.mock import MagicMock, patch

from cbpi.api import *


logger = logging.getLogger(__name__)

try:
    import RPi.GPIO as GPIO
except Exception:
    logger.error("Failed to load RPi.GPIO. Using Mock")
    MockRPi = MagicMock()
    modules = {
        "RPi": MockRPi,
        "RPi.GPIO": MockRPi.GPIO
    }
    patcher = patch.dict("sys.modules", modules)
    patcher.start()
    import RPi.GPIO as GPIO
```

CraftbeerPi is using the `BCM` mode and this needs to be set when activating the `RPi.GPIO` package.

```
mode = GPIO.getmode()
if (mode == None):
    GPIO.setmode(GPIO.BCM)
```

Right before your Actor class, you need to specify the plugin/actor properties such as GPIO pin or if the actor should be low or high on active (inverted). These properties  can be selected or changed for each instance of an actor. 

```
@parameters([Property.Select(label="GPIO", options=[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27]), 
             Property.Select(label="Inverted", options=["Yes", "No"],description="No: Active on high; Yes: Active on low"),
             Property.Select(label="SamplingTime", options=[2,5],description="Time in seconds for power base interval (Default:5)")])

```

Then you start the Actor class itself

```
class GPIOActor(CBPiActor):
```

For actors, but also for Sensors you have the possibility to specify actions that can be triggered from the dashboard. This is done via `@action("{NAME}", parameters=[...])`. Parameters are specified in the same way as you do it for the plugin parameters. The example below shows you an example on how to set the poer for an actor.

This is followed by the corresponding fucntion that shall be triggered by the action. The parameters are directly assigned via the `label` of the action prameter (e.g. `Power` like in this example). In this case, the parameter power will be defined by the action and the action will call the `self.set_power()` function and will change the power for the actor

```
    # Custom property which can be configured by the user
    @action("Set Power", parameters=[Property.Number(label="Power", configurable=True,description="Power Setting [0-100]")])
    async def setpower(self,Power = 100 ,**kwargs):
        self.power=int(Power)
        if self.power < 0:
            self.power = 0
        if self.power > 100:
            self.power = 100           
        await self.set_power(self.power)      
```

For some actors (e.g. relaisboards) it is important to specify, if the actor is active on high or low (inverted). With such a fucntion, you can invert the state of the GPIO.

```
    def get_GPIO_state(self, state):
        # ON
        if state == 1:
            return 1 if self.inverted == False else 0
        # OFF
        if state == 0:
            return 0 if self.inverted == False else 1

```

All Actor plugins require the `on_start` fucntion to define initial variables for the actor instance. When the actor instance is starting, you collect the required information from the parameter definition and you set the GPIO as output and set the GPIO / actor to `off` with `GPIO.output(self.gpio, self.get_GPIO_state(0))`. The `get_GPIO_state` function returns a `0` or `1` which depends on the `inverted` setting. The `self.state` is set to `False` on start which. All other fnctions in the server can retrieve that status of the actor with the get_state function, which returns the state. 

```
    async def on_start(self):
        self.power = None
        self.gpio = self.props.GPIO
        self.inverted = True if self.props.get("Inverted", "No") == "Yes" else False
        self.sampleTime = int(self.props.get("SamplingTime", 5)) 
        GPIO.setup(self.gpio, GPIO.OUT)
        GPIO.output(self.gpio, self.get_GPIO_state(0))
        self.state = False
```

The `on` function is also required. It defines, what is done to switch the actor on. Even if the actor has no possibility to vary the power, the `power=None`is required in the definition of this function. If the actor has not the possibility to specify the power, you don't need the code with respect to the power setting and you don't need to call the `self.set_power` function. Afterwards, the GPIO is set to on (status depending on the inverrted setting) and the `self.state` is set to `True` to let the system know, that the actor is on. The actor will now run with the specified power or with 100%.

```
    async def on(self, power = None):
        if power is not None:
            self.power = power
        else: 
            self.power = 100
        await self.set_power(self.power)

        logger.info("ACTOR %s ON - GPIO %s " %  (self.id, self.gpio))
        GPIO.output(self.gpio, self.get_GPIO_state(1))  
        self.state = True

```

You will also need to specify the `off` function to switch the actor off. Power settings are not required for this function. It isl also essential to set the state of the actor to `False`.

```
    async def off(self):
        logger.info("ACTOR %s OFF - GPIO %s " % (self.id, self.gpio))
        GPIO.output(self.gpio, self.get_GPIO_state(0))
        self.state = False
```

As mentioned before, server functions need to read the state of the actor. Therefore, the `get_state` function is required for all actors.

```
    def get_state(self):
        return self.state
```

The run fucntion is also required for all actors. It can be more or less empty and contain just the `while self.running == True:` loop with an `await asyncio.sleep(1)`. However, this function is used here for proportional heating to emulate PWM. One could also call it "poor man's PWM". In the parameter section, the user could select the parameter `SamplingTime` which defines the the the time for one heat cycle. If the time is for instance 5 seconds and the power is set to 60% the function will switch the heater on for 3 seconds and turns it off for 2 seconds. These times vary with the power setting. The `run` function is continuously running, while the actor is available in the system. The proportional heating is done, as long as `self.state = True`.

```
    async def run(self):
        while self.running == True:
            if self.state == True:
                heating_time=self.sampleTime * (self.power / 100)
                wait_time=self.sampleTime - heating_time
                if heating_time > 0:
                    #logging.info("Heating Time: {}".format(heating_time))
                    GPIO.output(self.gpio, self.get_GPIO_state(1))
                    await asyncio.sleep(heating_time)
                if wait_time > 0:
                    #logging.info("Wait Time: {}".format(wait_time))
                    GPIO.output(self.gpio, self.get_GPIO_state(0))
                    await asyncio.sleep(wait_time)
            else:
                await asyncio.sleep(1)
```

The `set_power()`is also essential for each actor. The set power function just sets the `self.power` parameter and sends an update to the websoket and via mqtt with `await self.cbpi.actor.actor_update(self.id,power)`. However, if your actor has no power capabilities, you can leave the function basically empty and just leave the `pass` in.

```
    async def set_power(self, power):
        self.power = power
        await self.cbpi.actor.actor_update(self.id,power)
        pass
```

Finally, you need to register your plugin with the `setup(cbpi)` function.

```
def setup(cbpi):
    '''
    This method is called by the server during startup 
    Here you need to register your plugins at the server  
    :param cbpi: the cbpi core 
    :return: 
    '''
    cbpi.plugin.register("GPIOActor", GPIOActor)
```
