# Development

## Running a development version of the server

At least the server and some of the plugins have currently development branches in the GIT repo that can be installed to test new features or bug fixes. The development branch is primarily used to updated the software and run tests, before it is rolled out to the main branch which is more stable.

To install for instance the development branch of the server, you need to run the following command:

```
sudo pip3 install https://github.com/avollkopf/craftbeerpi4/archive/development.zip
```

The only difference to the installation of the master branch is that main.zip is replaced with development.zip. If you want to upgrade from an existing installation, you should add the flag `--upgrade`.

```
sudo pip3 install --upgrade https://github.com/avollkopf/craftbeerpi4/archive/development.zip
```

To revert back to the master branch, just run the commands for [updating your server](server-installation.md#updating-the-server).

## Cloning the server to your local drive and install it from there

For development, it can be also important to have the server code on your local harddrive and install it from there. Therefore, you need to clone the repo in a first step:

```
git clone https://github.com/avollkopf/craftbeerpi4
```

This will pull a local copy of the server software to your harddrive.

Typically, the master branch will be pulled. You can check this with once you navigated into the craftbeerpi4 directory:

```
cd craftbeerpi4
git branch
```

If you want to use the development branch, you need to checkout this branch from within the craftbeerp4 directory:

```
git checkout development
```

Afterwards you can check again which branch is used as describe above.

To install the server now, you have two possibilities. 

1. You can either install it as package.
2. You can install the server for development. This will have the effect, that every change in the code will take effect as soon as you start the server.

Package installation:

```
sudo pip3 install ./craftbeerpi4
```

Development installation:

```
sudo pip3 install -e ./craftbeerpi4
```


## Setting up a virtual environment for development&#x20;

Development of plugins and server is recommende to be done in a virtual environemnt ot on a separate system / sd-card. This allows you to create / modify code and test it without the risk to harm your running system.

{% hint style="info" %} 
How to create virtual env in [Python](https://docs.python.org/3/tutorial/venv.html)
{% endhint %}

If you choose to follow the path of the virtual enviroment, you need to ensure that all relevant python packages for the venv are installen and then you need to run the following commands to activate your venv.

```
python3 -m venv venv
source venv/bin/activate
```

Now you can either install the server as described in this manual inside you virtual environment or you can clone the server to your local system and install it from there.

{% hint style="info" %} 
Inside your virtual environment you need to  install the server or anything else without sudo!
{% endhint %}

## Creating new Plugins

CraftbeerPi4 has the capability to create a plugin template for the development of your own plugin. You just need to run the following command:

```
cbpi create {PLUGINNAME}
```

This will create a folder with a template for your plugin and configures ith with your PLUGINNAME

If you run for instance

```
cbpi create cbpi4-testplugin
```

A Plugin with the name cbpi4-testplugin will be created and you will see the following output:

```

Plugin cbpi4-testplugin created! See https://craftbeerpi.gitbook.io/craftbeerpi4/development how to run your plugin

Happy Development! Cheers

```

### Plugin folder structure

CraftbeeerPi4 is creating a folder with the name cbpi4-testplugin which has the follwoing structure:

```
pi@raspberrypi:~ $ cd cbpi4-testplugin/
pi@raspberrypi:~/cbpi4-testplugin $ ll
insgesamt 52
drwxr-xr-x 3 pi pi  4096 17. Jan 11:47 cbpi4-testplugin
-rw-r--r-- 1 pi pi 35149 17. Jan 11:47 LICENSE
-rw-r--r-- 1 pi pi    80 17. Jan 11:47 MANIFEST.in
-rw-r--r-- 1 pi pi    30 17. Jan 11:47 README.md
-rw-r--r-- 1 pi pi   446 17. Jan 11:47 setup.py
```

The main folder conatins the LICENSE file and a MANIFEST file where you do not need to change anything. The README.md file will be seen if you upload your plugin to a github repo. This file should be edited with the required information on how to install, configure and use your plugin.

The sub-folder `cbpi4-testplugin` ist the folder, where the plugin code is located.


#### Plugin setup.py file

The setup.py file contains some information for the plugin installation such as version number and other required packages. You need to edit this file accoridingly. Additional information on how to use setuptools can be found [here](https://docs.python.org/3.9/distutils/setupscript.html).

The setup.py file looks like this:

```
from setuptools import setup

setup(name='cbpi4-testplugin',
      version='0.0.1',
      description='CraftBeerPi Plugin',
      author='',
      author_email='',
      url='',
      include_package_data=True,
      package_data={
        # If any package contains *.txt or *.rst files, include them:
      '': ['*.txt', '*.rst', '*.yaml'],
      'cbpi4-testplugin': ['*','*.txt', '*.rst', '*.yaml']},
      packages=['cbpi4-testplugin'],
     )
```

You should change the version number whenever you modify/update your plugin. You should also enter some information in the description (e.g. Sensor Plugin). As author you can enter your name and in the email your email if you want. The URL should be filled with the homepage of your plugin which is typically the url of your github repo. 

You can also specify required python packages that will be installed during the installation of your plugin. In addition, it is recommended to andd a few lines that will help to display the README.md file also on the pypi.org page if you decide to create also a pacage for pypi.org that can be installed later just via:

```
sudo pip3 install cbpi4-testplugin
```

Below is an example of a setup.py for a [plugin](https://pypi.org/project/cbpi4-scd30-CO2-Sensor/), which is also available on the pypi.org page and can be installed directly from there.


```
from setuptools import setup

# read the contents of your README file
from os import path
this_directory = path.abspath(path.dirname(__file__))
with open(path.join(this_directory, 'README.md'), encoding='utf-8') as f:
    long_description = f.read()

setup(name='cbpi4-scd30_CO2_Sensor',
      version='0.0.3',
      description='CraftBeerPi4 Plugin for SCD30 based CO2 Sensor',
      author='Alexander Vollkopf',
      author_email='avollkopf@web.de',
      url='https://github.com/avollkopf/cbpi4-scd30-co2-sensor',
      license='GPLv3',
      include_package_data=True,
      package_data={
        # If any package contains *.txt or *.rst files, include them:
      '': ['*.txt', '*.rst', '*.yaml'],
      'cbpi4-scd30_CO2_Sensor': ['*','*.txt', '*.rst', '*.yaml']},
      packages=['cbpi4-scd30_CO2_Sensor'],
        install_requires=[
        'cbpi>=4.0.0.33',
        'smbus2',
        'scd30_i2c',
  ],
  long_description=long_description,
  long_description_content_type='text/markdown'

     )
```

One section has been added to read the content of the readme file and provied it at the end of the setup.py as 'long_description'. There is also a parameter 'install_requires' which can be filled with additional packages that are required for the plugin. This plugin requires for instance in addition to cbpi the packages 'smbus2' and 'scd30_i2c'.

#### Creation of plugin package and upload to pypi.org

Once you have written your plugin and uploaded it to your github repo, you can also create a package on the pypi.org page. Therefore, you need to create an acoount on pypi.org and install the [twine package](https://twine.readthedocs.io/en/stable/).

In the main folder of your plugin you need to run the following command to create a distribution package:

```
sudo python setup.py sdist
```

Once the apckage is created, you can upload it with twine:

```
sudo twine upload dist/*
```

### Plugin code folder

If you change into the sub folder of your main plugin directory, you can edit the plugin code. The code folder structre looks like this:

```
pi@raspberrypi:~/cbpi4-testplugin $ cd cbpi4-testplugin/
pi@raspberrypi:~/cbpi4-testplugin/cbpi4-testplugin $ ll
insgesamt 12
-rw-r--r-- 1 pi pi   33 17. Jan 11:47 config.yaml
-rw-r--r-- 1 pi pi 1881 17. Jan 11:47 __init__.py
drwxr-xr-x 2 pi pi 4096 17. Jan 11:47 static
```

The config.yaml is created automatically and should not be changed. The static folder is typically not required and can be removed in most of the cases. The `__init__.py` file contains the code of your plugin.

### Plugin Types

CraftbeerPi4 provides flexibility as Plgins can be written for different purposes. The following plugin types are currently supported:

| Type                    | Description                                                               | Example |
| ------------------------ | ------------------------------------------------------------------------ |-------- |
| Sensor                   | Allows addition of differnet sensor types besies onwore temp sensor      | <p>[cbpi4-pt100x](https://github.com/avollkopf/cbpi4-pt100x)</p><p>[cbpi4-iSpindle](https://github.com/avollkopf/cbpi4-iSpindle)</p> |
| Actor                    | Allows addition of different actor types / hardware                      | <p>[cbpi4-GroupedActor](https://github.com/avollkopf/cbpi4-GroupedActor)</p><p>[cbpi-PCF8574-GPIO](https://github.com/avollkopf/cbpi4-PCF8574-GPIO)</p> |
| Kettle Logic             | Allows to create a kettle logic that fits for your system requirements   | [cbpi4-PIDBoil](https://github.com/avollkopf/cbpi4-PIDBoil) |
| Fermenter Logic          | Allows to create a fermenter logic that fits for your system requirements | <p>No example yet</p><p>Builtin FermenterHysteresis</p> |
| Mash Steps               | Allows to add mash steps that users may require / adapt to their needs   | [cbpi4-BM_Steps](https://github.com/avollkopf/cbpi4-BM_Steps) |
| Notifications            | Allows users to define callback functions that forward cbpi notifications to other message services or a buzzer | <p>[cbpi4-Pushover](https://github.com/avollkopf/cbpi4-PushOver)</p><p>[cbpi4-buzzer](https://github.com/avollkopf/cbpi4-buzzer)</p> |
| Automated Recipe Creation | Allows users to adapt the automated recipe creation process (xml, kbh or brewfather) to their requirements. | [cbpi4-RecipeImport](https://github.com/avollkopf/cbpi4-RecipeImport)|
| Other Functions           | Allows users to add other functionalities such as an LCD Display        | [cbpi4-LCDisplay](https://github.com/avollkopf/cbpi4-LCDisplay) |

### Plugin Classes

CraftbeerPi4 has defined different classes, that you need to create a Sensor, Actor, Logic,.....

The following table will describe show you the clasess.

| Class                 | Properties |  Usage                                                                                                 |
| --------------------- | ---------- |------------------------------------------------------------------------------------------------------ |
| CBPiSensor            | Yes | Required for all type of sensors. For some functions the class CBPiExtension might be required in addition | 
| CBPiActor             | Yes | Required for all type of actors. For some functions the class CBPiExtension might be required in addition |
| CBPiKettleLogic       | Yes | Required to create a new Kettle Logic type                                                             |
| CBPiFermenterLogic    | Yes | Required to create a new Fermenter Logic type                                                             |
| CBPiStep              | Yes | Required to create new MashSteps                                                                       |
| CBPiExtension         | No | Required for various purposes such as addition of cbpi setting prameters, definition of http endpoints or startup of additional hardware |

### Plugin Properties

If you want to add for instance a new onewire sensor, you need to select the sensor id, define a name, an Interval and an offset, if required. This is done via so called properties.

![Empty Mash Profile window](../.gitbook/assets/cbpi4-plugin-properties-example.png)

The Sensor Name and Sensor Type will be always required, even if you don't add properties to your plugin. However, the other porperties need to be added via  @parameters right in front of your class. Properties can be added to all plugins except for the CBPiExtension.

```
@parameters([Property.Select(label="Sensor", options=getSensors()), 
             Property.Number(label="offset",configurable = True, default_value = 0, description="Sensor Offset (Default is 0)"),
             Property.Select(label="Interval", options=[1,5,10,30,60], description="Interval in Seconds")])
class OneWire(CBPiSensor):
```

If you don't want to add properties for your sensor leave the @ parameters empty:

```
@parameters([])
class YourNewSensor(CBPiSensor):
```

The following properties are available:

| Property            | Description                                                         |
| ------------------- | ------------------------------------------------------------------- |
| Property.Select     | The user can select some pre-defined options                        |
| Property.Number     | The user can enter a number (e.g. offset value for a sensor)        |
| Property.Text       | The user can enter a string / text                                  |
| Property.Sensor     | The user can select a sensor that should be used for the plugin     |
| Property.Actor      | The user can select an actor that should be used in the plugin (e.g. in a step) |
| Property.Kettle     | The user can select a kettle that should be used in the plugin (e.g. in a step) |
| Property.Fermenter  | The user can select a femrenter that should be used in the plugin   |

{% hint style="info" %} 
Some properties will be used as default for the different plugin classes. 
- A Kettle will for instance always require a kettle logic, a sensor, a heater (actor) and an agotator (actor).
- A Fermenter will always require a logic, a heater, a cooler, a sensor, a brewname and a target temp.

- Only the properties you want to ise in addition have to be specified in the @parameters.
{% endhint %}

